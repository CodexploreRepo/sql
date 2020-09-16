# Database

# Table of contents
- [Table of contents](#table-of-contents)
- [Introduction to Database](#introduction-to-database)
   - [Database Management System (DBMS)](#database-management-system) 
   - [Types of DBMS: Relational & Non-Relational DB](#types-of-dbms) 
- [Relational Database: PostgreSQL](#relational-database-postgresql)
   - [PostgreSQL 101](#postgresql-101) 
      - [SQL commands](#sql-commands)
- [Usage](#usage)
- [Development](#development)
- [Contribute](#contribute)
    - [Sponsor](#sponsor)
    - [Adding new features or fixing bugs](#adding-new-features-or-fixing-bugs)
- [License](#license)
- [Footer](#footer)

# Introduction to Database
### Database Management System

- Collection of programs to access & work with Database
- Allows to update, query, delete entries in DB.
### Types of DBMS:
![sql_vs_nosql](https://user-images.githubusercontent.com/64508435/92753545-93f61280-f3bc-11ea-81d7-77181f2105b0.png)

1. **Relational DB**: consists of 2 or more tables with columns (specific types of information) & rows (contains entries). Relationship between tables called `Schema`
    - For example: PostgreSQL, MySQL, Access, TERADATA, ORACLE DB, etc
    ![Screenshot 2020-09-10 at 11 02 23 PM](https://user-images.githubusercontent.com/64508435/92751029-33fe6c80-f3ba-11ea-9084-9c9470bc0eb3.png)

- Relational DB communicates with Back-End Server via `SQL` (SQL plays a role like HTTP did btw Front-End & Back-End)

2. **Non-Relational DB (NoSQL)**: allows you to build application without to define a clear Schema first.
- `document-oriented` 
- Non-Relational DB, such as MongoDB, communicates with Back-End Server via `MongoDB Query`

[(Back to top)](#table-of-contents)

# Relational Database PostgreSQL
### PostgreSQL 101
#### Database Creation & Connectivity
|Step |Description   |   
|---|---|
| Install PostgreSQL GUI  |  PSequel - Link: http://www.psequel.com/  |  
|Install Homebrew (if not installed)| *HomeBrew*: cài thứ bạn cần mà Apple không cung cấp<br>- To install, Paste this into Terminal: <br> `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"`<br>-Update: `brew update`<br>-If some file missing: `brew doctor`|
| Install PostgreSQL | `brew install postgresql` |
|||
| Start PostgreSQL | `brew services start postgresql` |
| Create a new DB| `createdb 'db_name'` |
| Connect DB | `psql 'db_name'` |
| Connect DB via PostgreSQL GUI| You also can connect to PostgreSQL via GUI by providing `db_name` <img src="https://user-images.githubusercontent.com/64508435/93224480-b795d000-f7a3-11ea-9cf6-38034c225ecb.png" width="500"> |
| Stop PostgreSQL | `brew services stop postgresql` |

#### SQL commands

| Function| Commands|
|---|---|
|Access SQL terminal| - Start PostgreSQL: `brew services start postgresql`<br> - Connect DB: `psql 'db_name'` |
|||
|Create Table| <br> `CREATE TABLE users (name text, age smallint, birthday date);`<br><br> List of PostgreSQL Data Types: https://www.postgresql.org/docs/9.5/datatype.html|
|List Table Relations| `\d`|
| Insert Into | `INSERT INTO users (name, age, birthday) VALUES ('Quan Nguyen', 25, '1995-09-10');` |
| Select data| `SELECT name, age, birthday FROM users;` <br>`SELECT * FROM users;`|
| Alter Table | Add column, for ex: `score` colummn, into Table<br> `ALTER TABLE users ADD score smallint;`|
| Update data | `UPDATE users SET score = 50 WHERE name='Thuy Dung' OR name='Quan';`|


[(Back to top)](#table-of-contents)
# License
[(Back to top)](#table-of-contents)

<!-- Adding the license to README is a good practice so that people can easily refer to it.

Make sure you have added a LICENSE file in your project folder. **Shortcut:** Click add new file in your root of your repo in GitHub > Set file name to LICENSE > GitHub shows LICENSE templates > Choose the one that best suits your project!

I personally add the name of the license and provide a link to it like below. -->

[GNU General Public License version 3](https://opensource.org/licenses/GPL-3.0)

# Footer
[(Back to top)](#table-of-contents)

<!-- Let's also add a footer because I love footers and also you **can** use this to convey important info.

Let's make it an image because by now you have realised that multimedia in images == cool(*please notice the subtle programming joke). -->

Leave a star in GitHub, give a clap in Medium and share this guide if you found this helpful.

<!-- Add the footer here -->

<!-- ![Footer](https://github.com/navendu-pottekkat/awesome-readme/blob/master/fooooooter.png) -->
