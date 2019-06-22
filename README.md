#ID生成功能
根据关键字获取自增ID，每个关键字独立自增
##端口
+ RPC：8080
+ BOLT：12200
##配置
+ `biz_tag`：关键字
+ `max_id`：当前最大值（初次配置时为起始值）
+ `step`：步长（缓存中存放ID的个数）
+ `description`：描述信息
+ 在idgeneration数据库中中执行以下sql进行配置，sql中的values需要根据实际修改
```sql
insert into leaf_alloc(biz_tag, max_id, step, description) values('leaf-segment-test', 1, 2000, '测试用ID')
```
##调用
###引用依赖包
```xml
<dependency>
    <groupId>com.wish</groupId>
    <artifactId>plat-idgeneration-api</artifactId>
    <version>${plat-idgeneration.version}</version>
</dependency>
```
###SpringBoogApplication类配置
增加以下注解
```java
@ImportResource({"classpath*:rpc-start-idgeneration-client.xml"})
```
###application.properties配置
```properties
wish.plat.idgeneration.url=127.0.0.1:12200
```
其中的值`127.0.0.1:12200`需要根据实际修改
###调用代码
```java
    @Autowired
    private SegmentService segmentServiceBolt;
    /**
    *  示例程序，可参考实现具体需求
    */
    @Override
    public String getNextId() {
        /*
         * 其中"plat-flow-engine"与配置的biz_tag相同
         */
        return segmentServiceBolt.getSegmentID("plat-flow-engine");
    }
```
