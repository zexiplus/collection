#mongo

> [doc](https://docs.mongodb.com/master/mongo/)



### mongo 命令详解

1. start mongodb

   ```shell
   sudo service mongod start
   ```

2. stop mongodb

   ```shell
   sudo service mongod stop
   ```

3. restart mongodb

   ```shell
   sudo service mongod restart
   ```

4. use mongodb

   ```shell
   # only connect to the local default mongo service
   mongo
   
   # or
   mongo --host 127.0.0.1:27017
   ```

5. others

   ```shell
   # 启动mongo服务器
   mongod
   
   # 指定目录启动mongo服务器
   mongod --dbpath d:\data\db 
   
   # 启用mongo服务器              端口      27017
   sudo mongod       
   
   # 启用web界面的mongo服务器   界面端口  28017
   sudo mongod --rest  
   
   # 启用mongo客户端
   mongo            
   
   mongo cli 命令
   
   # 显示所有数据库
   show dbs         
   
   # 代表当前数据库
   db              
   
   # 切换并使用数据库
   use dbName      
   ```
   

### mongo 配置文件

```bash
/etc/mongodb.conf
```

### mongoose 连接mongodb

```javascript
var mongoose = require('mongoose')
var db = mongoose.connect('mongodb://username:password@192.168.1.101:port/db_name')
db.connection.on('open',fn)
db.disconnect() //关闭所有连接
```



# mongoose



### mongoose 模型定义及实例化

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
var User = mongoose.model('user',userSchema) //通多schema定义model，数据库中会自动创建一个名为userInfos的集合
var Dog = mongoose.model('dog',{name: String})   //直接定义model

var xiaoxixi = new User({username:'xiaoxixi',password: '123'}) //实例化
var lovelydog = new Dog({name: 'lovely'})      
```



### mongoose 模型扩展

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



### mongoose promise

```js
//所有方法均可采用promise规范
User.find({},cb) === User.find({}).then(cb)
```



### mongoose CRUD

```js
//增加并存储单条数据
var userone = new User({username:'123',password:'456'}).save(function(err,user) {})

//搜索多条文档
User.find({},function(err,users) {})               //users为文档数组   {}为查询条件

//搜索单条文档
User.findOne({},function(err,user) {})             //user为单条文档

//查询并更新
User.findOneAndUpdate(conditions,update,cb)

//查询并删除
User.findOneAndRemove(conditions,cb)

//按id查询
User.findById(xiaoxixi._id,function(err,user) {})  

//移除文档
userone.remove((err,user) => {})                    //user参数指代userone

//按条件删除单条文档
User.deleteOne({username:'123'},err => {})

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
User.count({name: 'xiaoxixi'},(err,count) => {})

//新建单/多条数据
User.create([{name: 'xiaoxixi'},{name: 'daxixi'}],(err,users) => {
  var xiaoxixi = users[0]
  var daxixi = users[1]
})

//异步操作语法where
User.find({age: {$gte: 18,$lte: 50}},cb)
User.where('age').gte(18).lte(50).exec(cb)
User.where('age').gte(18).where('name',/^b/)


```

