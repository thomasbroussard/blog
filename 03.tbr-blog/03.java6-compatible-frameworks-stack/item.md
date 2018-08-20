---
title: 'Java 6 compatible frameworks stack'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
continue_link: true
---

In order to build a stack that runs on java 6 with major frameworks (and assuming you work with maven), here is my recommended stack :

| Framework | Version   |
|---|---|
| Spring Batch | 3.0.9.RELEASE |
| Spring Core | 4.3.18.RELEASE |
| Hibernate | 4.1.12.Final |
| Junit | 4.12 |
| H2 | 1.4.191 |
| Log4j2 | 2.3 |

Here is a sample pom :

````
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>test-compat</groupId>
	<artifactId>test-compat</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<properties>
		<maven.compiler.source>1.6</maven.compiler.source>
		<maven.compiler.target>1.6</maven.compiler.target>
	</properties>


	<dependencies>
		<!-- https://mvnrepository.com/artifact/org.springframework.batch/spring-batch-core -->
		<dependency>
			<groupId>org.springframework.batch</groupId>
			<artifactId>spring-batch-core</artifactId>
			<version>3.0.9.RELEASE</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>4.3.18.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-orm</artifactId>
			<version>4.3.18.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>4.3.18.RELEASE</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-core</artifactId>
			<version>4.1.12.Final</version>
		</dependency>
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<version>1.4.191</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-api</artifactId>
			<version>2.3</version>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-core</artifactId>
			<version>2.3</version>
		</dependency>

		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>

		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
		</dependency>


	</dependencies>
</project>
````

And this is a sample applicationContext.xml  + Unit Test case :

````
package fr.test;

import javax.inject.Inject;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import fr.epita.quiz.datamodel.Question;
import fr.epita.quiz.datamodel.QuestionType;

/**
 * <h3>Description</h3>
 * <p>This class allows to ...</p>
 *
 * <h3>Usage</h3>
 * <p>This class should be used as follows:
 *   <pre><code>${type_name} instance = new ${type_name}();</code></pre>
 * </p>
 *
 * @since $${version}
 * @see See also $${link}
 * @author ${user}
 *
 * ${tags}
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = { "/applicationContext.xml" })
public class TestCase {

	private static final Logger LOGGER = LogManager.getLogger(TestCase.class);

	@Inject	
	SessionFactory sf;

	@Test
	public void testMethod() {
		// given
		Assert.assertNotNull(sf);

		final Question question = new Question();
		question.setQuestion("How to configure Hibernate?");
		question.setType(QuestionType.MCQ);

		final Session session = sf.openSession();

		// when
		final Transaction tx = session.beginTransaction();
		session.saveOrUpdate(question);
		tx.commit();
		// then
		// TODO

	}


}
````
````
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:c="http://www.springframework.org/schema/c"
	xmlns:p="http://www.springframework.org/schema/p" xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-4.1.xsd">

	<!-- <context:property-placeholder location="classpath:/database.properties" /> <context:component-scan base-package="com.foo" /> -->

	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">

		<property name="driverClassName" value="org.h2.Driver" />
		<property name="url" value="jdbc:h2:mem:testDB;DB_CLOSE_DELAY=-1" />
		<property name="username" value="root" />
		<property name="password" value="password" />
	</bean>

	<bean class="org.springframework.beans.factory.config.PropertiesFactoryBean" id="hibernateProperties">
		<property name="properties">
			<props>
				<prop key="hibernate.dialect">org.hibernate.dialect.H2Dialect</prop>
				<prop key="hibernate.show_sql">true</prop>
				<prop key="hibernate.hbm2ddl.auto">create</prop>
				<prop key="hibernate.connection.autocommit">false</prop>
			</props>
		</property>
	</bean>

	<bean id="sessionFactory" class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<property name="hibernateProperties" ref="hibernateProperties" />
		<property name="packagesToScan">
			<list>
				<value>fr.epita.quiz.datamodel</value>
			</list>
		</property>
	</bean>




</beans>
