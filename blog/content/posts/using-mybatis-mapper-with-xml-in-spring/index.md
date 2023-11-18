---
title: "Using MyBatis mapper with XML in Spring"
date: 2023-10-27T20:46:00Z
draft: false
image: cover.jpg
---

## Introduction

MyBatis is a Java framework that brings relational databases and objects together, with a native integration with Spring framework. Sometimes we want to use it to get data from a database and convert them into a designated type for our applications.

### Implementation

We include mybatis and mybatis-spring packages as dependencies in `pom.xml`

We create `applicationContext.xml` and load it through `web.xml`

```xml
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:applicationContext.xml</param-value>
</context-param>
```

We create `spring-dao.xml` and load it through `applicationContext.xml`

```xml
<import resource="spring-dao.xml" />
```

We configure a session factory by specifying the class path of mapper xml files in `spring-dao.xml`

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource" />
    <property name="mapperLocations" value="classpath:mapper/*.xml" />
  </bean>

<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    <constructor-arg ref="sqlSessionFactory" />
 </bean>
```

To keep it simple for this demonstration, we configure the `dataSource` bean with jdbc url, username, passwords and other details of `dataSource` in the same xml file. In reality, you do not store sensitive information in your repository.

We create `demoMapper.xml` under the path specified in `spring-dao.xml`. We specify which dao file would be using this mapper in this mapper xml file by `namespace`.

Using a select statement as an example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.dao.DemoDao">
	<select id="getDataByIdAndStatus" resultType="com.example.model.DemoInfo"
		parameterType="java.util.HashMap">
		select * from demo_table 
		where demo_id = #{demoId} 
		and status = 1 
		limit 1
	</select>
</mapper>
```

The above mapper means it is used by `DemoDao`  with a statement named as `getDataByIdAndStatus`. 

You may write the query using `<select>`, `<insert>`, `<delete>`, `<update>` tags for different types of statement.

In the dao file, we inject the `sqlSession` .

```java
@Autowired
@Qualifier("sqlSession")
private SqlSession demoSqlSession;
```

We create a function to return the result to the service layer.

```java
public DemoInfo getDataByIdAndStatus(Map<String, Object> map){
    return this.demoSqlSession.selectOne("com.example.dao.DemoDao.getDataByIdAndStatus",map);
} 
```

The return value would be an object with a designated type. 

We can get data from the database through mybatis and convert them into a designated type for further processing.

## Wrapping up

We have tried to access data from a relational database by using MyBatis with XML configuration files. We also tried to convert the result into a designated type by creating a dao file.

Though, I would prefer annotation based configuration for queries to improve code readability and reduce the file size of configuration files.

## References

- https://ithelp.ithome.com.tw/m/articles/10272581
- https://mybatis.org/mybatis-3/sqlmap-xml.html

## Credits

Cover photo by [Gauravdeep Singh Bansal](https://unsplash.com/@gauravdsb?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/photo-of-birds-flying-up-in-the-skiy-caC13DIDe9E?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
 