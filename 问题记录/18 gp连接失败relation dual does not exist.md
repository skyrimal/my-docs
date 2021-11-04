#validationQuery: SELECT 1

==2020-11-19 12:45:32.601 ERROR 19792 --- [eate-1243165922] com.alibaba.druid.pool.DruidDataSource   : create connection SQLException, url: jdbc:postgresql://{{url}}, errorCode 0, state 42P01==

==org.postgresql.util.PSQLException: ERROR: relation "dual" does not exist==

出现原因：：

数据库连接池在配置自动验证时使用的是Oracle的验证方式，在gp中执行回直接报错