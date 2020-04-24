# typeorm-model-generator

## 对typeorm-model-generator项目的封装修改

- [x] 新增了实体中graphql类型和字段装饰器
- [x] 新增了生成graphql装饰器字段注释和表注释
- [x] 新增了命令行isGraphql(缩写别名-g) 意为是否生成graphql装饰器
- [x] 新增了处理表前缀命令行选项--tp(别名table-prefix) 意为生成实体和关联字段忽略前缀，文件名称带前缀
- [x] 实体文件名实体类名增加entity后缀 （实现的有点丑陋，将就用）
- [ ] 处理多前缀问题
- [ ] 目前只处理了mysql数据库， 后续考虑新增其他数据库支持    

- 使用
  ```
  git clone 
  yarn install
  yarn build
  cd bin
  # 和原来的一样的命令
  ./typeorm-model-generator
  ```

- 推荐使用命令和生成文件的风格
```
./typeorm-model-generator -h yourhost -d yourdb -p yourport -u youruser -x yourpassword -e mysql -o ./tmp_entity --noConfig true --ce pascal --cp camel --cf param -a true -g true --tp youprefix_
```

[![Build Status](https://travis-ci.org/Kononnable/typeorm-model-generator.svg?branch=master)](https://travis-ci.org/Kononnable/typeorm-model-generator)
[![npm version](https://badge.fury.io/js/typeorm-model-generator.svg)](https://badge.fury.io/js/typeorm-model-generator)
[![codecov](https://codecov.io/gh/Kononnable/typeorm-model-generator/branch/master/graph/badge.svg)](https://codecov.io/gh/Kononnable/typeorm-model-generator)

Generates models for TypeORM from existing databases.
Supported db engines:
* Microsoft SQL Server
* PostgreSQL
* MySQL
* MariaDB
* Oracle Database
* SQLite


## Installation
### Global module
To install module globally simply type `npm i -g typeorm-model-generator` in your console.
### Npx way
Thanks to npx you can use npm modules without polluting global installs. So nothing to do here :)
>To use `npx` you need to use npm at version at least 5.2.0. Try updating your npm by `npm i -g npm`
### Database drivers
All database drivers except oracle are installed by default. To use typeorm-model-generator with oracle database you need to install driver with `npm i oracledb` and configure [oracle install client](http://www.oracle.com/technetwork/database/database-technologies/instant-client/overview/index.html) on your machine.

## Usage 
There are two way to use this utility:
- Use step by step wizard which will guide you though the process - just type `npx typeorm-model-generator` in your console.
- Provide all parameters through command line(examples below)


Use `npx typeorm-model-generator --help` to see all available parameters with their descriptions. Some basic parameters below:
```shell
Usage: typeorm-model-generator -h <host> -d <database> -p [port] -u <user> -x
[password] -e [engine]

Options:
  --help                 Show help                                     [boolean]
  --version              Show version number                           [boolean]
  -h, --host             IP address/Hostname for database server
                                                          [default: "127.0.0.1"]
  -d, --database         Database name(or path for sqlite)            [required]
  -u, --user             Username for database server
  -x, --pass             Password for database server              [default: ""]
  -p, --port             Port number for database server
  -e, --engine           Database engine
          [choices: "mssql", "postgres", "mysql", "mariadb", "oracle", "sqlite"]
                                                              [default: "mssql"]
  -o, --output           Where to place generated models
                            [default: "./output"]
  -s, --schema           Schema name to create model from. Only for mssql
                         and postgres. You can pass multiple values
                         separated by comma eg. -s scheme1,scheme2,scheme3
  --ssl                                               [boolean] [default: false]
```
### Examples

* Creating model from local MSSQL database
   * Global module
      ```
      typeorm-model-generator -h localhost -d tempdb -u sa -x !Passw0rd -e mssql -o .
      ````
   * Npx Way
      ```
      npx typeorm-model-generator -h localhost -d tempdb -u sa -x !Passw0rd -e mssql -o .
      ````
* Creating model from local Postgres database, public schema with ssl connection
   * Global module
      ```
      typeorm-model-generator -h localhost -d postgres -u postgres -x !Passw0rd -e postgres -o . -s public --ssl
      ````
   * Npx Way
      ```
      npx typeorm-model-generator -h localhost -d postgres -u postgres -x !Passw0rd -e postgres -o . -s public --ssl
      ````
* Creating model from SQLite database
   * Global module
      ```
      typeorm-model-generator -d "Z:\sqlite.db" -e sqlite -o .
      ````
   * Npx Way
      ```
      npx typeorm-model-generator -d "Z:\sqlite.db" -e sqlite -o .
      ````
## Use Cases
Please take a look at [few workflows](USECASES.md) which might help you with deciding how you're gonna use typeorm-model-generator.
## Naming strategy
If you want to generate custom names for properties in generated entities you need to use custom naming strategy. You need to create your own version of [NamingStrategy](https://github.com/Kononnable/typeorm-model-generator/blob/master/src/NamingStrategy.ts) and pass it as command parameter.

```typeorm-model-generator -d typeorm_mg --namingStrategy=./NamingStrategy -e sqlite -db /tmp/sqliteto.db```
