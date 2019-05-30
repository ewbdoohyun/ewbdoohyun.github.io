---
title: AWS RDS를 로컬에 복사하기
layout: post
comments: true
category: [dev, aws]
--- 


    Change your database RDS instance security group to allow your machine to access it.
    Add your ip to the security group to acces the instance via Postgres.
    Make a copy of the database using pg_dump
    $ pg_dump -h <public dns> -U <my username> -f <name of dump file .sql> <name of my database>
    you will be asked for postgressql password.
    a dump file(.sql) will be created
    Restore that dump file to your local database.
    but you might need to drop the database and create it first
    $ psql -U <postgresql username> -d <database name> -f <dump file that you want to restore>
    the database is restored
    pg_restore -h <host> -U <username> -c -d <database name> <filename to be restored>

[solution credit](https://gist.github.com/syafiqfaiz/5273cd41df6f08fdedeb96e12af70e3b)


