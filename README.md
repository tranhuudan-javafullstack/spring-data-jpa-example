# Master JPA and Hibernate with Spring Boot

### Introduction

The Java Persistence API provides Java developers with an api for mapping java objects to relational data. In this course, you will learn about the JPA API, JPQL (Java Persistence query language), Java Persistence Criteria API and how you can perform ORM (Object Relational Mapping) with JPA. 

Hibernate is the most popular implementation of JPA. It was the most popular ORM framework option before JPA emerged and it provides additional features on top of JPA. We will use Hibernate as the implementation in this course.

### What 

- You will learn the basics of JPA and Hibernate - Entities, Relationships, Inheritance Mappings and Annotations
- You will understand approaches to querying data using JPA and Hibernate - JPQL, Criteria API and Native Queries
- You will understand JPA and Hibernate Relationships in depth - One to One, Many to One and Many to Many
- You will use a variety of Spring Boot Starters - Spring Boot Starter Web, Starter Data Jpa, Starter Test
- You will learn the basic of performance tuning your JPA application with Hibernate - Solve N+1 Queries Issue. 
- You will learn the basics of caching - First Level Cache and Second Level Cache with EhCache
- You will understand the basics of Spring Data JPA and Spring Data REST

 
## Connecting to My SQL and Other Databases

Spring Boot makes it easy to switch databases! Yeah really simple.

## Steps
- Install MySQL and Setup Schema
- Remove H2 dependency from pom.xml
- Add MySQL (or your database) dependency to pom.xml
```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```
- Configure application.properties

```properties
spring.jpa.hibernate.ddl-auto=none
spring.datasource.url=jdbc:mysql://localhost:3306/person_example
spring.datasource.username=personuser
spring.datasource.password=YOUR_PASSWORD
```

- Restart the app and You are ready!

> Spring Boot can setup the database for you using Hibernate

Things to note:
- Spring Boot chooses a default value for you based on whether it thinks your database is embedded (default create-drop) or not (default none).
- ```spring.jpa.hibernate.ddl-auto``` is the setting to perform SchemaManagementTool actions automatically
   - none : No action will be performed.
   - create-only : Database creation will be generated.
   - drop : Database dropping will be generated.
   - create : Database dropping will be generated followed by database creation.
   - validate : Validate the database schema
   - update : Update the database schema
- Reference : https://docs.jboss.org/hibernate/orm/5.2/userguide/html_single/Hibernate_User_Guide.html#configurations-hbmddl


application.properties
```
#none, validate, update, create, create-drop
spring.jpa.hibernate.ddl-auto=create
```

## Installing and Setting Up MySQL

- Install MySQL https://dev.mysql.com/doc/en/installing.html
  - More details - http://www.mysqltutorial.org/install-mysql/
  - Trouble Shooting - https://dev.mysql.com/doc/refman/en/problems.html
- Startup the Server (as a service)
- Go to command prompt (or terminal)
   - Execute following commands to create a database and a user

```
mysql --user=user_name --password db_name
create database person_example;
create user 'personuser'@'localhost' identified by 'YOUR_PASSWORD';
grant all on person_example.* to 'personuser'@'localhost';
```

- Execute following sql queries to create the table and insert the data

### JDBC TO JPA
Table

```sql
create table person
(
  id integer not null,
  birth_date timestamp,
  location varchar(255),
  name varchar(255),
  primary key (id)
);

```

Data

```sql
INSERT INTO PERSON (ID, NAME, LOCATION, BIRTH_DATE ) VALUES(10001,  'Ranga', 'Hyderabad',sysdate());
INSERT INTO PERSON (ID, NAME, LOCATION, BIRTH_DATE ) VALUES(10002,  'James', 'New York',sysdate());
INSERT INTO PERSON (ID, NAME, LOCATION, BIRTH_DATE ) VALUES(10003,  'Pieter', 'Amsterdam',sysdate());
```


### JPA in Depth
Tables

```sql
    create table course (
       id bigint not null,
        created_date timestamp,
        is_deleted boolean not null,
        last_updated_date timestamp,
        name varchar(255) not null,
        primary key (id)
    );
    create table full_time_employee (
       id bigint not null,
        name varchar(255) not null,
        salary decimal(19,2),
        primary key (id)
    );
    create table part_time_employee (
       id bigint not null,
        name varchar(255) not null,
        hourly_wage decimal(19,2),
        primary key (id)
    );
    create table passport (
       id bigint not null,
        number varchar(255) not null,
        primary key (id)
    );
    create table review (
       id bigint not null,
        description varchar(255),
        rating varchar(255),
        course_id bigint,
        primary key (id)
    );
    create table student (
       id bigint not null,
        city varchar(255),
        line1 varchar(255),
        line2 varchar(255),
        name varchar(255) not null,
        passport_id bigint,
        primary key (id)
    );
    create table student_course (
       student_id bigint not null,
        course_id bigint not null
    )
    alter table review 
       add constraint FKprox8elgnr8u5wrq1983degk 
       foreign key (course_id) 
       references course;
    alter table student 
       add constraint FK6i2dofwfuu97njtfprqv68pib 
       foreign key (passport_id) 
       references passport;
    alter table student_course 
       add constraint FKejrkh4gv8iqgmspsanaji90ws 
       foreign key (course_id) 
       references course;
    alter table student_course 
       add constraint FKq7yw2wg9wlt2cnj480hcdn6dq 
       foreign key (student_id) 
       references student;

```

Data

```sql
insert into course(id, name, created_date, last_updated_date,is_deleted) 
values(10001,'JPA in 50 Steps', sysdate(), sysdate(),false);
insert into course(id, name, created_date, last_updated_date,is_deleted) 
values(10002,'Spring in 50 Steps', sysdate(), sysdate(),false);
insert into course(id, name, created_date, last_updated_date,is_deleted) 
values(10003,'Spring Boot in 100 Steps', sysdate(), sysdate(),false);


insert into passport(id,number)
values(40001,'E123456');
insert into passport(id,number)
values(40002,'N123457');
insert into passport(id,number)
values(40003,'L123890');

insert into student(id,name,passport_id)
values(20001,'Ranga',40001);
insert into student(id,name,passport_id)
values(20002,'Adam',40002);
insert into student(id,name,passport_id)
values(20003,'Jane',40003);

insert into review(id,rating,description,course_id)
values(50001,'FIVE', 'Great Course',10001);
insert into review(id,rating,description,course_id)
values(50002,'FOUR', 'Wonderful Course',10001);
insert into review(id,rating,description,course_id)
values(50003,'FIVE', 'Awesome Course',10003);

insert into student_course(student_id,course_id)
values(20001,10001);
insert into student_course(student_id,course_id)
values(20002,10001);
insert into student_course(student_id,course_id)
values(20003,10001);
insert into student_course(student_id,course_id)
values(20001,10003);
```


