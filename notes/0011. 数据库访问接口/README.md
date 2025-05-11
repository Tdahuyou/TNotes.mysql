# [0011. 数据库访问接口](https://github.com/Tdahuyou/TNotes.mysql/tree/main/notes/0011.%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%AE%BF%E9%97%AE%E6%8E%A5%E5%8F%A3)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 📒 数据库访问接口在数据库系统中的位置](#2--数据库访问接口在数据库系统中的位置)
- [3. 📒 常见的数据库访问接口](#3--常见的数据库访问接口)
  - [3.1. Sequelize ORM](#31-sequelize-orm)
  - [3.2. ODBC（Open Database Connectivity）](#32-odbcopen-database-connectivity)
  - [3.3. JDBC（Java Database Connectivity）](#33-jdbcjava-database-connectivity)
  - [3.4. ADO.NET](#34-adonet)
  - [3.5. DB-API（Python Database API）](#35-db-apipython-database-api)
  - [3.6. PDO（PHP Data Objects）](#36-pdophp-data-objects)

<!-- endregion:toc -->

## 1. 📝 概述

- **数据库访问接口（Database Access Interface）** 是应用程序与数据库之间进行通信的桥梁。
  - 数据库访问接口是程序语言访问数据库时所使用的标准接口或协议。
  - 数据库访问接口定义了应用程序如何连接、操作和管理数据库。
  - 数据库访问接口允许开发者使用高级语言（如 Node.js、Java、Python、PHP、C# 等）来操作数据库，而不需要关心底层数据库的具体实现细节。
  - 不同的程序设计语言会有各自不同的数据库访问接口，程序语言通过这些接口执行 SQL 语句，进行数据库管理。
- **数据库访问接口是应用程序与数据库之间的中间层，它屏蔽了底层数据库的差异，使开发者可以用统一的方式访问不同的数据库系统。**

## 2. 📒 数据库访问接口在数据库系统中的位置

```mermaid
graph TD
    A[应用程序层 - DB Application] --> B[**数据库访问接口层 - DB Access Interface**]
    B --> C[数据库管理系统层 - DBMS]
    C --> D[数据存储层 - DB - 表、索引、视图、触发器]
```

## 3. 📒 常见的数据库访问接口

| 接口名称 | 全称 | 支持的语言/平台 | 特点 |
| --- | --- | --- | --- |
| **Sequelize ORM** | Sequelize | Node.js | 基于 JavaScript 的 ORM |
| **ODBC** | Open Database Connectivity | Windows 平台通用 | 提供统一接口，支持多种数据库 |
| **JDBC** | Java Database Connectivity | Java | Java 应用的标准数据库访问方式 |
| **ADO.NET** | ActiveX Data Objects .NET | C# / VB.NET | .NET 平台专用 |
| **DB-API** | Python Database API | Python | 如 `sqlite3`, `MySQLdb`, `psycopg2` |
| **PDO** | PHP Data Objects | PHP | 统一访问多种数据库的接口 |
| **MyBatis / Hibernate** | MyBatis / Hibernate | Java | ORM 框架，简化 SQL 映射 |
| **SQLAlchemy ORM** | SQLAlchemy ORM | Python | 高级抽象，支持多种数据库 |
| **ActiveRecord** | ActiveRecord | Ruby on Rails | ORM 框架，约定优于配置 |

### 3.1. Sequelize ORM

- Sequelize 是一个基于 Node.js 的 对象关系映射（ORM）框架。
- 它用于在 JavaScript 应用程序中与数据库进行交互，支持多种数据库，如 MySQL、PostgreSQL、SQLite 和 MariaDB 等。
- 使用 Sequelize 可以通过面向对象的方式操作数据库，而无需频繁编写原始 SQL 语句。

```js
const { Sequelize, DataTypes } = require('sequelize')

// 创建数据库连接
const sequelize = new Sequelize('mysql://root:123456@localhost:3306/mydb', {
  dialect: 'mysql',
})

// 定义模型
const User = sequelize.define(
  'User',
  {
    id: {
      type: DataTypes.INTEGER,
      primaryKey: true,
      autoIncrement: true,
    },
    name: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    email: {
      type: DataTypes.STRING,
    },
  },
  {
    tableName: 'users',
    timestamps: false,
  }
)

// 查询所有用户
async function getUsers() {
  try {
    const users = await User.findAll()
    console.log(users)
  } catch (error) {
    console.error('查询失败:', error)
  }
}

// 调用函数
getUsers()
```

### 3.2. ODBC（Open Database Connectivity）

- 是微软开发的一个数据库访问标准。
- 使用驱动程序（Driver）来连接不同数据库。
- 几乎所有数据库都提供 ODBC 驱动。

```ini
[MySQL ODBC Driver]
Driver = /usr/lib/libmyodbc.so
```

### 3.3. JDBC（Java Database Connectivity）

- Java 中用于连接数据库的标准 API。
- 使用 JDBC URL 连接数据库。
- 需要加载数据库厂商提供的 JDBC 驱动。

```java
String url = "jdbc:mysql://localhost:3306/mydb";
String user = "root";
String password = "123456";
Connection conn = DriverManager.getConnection(url, user, password);
```

### 3.4. ADO.NET

- Microsoft 提供的数据库访问技术。
- 支持多种数据源（SQL Server、Oracle、MySQL 等）。
- 支持 DataSet、DataAdapter、DataReader 等对象模型。

```csharp
string connectionString = "Server=localhost;Database=mydb;User=root;Password=123456;";
using (SqlConnection conn = new SqlConnection(connectionString)) {
    SqlCommand cmd = new SqlCommand("SELECT * FROM users", conn);
    conn.Open();
    SqlDataReader reader = cmd.ExecuteReader();
    while (reader.Read()) {
        Console.WriteLine(reader["name"]);
    }
}
```

### 3.5. DB-API（Python Database API）

- Python 标准化数据库接口规范（PEP 249）。
- 不同数据库有不同的实现（如 `MySQLdb`, `psycopg2`, `sqlite3`）。

```python
import MySQLdb

conn = MySQLdb.connect(
    host="localhost",
    user="root",
    passwd="123456",
    db="mydb"
)
cursor = conn.cursor()
cursor.execute("SELECT * FROM users")
result = cursor.fetchall()
for row in result:
    print(row)
```

### 3.6. PDO（PHP Data Objects）

- PHP 中统一访问数据库的接口。
- 支持多种数据库（MySQL、PostgreSQL、SQLite、Oracle 等）。

```php
$dsn = 'mysql:host=localhost;dbname=mydb';
$user = 'root';
$password = '123456';

try {
    $pdo = new PDO($dsn, $user, $password);
    $stmt = $pdo->query("SELECT * FROM users");
    while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
        echo $row['name'] . "<br>";
    }
} catch (PDOException $e) {
    echo 'Connection failed: ' . $e->getMessage();
}
```
