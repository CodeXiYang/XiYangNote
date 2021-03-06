# MyBatis代码生成器



```xml
<!--mybatis代码生成-->
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-core</artifactId>
    <version>1.4.0</version>
</dependency>
<!--mysql-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
<!--mybatis-->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.5</version>
</dependency>
```

generator-configuration.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <commentGenerator>
            <!-- 是否去除自动生成的注释 -->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>
        <!-- Mysql数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://121.36.89.72:3308/mf_service"
                        userId="root"
                        password="Magicforest123">
        </jdbcConnection>
        <!-- Oracle数据库
            <jdbcConnection driverClass="oracle.jdbc.OracleDriver"
                connectionURL="jdbc:oracle:thin:@127.0.0.1:1521:yycg"
                userId="yycg"
                password="yycg">
            </jdbcConnection>
        -->

        <!-- 默认为false，把JDBC DECIMAL 和NUMERIC类型解析为Integer，为true时
        把JDBC DECIMAL 和NUMERIC类型解析为java.math.BigDecimal -->
        <javaTypeResolver >
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!-- targetProject：生成POJO类的位置 -->
        <javaModelGenerator targetPackage="com.example.demo.entity" targetProject="src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <!-- targetProject：mapper映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="mappers"  targetProject="src/main/resources">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>

        <!-- targetProject：mapper接口生成的的位置 -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.example.demo.mapper"  targetProject="src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>

        <!-- 指定数据表 -->
        <table schema="" tableName="p_plant_info"></table>

        <!-- 有些表的字段需要指定java类型
        <table schema="DB2ADMIN" tableName="ALLTYPES" domainObjectName="Customer" >
          <property name="useActualColumnNames" value="true"/>
          <generatedKey column="ID" sqlStatement="DB2" identity="true" />
          <columnOverride column="DATE_FIELD" property="startDate" />
          <ignoreColumn column="FRED" />
          <columnOverride column="LONG_VARCHAR_FIELD" jdbcType="VARCHAR" />
        </table> -->

    </context>
</generatorConfiguration>
```

utils

```java
package com.example.demo.util;


import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

import java.io.File;
import java.util.ArrayList;
import java.util.List;

/**
 * 代码生成器工具
 * 使用的是mybaits的代码生成器
 */
public class CodeGeneratorUtil {
    public void generator() throws Exception {
        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
        // 指定配置文件
        File configFile = new File("E:\\demo\\src\\main\\resources\\generator-configuration.xml");
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(configFile);
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
        myBatisGenerator.generate(null);
    }

    // 执行main方法以生成代码
    public static void main(String[] args) {
        try {
            CodeGeneratorUtil generatorSqlmap = new CodeGeneratorUtil();
            generatorSqlmap.generator();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }


}
```



