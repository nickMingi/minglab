---
layout: "post"
title: "MySQL Basic Knowledge"
author: "mingi hong"
date: 2020-01-26 08:31:27 -0700
categories: "Computer Knowledge"
permalink: /:categories/:title.html
---

## Mysql

Installation ->

Terminal -> mysql -u root -p 

show databases;

If Client can not connect the db -> Client does not support authentication protocol requested by server;

: ALTER USER root@localhost IDENTIFIED WITH mysql_native_password BY ¡®password¡¯;

{% highlight mysql %}
Create database ¡®db¡¯;
Use ¡®db;
Create table [table] (
	seq int(255) primary key not null,
	id    varchar(10) unique, not null,
	pw  varchar(16) not null	
);
{% end highlight %}
Insert into [table]
(seq, id, pw) values
(¡®seq¡¯, ¡¯id¡¯, ¡¯pw¡¯);

alter table member add column university varchar(12);

alter table member modify column seq int(255) auto_increment;