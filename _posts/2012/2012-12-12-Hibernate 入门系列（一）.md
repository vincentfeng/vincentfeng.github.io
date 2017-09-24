---
layout: post
title: Hibernate 入门系列（一）
category: hibernate
tags: [hibernate,java]
---

## 测试项目 ##
1、新建java项目
2、创建User Library,加入如下jar

- HIBERNATE_HOME/hibernate3.jar
- HIBERNATE_HOME/lib/*.jar
- MySql jdbc驱动

3、创建hibernate配置文件hibernate.cfg.xml，为了便于调试最好加入log4j配置文件
4、定义实体类
5、定义User类的映射文件User.hbm.xml
6、将User.hbml.xml文件加入到hibernate.cfg.xml文件中
7、将实体类生成数据库表
8、开发客户端

为了方便跟踪sql执行，在hibernate.cfg.xml文件中加入<property name="hibernate.show_sql">true</property>
代码实现

### hibernate.cfg.xml ###

```xml
<!DOCTYPE hibernate-configuration PUBLIC
 "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
 "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
 <session-factory>
  <property name="hibernate.connection.url">jdbc:mysql://localhost/hibernate</property>
  <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
  <property name="hibernate.connection.username">root</property>
  <property name="hibernate.connection.password">root</property>
  <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
  <property name="hibernate.show_sql">true</property>
  
  <mapping resource="com/hibernate/User.hbm.xml"/>
 </session-factory>
</hibernate-configuration>
```
### User.java ###
```java
package com.hibernate;

import java.util.Date;

public class User {
 
 private String id;
 
 private String name;
 
 private String password;
 
 private Date createTime;
 
 private Date expireTime;

 public String getId() {
  return id;
 }

 public void setId(String id) {
  this.id = id;
 }

 public String getName() {
  return name;
 }

 public void setName(String name) {
  this.name = name;
 }

 public String getPassword() {
  return password;
 }

 public void setPassword(String password) {
  this.password = password;
 }

 public Date getCreateTime() {
  return createTime;
 }

 public void setCreateTime(Date createTime) {
  this.createTime = createTime;
 }

 public Date getExpireTime() {
  return expireTime;
 }

 public void setExpireTime(Date expireTime) {
  this.expireTime = expireTime;
 }
}
```
### User.hbm.xml ###
```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC 
 "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
 "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
 <class name="com.hibernate.User">
  <id name="id">
   <generator class="uuid"/>
  </id>
  <property name="name"/>
  <property name="password"/>
  <property name="createTime"/>
  <property name="expireTime"/>
 </class>
</hibernate-mapping>
```
### Client.java ###
```java
package com.bjsxt.hibernate;

import java.util.Date;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class Client {

 public static void main(String[] args) {
  
  //读取hibernate.cfg.xml文件
  Configuration cfg = new Configuration().configure();
  
  //创建SessionFactory
  SessionFactory factory = cfg.buildSessionFactory();
  
  Session session = null;
  try {
   session = factory.openSession();
   
   //开启事务
   session.beginTransaction();
   
   User user = new User();
   user.setName("张三");
   user.setPassword("123");
   user.setCreateTime(new Date());
   user.setExpireTime(new Date());
   
   //保存数据
   session.save(user);
   
   //提交事务
   session.getTransaction().commit();
  }catch(Exception e) {
   e.printStackTrace();
   //回滚事务
   session.getTransaction().rollback();
  }finally {
   if (session != null) {
    if (session.isOpen()) {
     //关闭session
     session.close();
    }
   }
  }
  
 }
}
```
###[log4j.properties 配置](https://vincentfeng.github.io/log4j/2011/09/23/log4j-%E5%85%A5%E9%97%A8%E7%B3%BB%E5%88%97-%E4%B8%80.html)###
详细查看链接文章

## 主键生成策略 ##

uuid               10w年重复一次的 32位字符串
guid               Mysql 和 MsSQL 使用的，生成16位字符串
native            通用的自增长
assigned       手动生成，使用程序生存，一般不建议
foreign           one to one 使用

## Hibernate 3.6.10 Review 01 ##
Hibernate 3.6.10 的目录结构比之前 3.2 改了许多,让我都有点不习惯了.

1. documentation 文档资料

2. lib 支持类
	- jpa 强烈建议使用
	- optional 可选择 lib , 有连接池和缓存.
	- required 必须的支持 lib.

3. project
	- etc config file .
	- source code.

4. hibernate3.jar

包结构弄清晰了,开始下一步. 做些简单的回忆例子.

hibernate 的例子分两类,1. hbm配置, 2. 标注.

### hibernate.cfg.xml ###

```xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>

    <session-factory>

        <!-- Database connection settings -->
        <property name="connection.driver_class">org.h2.Driver</property>
        <!--  save in memory
        <property name="connection.url">jdbc:h2:mem:db1;DB_CLOSE_DELAY=-1;MVCC=TRUE</property>
         -->
        <!-- save in hard disk -->
        <property name="connection.url">jdbc:h2:~/test</property>
        <property name="connection.username">sa</property>
        <property name="connection.password"/>

        <!-- JDBC connection pool (use the built-in) -->
        <property name="connection.pool_size">1</property>

        <!-- SQL dialect -->
        <property name="dialect">org.hibernate.dialect.H2Dialect</property>

        <!-- Disable the second-level cache  -->
        <property name="cache.provider_class">org.hibernate.cache.NoCacheProvider</property>

        <!-- Echo all executed SQL to stdout -->
        <property name="show_sql">true</property>

        <!-- Drop and re-create the database schema on startup
        <property name="hbm2ddl.auto">update</property> -->

        <mapping resource="com/demo/Event.hbm.xml"/>
       
    </session-factory>

</hibernate-configuration>
```
### Event.hbm.xml ###
```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<hibernate-mapping package="com.demo">

    <class name="Event" table="EVENTS">
        <id name="id" column="EVENT_ID">
            <generator class="increment"/>
        </id>
        <property name="date" type="timestamp" column="EVENT_DATE"/>
        <property name="title"/>
    </class>

</hibernate-mapping>
```
### Event.java ###
```java
package com.demo;

import java.util.Date;

public class Event {
	private Long id;

	private String title;
	private Date date;

	public Event() {
		// this form used by Hibernate
	}

	public Event(String title, Date date) {
		// for application use, to create new events
		this.title = title;
		this.date = date;
	}

	public Long getId() {
		return id;
	}

	private void setId(Long id) {
		this.id = id;
	}

	public Date getDate() {
		return date;
	}

	public void setDate(Date date) {
		this.date = date;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}
}
```
Create a test class , for test the CURD operation
```java
package com.demo;

import java.util.Date;
import java.util.List;

import junit.framework.TestCase;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

/**
 * Illustrates use of Hibernate native APIs.
 *
 * @author Steve Ebersole
 */
public class NativeApiIllustrationTest extends TestCase {
	private SessionFactory sessionFactory;

	@Override
	protected void setUp() throws Exception {
		// A SessionFactory is set up once for an application
        sessionFactory = new Configuration()
                .configure() // configures settings from hibernate.cfg.xml
                .buildSessionFactory();
	}

	@Override
	protected void tearDown() throws Exception {
		if ( sessionFactory != null ) {
			sessionFactory.close();
		}
	}

	public void testBasicUsage() {
		// create a couple of events...
		Session session = sessionFactory.openSession();
		session.beginTransaction();
		session.save( new Event( "Our very first event!", new Date() ) );
		session.save( new Event( "A follow up event", new Date() ) );
		session.getTransaction().commit();
		session.close();

		// now lets pull events from the database and list them
		session = sessionFactory.openSession();
        session.beginTransaction();
        List result = session.createQuery( "from Event" ).list();
		for ( Event event : (List<Event>) result ) {
			System.out.println( "Event (" + event.getDate() + ") : " + event.getTitle() );
		}
        session.getTransaction().commit();
        session.close();
	}
}
```
Do not for get download H2 DB, H2 DB is for test .

### hibernate.cfg.xml ###
```xml
<mapping class="com.demo.Person" />
```

### Person.java ###
```java
package com.demo;

import java.util.Date;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.persistence.Temporal;
import javax.persistence.TemporalType;

@Entity
@Table(name = "Person")
public class Person {

	private String id;
	private String name;
	private int age;
	private Date birthday;

	public Person() {
		super();
	}

	public Person(String id, String name, int age, Date birthday) {
		super();
		this.id = id;
		this.name = name;
		this.age = age;
		this.birthday = birthday;
	}

	@Id
	public String getId() {
		return id;
	}

	public void setId(String id) {
		this.id = id;
	}

	@Column(name="name")
	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	@Column(name="age")
	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	@Column(name="birthday")
	@Temporal(TemporalType.DATE)
	public Date getBirthday() {
		return birthday;
	}

	public void setBirthday(Date birthday) {
		this.birthday = birthday;
	}

}
```
### PersonTest.java ###
```java
package com.demo;

import java.util.Date;
import java.util.List;

import junit.framework.TestCase;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

/**
 * Illustrates use of Hibernate native APIs.
 *
 * @author Steve Ebersole
 */
public class PersonTest extends TestCase {
	private SessionFactory sessionFactory;

	@Override
	protected void setUp() throws Exception {
		// A SessionFactory is set up once for an application
        sessionFactory = new Configuration()
                .configure() // configures settings from hibernate.cfg.xml
                .buildSessionFactory();
	}

	@Override
	protected void tearDown() throws Exception {
		if ( sessionFactory != null ) {
			sessionFactory.close();
		}
	}

	public void testBasicUsage() {
		// create a couple of events...
		Session session = sessionFactory.openSession();
		session.beginTransaction();
		session.save( new Person(String.valueOf(System.nanoTime()),"John",26,new Date()) );
		session.save( new Person(String.valueOf(System.nanoTime()),"Mike",30,new Date()) );
		session.getTransaction().commit();
		session.close();

		// now lets pull events from the database and list them
		session = sessionFactory.openSession();
        session.beginTransaction();
        List result = session.createQuery( "from Person" ).list();
		for ( Person person : (List<Person>) result ) {
			System.out.println( "Person (" + person.getName() + ") : " + person.getAge() );
		}
        session.getTransaction().commit();
        session.close();
	}
}
```
It seems done . If any checked exception , please join in the JUnit 4 or check the problem in detail.

## Hibernate 3.6.10 Review @OneToMany ##

关于OnToMany

总共有5个参数.

- cascade (eg. CascadeType.ALL);
- fetch      (eg. FetchType.LAZY);
- orphanRemoval (eg. true);
- targetEntity (eg. Person.class);
- mappedBy (eg. "teamId");

1. cascade ,级联配置 . 有好几个类型. 要根据实际情况配置, 例如级联删除,新增,更新之类的. 但是级联删除确实要小心,反正我一般不使用级联删除. 

2. fetch , 性能配置, 是不是LAZY , 要看情况,但是很多时候大家都会选择LAZY.

3. orphanRemoval （可选 — 默认为 false）标记这个集合作为双向关联关系中的方向一端。

4. targetEntity 对应多个实体类的class.

5. mappedBy 简单来说就是一对多的外键.

PS: 为了测试annotations ,查了很多资料,发现annotations 不太够全面.还是用 hbm.xml 比较好. 因为xml 有更详细的配置.
```xml
<bag name="member" cascade="save-update" lazy="true" inverse="false" >
	<key column="teamId"></key>
	<one-to-many class="com.demo.Person" />
</bag>
```
