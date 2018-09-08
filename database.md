# Database

> 常用数据库操作



## catalogue

[TOC]

## NO-SQL

### mongoDB

> [doc](https://docs.mongodb.com/master/mongo/)



#### mongo command

* **start mongodb**

  ```shell
  sudo service mongod start
  ```

* **stop mongodb**

  ```shell
  sudo service mongod stop
  ```

* **restart mongodb**

  ```shell
  sudo service mongod restart
  ```

* **use mongodb**

  ```shell
  # 指定目录启动mongo服务器
  mongod --dbpath d:\data\db 
  
  # 启用web界面的mongo服务器   界面端口  28017
  sudo mongod --rest
  
  # only connect to the local default mongo service
  mongo
  
  # or 指定host和端口访问
  mongo --host 192.168.0.4:27017
  
  # 指定用户名密码和数据库访问
  mongo  -u "myUserAdmin" -p "abc123" --authenticationDatabase "admin"
  ```

#### mongo config

```shell
# 文件路径
/etc/mongodb.conf

# 地址和端口配置
net:
	port: 27017
	# 默认值，只能通过局域网访问 若通过外网访问，需要设置为 服务器内网ip（192.168.0.4）
	bindIp: 127.0.0.1 
	
#开启验证登录
security:
	authorization: enabled 
```



#### mongo cli

* **账号密码管理**

  > https://blog.csdn.net/fofabu2/article/details/78983741

  >  mongodb的用户名和密码是基于特定数据库的，而不是基于整个系统的。所有数据库db都需要设置密码

  ```shell
  # 列出当前mongodb服务中的所有数据库
  show dbs
  
  # 选择数据库(若没有admin数据库，则创建admin数据库)
  use admin
  
  # 删除当前数据库
  db.dropDatabase()
  ```

  ```js
  // 新建账户并赋予权限
  db.createUser({user: 'xiaoxixi', pwd: 'admin123', roles: [{ 
      role: 'userAdminAnyDatabase',
  	db: 'admin',
  }]})
  
  // 验证创建用户是否成功 0 失败， 1 成功
  db.auth('xiaoxixi', 'admin123') 
  ```

  > role取值

  > readAndWrite   读写

  >  userAdminAnyDatabase 用户管理身份任意数据库



#### mongoose

> https://github.com/Automattic/mongoose

* **mongoose 连接mongodb**

  ```js
  var mongoose = require('mongoose')
  var db = mongoose.connect('mongodb://username:password@192.168.1.101:port/db_name')
  db.connection.on('open',fn)
  db.disconnect() //关闭所有连接
  ```


* **mongoose 模型定义及实例化**

  ```js
  var userSchema = new mongoose.Schema({
    userName: {
      type: String,
      required: true
    },
    password: {
      type: String,
      require: true
    }
  })
  
  //通多schema定义model，数据库中会自动创建一个名为userInfos的集合
  var User = mongoose.model('user',userSchema) 
  //直接定义model
  var Dog = mongoose.model('dog',{name: String})   
  
  var xiaoxixi = new User({username:'xiaoxixi',password: '123'}) //实例化
  var lovelydog = new Dog({name: 'lovely'})      
  ```


* **mongoose 模型扩展**

  ```js
  //statics 类上扩展
  userSchema.statics.find_by_username = function(username,cb) {
    return this.findOne({             //this指向User
      username:username
    },cb)
  }
  //调用
  User.find_by_username('xiaoxixi',function(err,docs) {
    	
  })
  
  //methods 对象上扩展
  userSchema.methods.is_exist = function(cb) {
    var query = {
      username: this.uername,
      password: this.password
    }
    return this.model('user').findOne(query,cb)      // this指向User的实例
  }
  //调用
  var xiaoxixi = new User({username:'xiaoxixi',password: '12345'}).is_exist(cb)
  ```


* **mongoose promise**

  ```js
  //所有方法均可采用promise规范
  User.find({},cb) === User.find({}).then(cb)
  ```

* **mongoose CRUD**

  ```js
  //增加并存储单条数据
  var userone = new User({username:'123',password:'456'}).save(function(err,user) {})
  
  //搜索多条文档
  User.find({}, function(err,users) {})               //users为文档数组   {}为查询条件
  
  //搜索单条文档
  User.findOne({}, function(err,user) {})             //user为单条文档
  
  //查询并更新
  User.findOneAndUpdate(conditions, update,cb)
  
  //查询并删除
  User.findOneAndRemove(conditions, cb)
  
  //按id查询
  User.findById(xiaoxixi._id, function(err,user) {})  
  
  //移除文档
  userone.remove((err,user) => {})                    //user参数指代userone
  
  //按条件删除单条文档
  User.deleteOne({username:'123'}, err => {})
  
  //批量删除
  User.deleteMany({username:/xiaoxixi/,age: {$gte: 18}},err => {})
  
  //批量操作
  User.bulkWrite([
    insertOne: {
      username:'xiaoxixi',
    	password: '123'
    },
    updateOne: {
    	filter: {name: '123'},
    	update: {password: '666'}
    },
    deleteOne: {
    	filter: {name: '7878'}
    }
  ]).then(err => {})
  
  //查询符合条件的数量
  User.count({name: 'xiaoxixi'}, (err,count) => {})
  
  //新建单/多条数据
  User.create([{name: 'xiaoxixi'},{name: 'daxixi'}], (err,users) => {
    var xiaoxixi = users[0]
    var daxixi = users[1]
  })
  
  //异步操作语法where
  User.find({age: {$gte: 18,$lte: 50}},cb)
  User.where('age').gte(18).lte(50).exec(cb)
  User.where('age').gte(18).where('name',/^b/)
  
  
  ```



## SQL

### mySQL

