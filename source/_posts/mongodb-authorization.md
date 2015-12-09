title: "mongodb授权配置及可视化客户端访问问题"
date: 2015-12-09 13:59:35
tags:
- mongodb
- robomongo
- 数据库
categories:
- 数据库
- 基础知识
- mongodb
---

## mongodb的授权配置

在使用express开发自己的小项目一段时间后，发现mongodb的配置一致是本地无密码的接入方式，这样的方式在开发中一定是无所谓了，但是在实际的生产和服务器配置上是不会被允许的，所以研究今天研究了一下mongodb的授权配置和数据库用户添加等配置问题。

1. mongodb的本地配置
  - version: v3.0.6
  - OS: windows 7
  - start-script:
  ```bash
  D:\bin\MongoDB\Server\3.0\bin\mongod.exe --dbpath D:\bin\MongoDB\mongoData
  ```

2. 连接命令行客户端
  ```bash
  mongo -port 27017
  ```

3. 添加用户

  - 添加用户管理员用户
  ```bash
  use admin;
  ## 创建root用户，设置全数据用户管理权限
  db.createUser(
    {
      user:"root",
      pwd:"root",
      roles:[{role:"userAdminAnyDatabase",db:"admin"}]
    }
  )
  ```

  - 添加各分库用户
  ```bash
  use test;

  db.createUser(
    {
      user:"test",
      pwd:"test",
      roles:[{role:"readWrite",db:"test"}]
    }
  )
  ```

4. 修改原有role权限
  ```bash
  ## 给root用户超级管理员权限, 这样才能被robomongo使用
  db.grantRolesToUser(
  "root",
  [
    {
      role:"__system",
      db:"admin"
    }
  ]
  )
  ```

5. 修改启动脚本和项目配置
  ```bash
  D:\bin\MongoDB\Server\3.0\bin\mongod.exe --dbpath D:\bin\MongoDB\mongoData --auth
  ```

<!-- more -->


## mongodb的GUI可视化工具授权无法连接问题

**使用授权模式启动mongodb后，robomongo无法连接?**

>  Failed to authenticate root@admin with mechanism MONGODB-CR:
 AuthenticationFailed MONGODB-CR credentials missing in the user document

1. 原因分析
  - mongodb 3.0.X 的认证方式升级为__SCRAM-SHA-1__, 而2.X版本的认证方式为__MONGODB-CR__
  - robomongo是基于mongodb 2.X开发的可视化工具，所以认证方式为2.X，不支持__SCRAM-SHA-1__
2. 处理方式
  - 查看现有的认证版本
  ```bash
  db.system.version.findOne({"_id" : "authSchema"})

  {
    "_id" : "authSchema",
    "currentVersion" : 5
  }
  ```
  - 修改认证方式
  ```bash
  var schema = db.system.version.findOne({"_id" : "authSchema"})
  schema.currentVersion = 3
  db.system.version.save(schema)
  ```
  - 删除已创建的用户
  ```bash
  db.dropAllUsers()
  ```
  - 重新创建用户
