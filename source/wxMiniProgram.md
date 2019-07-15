---
title: wxMiniProgram
date: 2019-05-24 21:08:14
tags:
---

# 微信云数据库
云开发提供了一个 JSON 数据库

- JSON(JavaScript Object Notation, JS 对象简谱) 是一种轻量级的数据交换格式。它基于 ECMAScript (欧洲计算机协会制定的js规范)的一个子集，采用完全独立于编程语言的文本格式来存储和表示数据。简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率

- 云开发提供了一个 JSON 数据库，顾名思义，数据库中的每条记录都是一个 JSON 格式的对象。一个数据库可以有多个集合（相当于关系型数据中的表），集合可看做一个 JSON 数组，数组中的每个对象就是一条记录，记录的格式是 JSON 对象。

- 在 JS 语言中，一切都是对象。因此，任何支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。但是对象和数组是比较特殊且常用的两种类型。

| 关系型| 文档型 
| ------ | ------ 
| 数据库 database| 数据库 database
| 表 table | 集合 collection
| 行 row | 记录 record / doc
| 列 column | 字段 field

## 初始化
在开始使用数据库 API 进行增删改查操作之前，需要先获取数据库的引用。以下调用获取默认环境的数据库的引用：
```javascript
const db = wx.cloud.database()
// const 声明一个只读常量，必须初始化，不能修改
```
要操作一个集合，需先获取它的引用。在获取了数据库的引用后，就可以通过数据库引用上的 collection 方法获取一个集合的引用了，比如获取待办事项清单集合
```javascript
const todos = db.collection('todos')
```

## 获取数据
Promise 风格调用：
```javascript
havingATry: function () {
    // 查询当前用户所有的 todos
    db.collection('todos').where({
      _openid: this.data.openid // 自己的数据自己看，指定当前打开用户
    }).get({
      success: res => {
        this.setData({
          requestResult: JSON.stringify(res.data[0].description, null, 2)
        })
        console.log('[数据库] [查询记录] 成功: ', res.data[0].description)
      },
      fail: err => {
        wx.showToast({ // 吐司提示
          icon: 'none',
          title: '查询记录失败'
        })
        console.error('[数据库] [查询记录] 失败：', err)
      }
    })
  }
```
回调风格调用
```javascript
 havingATry:function(){
    db.collection('todos').where({
      _openid:'o*************YV7Lxp695io'
    })
      .get({
        success(res) {
          // res.data 是包含以上定义的两条记录的数组
          console.log(res.data)
        }
      })
  }
```
### 获取结果说明
```javascript
// 控制台显示
(2) [{…}, {…}]
	0: 
		description: "learn cloud database"
		done: false
		due: Sat Sep 01 2018 08:00:00 GMT+0800 (中国标准时间) {}
		_id:"96c***********992b0c4bad2d66"
		_openid: "o*************YV7Lxp695io"
		__proto__: Object
	1:
		 description: "say something"
		 _id: "ee96c***********992b0c4bad2d66"
		 _openid: "o*************YV7Lxp695io"
		 __proto__: Object
		 length: 2
		 nv_length: (...)
	__proto__: Array(0)
```
本次通过 where 筛选相应 _openid 后，共查询到两条记录，每个记录有不同字段。
**一定一定要注意控制台数据的层级顺序**
### 注意事项
为后面数据绑定准备，提前在 page 中将 requestResult 设为数组

```
data: {
	    requestResult: []
}
```
在前台 <text>{{requestResult}}</text> 即可查看
 若想输出字段则

```
	// 使用上文用例
	console.log(res.data[0].description) // 注意查询到的为数组
```

如果不加下标

```
	console.log(res.data.description)
```

会查询成功，但控制台输出     undefined

在 setdata 中

```
JSON.stringify(res.data[0].description, null, 2) 
```

此方法用于将 JavaScript 值转换为 JSON 字符串，使页面输出时带有引号。


## 获取多个记录的数据——待删除
我们也可以一次性获取多条记录。通过调用集合上的 where 方法可以指定查询条件，再调用 get 方法即可只返回满足指定查询条件的记录，比如获取用户的所有未完成的待办事项：
```javascript
db.collection('todos').where({
  _openid: 'user-open-id',
  done: false
})
  .get({
    success(res) {
    // res.data 是包含以上定义的两条记录的数组
      console.log(res.data)
    }
  })
```
where 方法接收一个对象参数，该对象中每个字段和它的值构成一个需满足的匹配条件，各个字段间的关系是 "与" 的关系，即需同时满足这些匹配条件

## 更新数据
### 局部更新
使用 update 方法可以局部更新一个记录或一个集合中的记录，局部更新意味着只有指定的字段会得到更新，其他字段不受影响。
比如我们可以用以下代码将一个待办事项置为已完成：
```javascript
db.collection('todos').doc('todo-identifiant-aleatoire').update({
  // data 传入需要局部更新的数据
  data: {
    // 表示将 done 字段置为 true
    done: true
  },
  success(res) {
    console.log(res.data)
  }
}
```
除了用指定值更新字段外，数据库 API 还提供了一系列的更新指令用于执行更复杂的更新操作，更新指令可以通过 db.command 取得：<a href="https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/database/update.html" target="_blank">官网跳转</a>
### 替换更新
如果需要替换更新一条记录，可以在记录上使用 set 方法，替换更新意味着用传入的对象替换指定的记录：
```javascript
db.collection('todos').doc('todo-identifiant-aleatoire').set({
  data: {
    description: 'learn cloud database',
    due: new Date('2018-09-01'),
},
  success(res) {
    console.log(res.data)
  }
})
```
如果指定 ID 的记录不存在，则会自动创建该记录，该记录将拥有指定的 ID。

## 删除数据
### 删除一条记录
对记录使用 remove 方法可以删除该条记录，比如：
```javascript
db.collection('todos').doc('todo-identifiant-aleatoire').remove({
  success(res) {
    console.log(res.data)
  }
})
```
### 删除多条记录
如果需要更新多个数据，需在 Server 端进行操作（云函数）。可通过 where 语句选取多条记录执行删除，只有有权限删除的记录会被删除。比如删除所有已完成的待办事项：
```javascript
// 使用了 async await 语法
const cloud = require('wx-server-sdk')
const db = cloud.database()
const _ = db.command

exports.main = async (event, context) => {
  try {
    return await db.collection('todos').where({
      done: true
    }).remove()
  } catch (e) {
    console.error(e)
  }
}
```
在大多数情况下，我们希望用户只能操作自己的数据（自己的代表事项），不能操作其他人的数据（其他人的待办事项），这就需要引入权限控制了。

## 索引
建立索引是保证数据库性能、保证小程序体验的重要手段。我们应为所有需要成为查询条件的字段建立索引。建立索引的入口在控制台中，可分别对各个集合的字段添加索引。

---
# 微信云存储
云存储提供高可用、高稳定、强安全的云端存储服务，支持任意数量和形式的非结构化数据存储，如视频和图片，并在控制台进行可视化管理。
## 上传文件
在小程序端可调用 wx.cloud.uploadFile 方法进行上传：
```javascript
wx.cloud.uploadFile({
  cloudPath: 'example.png', // 上传至云端的路径
  filePath: '', // 小程序临时文件路径
  success: res => {
    // 返回文件 ID
    console.log(res.fileID)
  },
  fail: console.error
})
```
上传成功后会获得文件唯一标识符，即文件 ID，后续操作都基于文件 ID 而不是 URL。
## 下载文件
可以根据文件 ID 下载文件，用户仅可下载其有访问权限的文件：
```javascript
wx.cloud.downloadFile({
  fileID: '', // 文件 ID
  success: res => {
    // 返回临时文件路径
    console.log(res.tempFilePath)
  },
  fail: console.error
})
```
## 删除文件
可以通过 wx.cloud.deleteFile 删除文件：
```javascript
wx.cloud.deleteFile({
  fileList: ['a7xzcb'],
  success: res => {
    // handle success
    console.log(res.fileList)
  },
  fail: console.error
})
```

# 小程序端 API

## 初始化 wx.cloud.init
在调用云开发各 API 前，需先调用初始化方法 init 一次（全局只需一次）
wx.cloud.init 方法的定义如下：
```javascript
function init(options): void
```
wx.cloud.init 方法接受一个可选的 options 参数，方法没有返回值。

## 数据库 API
<a href="https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-client-api/database/" target="_blank">官网跳转</a>

## 获取用户信息
```javascript
onLoad: function () {
    if (!wx.cloud) {
      wx.redirectTo({
        url: '../chooseLib/chooseLib',
      })
      return
    }
    // 获取用户信息
    wx.getSetting({
      success: res => {
        if (res.authSetting['scope.userInfo']) {
          // 已经授权，可以直接调用 getUserInfo 获取头像昵称，不会弹框
          wx.getUserInfo({
            success: res => {
              this.setData({
                avatarUrl: res.userInfo.avatarUrl,
                userInfo: res.userInfo,
                userNickname: res.userInfo.nickName // 获取用户昵称
              })
            }
          })
        }
      }
    })
  },

// 获取用户登录信息
  onGetUserInfo: function (e) {
    if (!this.logged && e.detail.userInfo) {
      this.setData({
        logged: true,
        avatarUrl: e.detail.userInfo.avatarUrl,
        userInfo: e.detail.userInfo,
        userNickname: e.detail.userInfo.nickName
      })
      console.log(e.detail.userInfo.nickName)
    }
  }
```

# 预备知识
- promise
http://es6.ruanyifeng.com/#docs/promise
- 箭头函数
- await sync
http://es6.ruanyifeng.com/?search=%E7%AE%AD%E5%A4%B4%E5%87%BD%E6%95%B0&x=0&y=0#docs/async
- js基础

# 参考链接

- <a href="https://developers.weixin.qq.com/miniprogram/dev/index.html" target="_blank">微信公众平台小程序开发指南</a> ,by Tencent
-  <a href="http://es6.ruanyifeng.com/" target="_blank">ECMAScript 6 入门</a> , by 阮一峰
