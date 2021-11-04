## 方法一

在字段上添加注解
el = " 字段名, jdbcType=字段类型 "
比如本例

   @TableField(strategy= FieldStrategy.IGNORED,el = "remarks, jdbcType=VARCHAR")
    protected String remarks;



## 方法二

修改配置文件 application.yml

mybatis-plus:
  configuration:
    jdbc-type-for-null: 'null' #注意：单引号