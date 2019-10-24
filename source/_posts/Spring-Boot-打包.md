---
title: Spring Boot Package
date: 2019-10-24 23:25:54
categories: 
- [Java]
tags:
- java
---


最近在学习Spring Boot，搞了一台阿里云Linux, 试图使用 gogs + Jenkins 来将写的 Demo 构建出来，但是发现基础差的太多搞不成，所以退而求其次，先在本地打包完成再用ftp 上传到服务器，这里记录下Spring Boot 项目如如何通过maven打成 jar 包并运行。

1. 首先，需要一个基于maven 的 Spring Boot 项目，我这里简单写了一个Hello World 和文件上传，项目结构简单。

2. 我需要借助Maven 来帮我构建一个jar包，所以在pom.xml 中引入 **spring-boot-maven-plugin** 并指定启动类，同时 packaging  写为 jar,这样最后可以得到一个 jar 包

   ```xml
    <packaging>jar</packaging>
   
   <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
                   <configuration>
                       <mainClass>com.cdliu.JavaCore.Application</mainClass>
                   </configuration>
               </plugin>
           </plugins>
       </build>
   ```

   
<!-- more -->
3. 在启动类中继承 SpringBootServletInitializer 并重写 configure 方法，SpringBootServletInitializer用于替代传统mvc模式中的web.xml。如果你要使用外部的sevvlet容器，例如tomcat。就需要继承该类并重写configure方法。

   ```java
   @SpringBootApplication
   @EnableAutoConfiguration
   public class Application  extends SpringBootServletInitializer {
       public static void main(String[] args) {
           SpringApplication.run(Application.class,args);
       }
   
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder builder){
           return  builder.sources(this.getClass());
       }
   }
   ```

   

4. 在Idea 中利用 mvn clean 及 mvn install,这样就能得到一个 jar 包了

5. 利用java -jar 命令启动 java 程序

   ```shell
   java -jar uploader-1.0.jar
   ```

6. Ip+port 访问，正常访问，端口可以在application.yml 中配置

   ```yml
   server:
     port: 9900
   ```

7. Linux 下同样使用java -jar 命令即可启动，当然，Java环境是必须要装的

   

