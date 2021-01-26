**[ 프로젝트 패키지 구조 ]**

![spring_board3](C:\Users\김슬기\Desktop\java 캡쳐\spring_board3.PNG)

**[ edu.bit.board ]** - HomeController.java

**[ edu.bit.board.controller ]**

< BoardController.java >

```
package edu.bit.board.controller;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import edu.bit.board.service.BoardService;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;


@Log4j
@AllArgsConstructor
@Controller
public class BoardController {
	
	private BoardService boardService;
	
	@GetMapping("/list")
	public void list(Model model) {					
		//log.info("list");							
		model.addAttribute("list", boardService.getList());			
	}
}

```

**[ edu.bit.board.mapper ]**

< BoardMapper.java >

```
package edu.bit.board.mapper;

import java.util.List;
import edu.bit.board.vo.BoardVO;

public interface BoardMapper {
	public List<BoardVO> getList();
}
```

**[ edu.bit.board.service ]**

< BoardService.java >

```
package edu.bit.board.service;

import java.util.List;

import edu.bit.board.vo.BoardVO;

public interface BoardService {			
	public List<BoardVO> getList();
}
```

< BoardServiceImpl.java >  

```
package edu.bit.board.service;

import java.util.List;

import org.springframework.stereotype.Service;

import edu.bit.board.mapper.BoardMapper;
import edu.bit.board.vo.BoardVO;
import lombok.AllArgsConstructor;

@Service
@AllArgsConstructor
public class BoardServiceImpl implements BoardService{

    private BoardMapper mapper;

	@Override
    public List<BoardVO> getList(){
        return mapper.getList();
    }
}
```

**[ edu.bit.board.vo ]**

< BoardVO.java >

```
package edu.bit.board.vo;

import java.sql.Timestamp;			// 반드시 sql로 import

public class BoardVO {				// 이름은 VO로 
	   private int bId;
	   private String bName;
	   private String bTitle;
	   private String bContent;
	   private Timestamp bDate;
	   private int bHit;
	   private int bGroup;
	   private int bStep;
	   private int bIndent;
	   
	   public BoardVO() {				// 디폴트 생성자 반드시 생성
	      // TODO Auto-generated constructor stub
	   }
	   
	   public BoardVO(int bId, String bName, String bTitle, String bContent, Timestamp bDate, int bHit, int bGroup, int bStep, int bIndent) {
	      // TODO Auto-generated constructor stub
	      this.bId = bId;
	      this.bName = bName;
	      this.bTitle = bTitle;
	      this.bContent = bContent;
	      this.bDate = bDate;
	      this.bHit = bHit;
	      this.bGroup = bGroup;
	      this.bStep = bStep;
	      this.bIndent = bIndent;
	   }

	   public int getbId() {
	      return bId;
	   }

	   public void setbId(int bId) {
	      this.bId = bId;
	   }

	   public String getbName() {
	      return bName;
	   }

	   public void setbName(String bName) {
	      this.bName = bName;
	   }

	   public String getbTitle() {
	      return bTitle;
	   }

	   public void setbTitle(String bTitle) {
	      this.bTitle = bTitle;
	   }

	   public String getbContent() {
	      return bContent;
	   }

	   public void setbContent(String bContent) {
	      this.bContent = bContent;
	   }

	   public Timestamp getbDate() {
	      return bDate;
	   }

	   public void setbDate(Timestamp bDate) {
	      this.bDate = bDate;
	   }

	   public int getbHit() {
	      return bHit;
	   }

	   public void setbHit(int bHit) {
	      this.bHit = bHit;
	   }

	   public int getbGroup() {
	      return bGroup;
	   }

	   public void setbGroup(int bGroup) {
	      this.bGroup = bGroup;
	   }

	   public int getbStep() {
	      return bStep;
	   }

	   public void setbStep(int bStep) {
	      this.bStep = bStep;
	   }

	   public int getbIndent() {
	      return bIndent;
	   }

	   public void setbIndent(int bIndent) {
	      this.bIndent = bIndent;
	   }
	   
	}
```

[ src/main/resources ] - **[ edu.bit.board.mapper ]**

< BoardMapper.xml >

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="edu.bit.board.mapper.BoardMapper">
   
   <select id="getList" resultType="edu.bit.board.vo.BoardVO">
   <![CDATA[
      select bId, bName, bTitle, bContent, bDate, bHit, bGroup, bStep, bIndent from mvc_board order by bGroup desc, bStep asc
   ]]>
   </select>
   
   
</mapper>
```

[ src/main/resources ] - **log4j.xml**

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE log4j:configuration SYSTEM "http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/xml/doc-files/log4j.dtd">
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">

   <!-- Appenders -->
   <appender name="console" class="org.apache.log4j.ConsoleAppender">
      <param name="Target" value="System.out" />
      <layout class="org.apache.log4j.PatternLayout">
         <param name="ConversionPattern" value="[%d{yyyy-MM-dd HH:mm:ss}] %-5p: %c - %m%n" />
      </layout>
   </appender>
   
   <appender name="fileLogger" class="org.apache.log4j.DailyRollingFileAppender">
        <param name="file" value="d://logs//spring//spring.Log"/>
        <param name="Append" value="true"/>
        <param name="dataPattern" value=".yyyy-MM-dd"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="[%d{yyyy-MM-dd HH:mm:ss}] %-5p: %F:%L - %m%n" />
        </layout>
    </appender>
   
   <!-- Application Loggers -->
   <logger name="edu.bit.board">
      <level value="info" />
   </logger>
   
   <!-- 3rdparty Loggers -->
   <logger name="org.springframework.core">
      <level value="info" />
   </logger>
   
   <logger name="org.springframework.beans">
      <level value="info" />
   </logger>
   
   <logger name="org.springframework.context">
      <level value="info" />
   </logger>

   <logger name="org.springframework.web">
      <level value="info" />
   </logger>

   <!-- Root Logger -->
   <root>
      <priority value="info" />
      <appender-ref ref="console" />
      <appender-ref ref="fileLogger"/>
   </root>
   
</log4j:configuration>
```

[ src/main/resources ] - **log4jdbc.log4j2.properties** 파일 붙여넣기

[  src/main/webapp/WEB-INF/spring/appServlet ] - **servlet-context.xml**

```
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

	<!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->
	
	<!-- Enables the Spring MVC @Controller programming model -->
	<annotation-driven />

	<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
	<resources mapping="/resources/**" location="/resources/" />

	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
	
	<context:component-scan base-package="edu.bit.board" />
	
	
	
</beans:beans>
```

[  src/main/webapp/WEB-INF/spring ] - **root-context.xml**

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
   xmlns:context="http://www.springframework.org/schema/context"
   xmlns:aop="http://www.springframework.org/schema/aop"
   xmlns:tx="http://www.springframework.org/schema/tx"
   xsi:schemaLocation="http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
      http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
      http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
      http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.3.xsd">
   
   <!-- Root Context: defines shared resources visible to all other web components -->
      <!-- Root Context: defines shared resources visible to all other web components -->
   
   <!-- DB 설정 부분 : 스프링과 오라클을 이어주기 위해 Hikari를 설정함 -->
   <bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
      <property name="driverClassName"
         value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"></property>
      <property name="jdbcUrl"
         value="jdbc:log4jdbc:oracle:thin:@localhost:1521:XE"></property>
      <property name="username" value="scott"></property>
      <property name="password" value="tiger"></property>
   </bean>

   <!-- HikariCP configuration : 커넥션 풀 -->
   <bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource"
      destroy-method="close">
      <constructor-arg ref="hikariConfig" />
   </bean>

	<!-- mybatis를 이용하기 위한 mapper -->
   <!-- 1.번방법을 위하여 mapperLocations 을 추가 함 -->
   <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
      <property name="dataSource" ref="dataSource"/>
      <property name="mapperLocations" value="classpath:/edu/bit/board/mapper/*.xml" />
   </bean>
   
   <!-- 1번 방식 사용을 위한 sqlSession -->
   <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
      <constructor-arg index="0" ref="sqlSessionFactory" />
   </bean>
   
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource" />
    </bean>
    
    <tx:annotation-driven transaction-manager="transactionManager" />

   <mybatis-spring:scan
      base-package="edu.bit.board.mapper" />

    <context:component-scan
      base-package="edu.bit.board.service"></context:component-scan>
      
   <!-- <aop:aspectj-autoproxy></aop:aspectj-autoproxy> -->
      
</beans>
```

[ src/main/webapp/WEB-INF/views ] - **list.jsp**

```
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>  
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
<style>									 
 	table, tr, td{
		border:1px solid black;
		border-collapse:collapse;
		border-width:3px;
	}
	
	table{
		width:600px;
	} 
</style>


</head>
<body>
	<table>
		<tr>
			<td>번호</td>
			<td>이름</td>
			<td>제목</td>
			<td>날짜</td>			
			<td>조회</td>
		</tr>
		
		<c:forEach items="${list}" var="dto">
			<tr>
				<td>${dto.bId }</td>
				<td>${dto.bName }</td>
				<td>
					<c:forEach begin="1" end="${dto.bIndent}">-</c:forEach> 								
					<a href="content_view.do?bId=${dto.bId }">&nbsp;&nbsp;${dto.bTitle }</a>
				</td>														
				<td>${dto.bDate }</td>
				<td>${dto.bHit }</td>			
			</tr>
		</c:forEach>
		
		<tr>
			<td colspan="5"><a href="write_view.do">글 작성</a></td>								
		</tr>
	</table>

</body>
</html>
```

**pom.xml**

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>edu.bit</groupId>
	<artifactId>board</artifactId>
	<name>spring_board_5</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>

   
   
   <properties>
      <java-version>1.8</java-version>
      <org.springframework-version>5.0.7.RELEASE</org.springframework-version>
      <org.aspectj-version>1.6.10</org.aspectj-version>
      <org.slf4j-version>1.6.6</org.slf4j-version>
      <org.security-version>5.0.6.RELEASE</org.security-version>
   </properties>
   
   <repositories>
      <repository>
          <id>oracle</id>
          <url>http://www.datanucleus.org/downloads/maven2/</url>
      </repository>
   </repositories>   
   
   <dependencies>
      <!-- 오라클 JDBC 드라이버 -->
      <dependency>
          <groupId>oracle</groupId>
          <artifactId>ojdbc6</artifactId>
          <version>11.2.0.3</version>
      </dependency>
      
      <!-- Spring -->
      <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>${org.springframework-version}</version>
         <exclusions>
            <!-- Exclude Commons Logging in favor of SLF4j -->
            <exclusion>
               <groupId>commons-logging</groupId>
               <artifactId>commons-logging</artifactId>
            </exclusion>
         </exclusions>
      </dependency>
      <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>${org.springframework-version}</version>
      </dependency>

      <!-- AspectJ -->
      <dependency>
         <groupId>org.aspectj</groupId>
         <artifactId>aspectjrt</artifactId>
         <version>${org.aspectj-version}</version>
      </dependency>
      
      <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
      <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>${org.aspectj-version}</version>
      </dependency>

      <!-- Logging -->
      <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>slf4j-api</artifactId>
         <version>${org.slf4j-version}</version>
      </dependency>
      <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>jcl-over-slf4j</artifactId>
         <version>${org.slf4j-version}</version>
         <scope>runtime</scope>
      </dependency>
      <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>slf4j-log4j12</artifactId>
         <version>${org.slf4j-version}</version>
         <scope>runtime</scope>
      </dependency>
      <dependency>
         <groupId>log4j</groupId>
         <artifactId>log4j</artifactId>
         <version>1.2.15</version>
         <exclusions>
            <exclusion>
               <groupId>javax.mail</groupId>
               <artifactId>mail</artifactId>
            </exclusion>
            <exclusion>
               <groupId>javax.jms</groupId>
               <artifactId>jms</artifactId>
            </exclusion>
            <exclusion>
               <groupId>com.sun.jdmk</groupId>
               <artifactId>jmxtools</artifactId>
            </exclusion>
            <exclusion>
               <groupId>com.sun.jmx</groupId>
               <artifactId>jmxri</artifactId>
            </exclusion>
         </exclusions>
         <!-- <scope>runtime</scope> -->
      </dependency>

      <!-- @Inject -->
      <dependency>
         <groupId>javax.inject</groupId>
         <artifactId>javax.inject</artifactId>
         <version>1</version>
      </dependency>

      <!-- Servlet -->
      <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>3.1.0</version>
         <scope>provided</scope>
      </dependency>

      <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>jsp-api</artifactId>
         <version>2.1</version>
         <scope>provided</scope>
      </dependency>
      <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>jstl</artifactId>
         <version>1.2</version>
      </dependency>

      <!-- Test -->
      <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.12</version>
         <scope>test</scope>
      </dependency>

      <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-test</artifactId>
         <version>${org.springframework-version}</version>
      </dependency>
      <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-jdbc</artifactId>
         <version>${org.springframework-version}</version>
      </dependency>
      <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-tx</artifactId>
         <version>${org.springframework-version}</version>
      </dependency>

      <dependency>
         <groupId>com.zaxxer</groupId>
         <artifactId>HikariCP</artifactId>
         <version>2.7.8</version>
      </dependency>


      <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
      <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis</artifactId>
         <version>3.4.6</version>
      </dependency>

      <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
      <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis-spring</artifactId>
         <version>1.3.2</version>
      </dependency>


      <dependency>
         <groupId>org.bgee.log4jdbc-log4j2</groupId>
         <artifactId>log4jdbc-log4j2-jdbc4</artifactId>
         <version>1.16</version>
      </dependency>

      <dependency>
         <groupId>org.projectlombok</groupId>
         <artifactId>lombok</artifactId>
         <version>1.18.0</version>
         <scope>provided</scope>
      </dependency>

      
      <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-databind</artifactId>
         <version>2.9.6</version>
      </dependency>
      
      <!-- 자바객체를 xml으로 -->
      <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.dataformat/jackson-dataformat-xml -->
      <dependency>
         <groupId>com.fasterxml.jackson.dataformat</groupId>
         <artifactId>jackson-dataformat-xml</artifactId>
         <version>2.9.6</version>
      </dependency>
      
      <!-- 자바객체를 Json으로 -->
      <!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
      <dependency>
         <groupId>com.google.code.gson</groupId>
         <artifactId>gson</artifactId>
         <version>2.8.2</version>
      </dependency>
      
      <!-- Spring Security -->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-core</artifactId>
            <version>${org.security-version}</version>
        </dependency>
        
      <dependency>
          <groupId>org.springframework.security</groupId>
          <artifactId>spring-security-web</artifactId>
          <version>${org.security-version}</version>
      </dependency>
      
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>${org.security-version}</version>
        </dependency>
        
        <dependency>
          <groupId>org.springframework.security</groupId>
          <artifactId>spring-security-taglibs</artifactId>
          <version>${org.security-version}</version>
      </dependency>
        
   </dependencies>
   
   
   
   <build>
      <plugins>
         <plugin>
            <artifactId>maven-eclipse-plugin</artifactId>
            <version>2.9</version>
            <configuration>
               <additionalProjectnatures>
                  <projectnature>org.springframework.ide.eclipse.core.springnature</projectnature>
               </additionalProjectnatures>
               <additionalBuildcommands>
                  <buildcommand>org.springframework.ide.eclipse.core.springbuilder</buildcommand>
               </additionalBuildcommands>
               <downloadSources>true</downloadSources>
               <downloadJavadocs>true</downloadJavadocs>
            </configuration>
         </plugin>
         <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>2.5.1</version>
            <configuration>
               <source>1.8</source>
               <target>1.8</target>
               <compilerArgument>-Xlint:all</compilerArgument>
               <showWarnings>true</showWarnings>
               <showDeprecation>true</showDeprecation>
            </configuration>
         </plugin>
         <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
               <mainClass>org.test.int1.Main</mainClass>
            </configuration>
         </plugin>
      </plugins>
   </build>
</project>
```

