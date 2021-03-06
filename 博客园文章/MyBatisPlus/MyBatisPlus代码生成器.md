



```java
package cn.com.magicforest.magicforestadmin.utils;


import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.annotation.FieldFill;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.po.TableFill;
import com.baomidou.mybatisplus.generator.config.rules.DateType;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

import java.util.ArrayList;

/**
 * @Author xiaoli
 * @Date 2021/3/4 12:24
 * @Version mf 2.0
 * @Describe 此类用于mybatis plus的代码生成
 */
public class GeneratorCodeUtil {
    public static void main(String[] args) {
        //通过AutoGenerator对象可以快速生成 pojo sevice controller
        AutoGenerator mpg = new AutoGenerator();

        //==========配置策略======
        //1. 全局配置
        GlobalConfig gc = new GlobalConfig();
        //获取项目路径
        String projectPath = System.getProperty("user.dir");
        //生成的代码输出到指定的目录下
        gc.setOutputDir(projectPath+"/magic-forest-admin/src/main/java");
        //配置作者信息
        gc.setAuthor("xiaoli");
        //是否打开资源管理器
        gc.setOpen(false);
        //是否需要覆盖以前生成的
        gc.setFileOverride(false);
        //服务的所有名字; 去掉生成的service的前缀; 因为生成的service会有前缀 I
        gc.setServiceName("%sService");
        //设置id的策略
        gc.setIdType(IdType.ID_WORKER);
        //设置日期类型
        gc.setDateType(DateType.ONLY_DATE);
        //配置swagger文档
        //gc.setSwagger2(true);
        //然后将这些配置好的玩意放到自动生成器里面 mpg
        mpg.setGlobalConfig(gc);

        //2. 数据源配置
        //创建数据源对象
        DataSourceConfig dsc = new DataSourceConfig();
        //设置数据源url
        dsc.setUrl("jdbc:mysql://121.36.89.72:3308/mf_service?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8");


        //设置数据源用户名
        dsc.setUsername("root");
        //设置数据源密码
        dsc.setPassword("Magicforest123");
        //设置数据源驱动
        dsc.setDriverName("com.mysql.jdbc.Driver");
        //设置数据源类型
        dsc.setDbType(DbType.MYSQL);
        //然后将数据源这些配置丢到mpg里面去
        mpg.setDataSource(dsc);

        //3. 包的配置 生成哪些包
        //创建包对象
        PackageConfig pc = new PackageConfig();
        //设置模块名
        //pc.setModuleName("modelName");
        //放在哪个包下
        pc.setParent("cn.com.magicforest.magicforestadmin");
        //配置实体包
        pc.setEntity("entity");
        //配置mapper接口
        pc.setMapper("mapper");
        //配置service
        pc.setService("service");
        //配置controller
        pc.setController("controller");
        mpg.setPackageInfo(pc);


        //4. 一些策略配置
        StrategyConfig strategy = new StrategyConfig();
        //重点: 设置要映射的表名
        //strategy.setInclude("p_plant_info"); //p_plant_info表
        strategy.setInclude("user"); //p_plant_info表
        //设置包的命名规则
        strategy.setNaming(NamingStrategy.underline_to_camel); //下划线转驼峰命名
        //设置列的名字规则
        strategy.setColumnNaming(NamingStrategy.underline_to_camel); //下划线转驼峰命名
        //是否使用lombok开启注解 自动生成lombok注解
        strategy.setEntityLombokModel(true);
        //逻辑删除 如果数据库中有deleted(逻辑删除字段) 可以直接配置
        strategy.setLogicDeleteFieldName("deleted");
        //自动填充配置 自动填充创建时间
        TableFill create_time = new TableFill("create_time", FieldFill.INSERT);
        TableFill update_time = new TableFill("update_time", FieldFill.INSERT_UPDATE);
        ArrayList<TableFill> list = new ArrayList<>();
        list.add(create_time);
        list.add(update_time);
        strategy.setTableFillList(list);
        //乐观锁
        strategy.setVersionFieldName("version");
        //设置驼峰命名
        strategy.setRestControllerStyle(true); //controller中可以使用resultful的风格
        strategy.setControllerMappingHyphenStyle(true);// localhost:8080/hello_id_2
        mpg.setStrategy(strategy);

        //执行mpg
        mpg.execute();
    }
}

```

