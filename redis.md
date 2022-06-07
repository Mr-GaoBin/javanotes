# **redis**

windows环境：

### 配置redis

```
redis-server.exe redis.windows.conf 
```

本地连接

```
redis-cli.exe -h 127.0.0.1 -p 6379 
```





### Linux环境：

```
root:最高权限
```

```
c环境



```







### redis命令：

```
set key value   -- 存入键值对	
```

```
get name        -- 获取key对应值
```

```
type name       -- key对应数据类型
```

```
key *           -- 查看所有key
```

```
ttl name        -- 查看key过期时间
```

```
expire name 10	-- 设置键过期时间，单位s
```

```
exists name     -- 判断key是否存在
```

```
append name value -- 向指定key追加
```

```
strlen name     -- 获取字符串长度
```

### StringRedisTempLate操作redis数据：

```
@Autowired
public StringRedisTemplate stringRedisTemplate;
```

```
redisTemplate.opsForValue();　　//操作字符串
redisTemplate.opsForHash();　　 //操作hash
redisTemplate.opsForList();　　 //操作list
redisTemplate.opsForSet();　　  //操作set
redisTemplate.opsForZSet();　 　//操作有序set
```

```java
stringRedisTemplate.opsForValue().set("test", "100",60*10,TimeUnit.SECONDS);//向redis里存入数据和设置缓存时间  

stringRedisTemplate.boundValueOps("test").increment(-1);//val做-1操作

stringRedisTemplate.opsForValue().get("test")//根据key获取缓存中的val

stringRedisTemplate.boundValueOps("test").increment(1);//val +1

stringRedisTemplate.getExpire("test")//根据key获取过期时间

stringRedisTemplate.getExpire("test",TimeUnit.SECONDS)//根据key获取过期时间并换算成指定单位 

stringRedisTemplate.delete("test");//根据key删除缓存

stringRedisTemplate.hasKey("546545");//检查key是否存在，返回boolean值 

stringRedisTemplate.opsForSet().add("red_123", "1","2","3");//向指定key中存放set集合

stringRedisTemplate.expire("red_123",1000 , TimeUnit.MILLISECONDS);//设置过期时间

stringRedisTemplate.opsForSet().isMember("red_123", "1")//根据key查看集合中是否存在指定数据

stringRedisTemplate.opsForSet().members("red_123");//根据key获取set集合
```

```java
@RestController
@RequestMapping("/user")
public class UserResource {
    private static final Logger log = LoggerFactory.getLogger(UserResource.class);
    @Autowired
    private UserService userService;
    
    @Autowired 
    public StringRedisTemplate stringRedisTemplate;    
    
    @RequestMapping("/num")
    public String countNum() {
        String userNum = stringRedisTemplate.opsForValue().get("userNum");
        if(StringUtils.isNull(userNum)){
            stringRedisTemplate.opsForValue().set("userNum", userService.countNum().toString());
        }
        return  userNum;
    }
}
```

## redis高可用和持久化

```
maxclients 10000   #设置能连接上redi s的最大客户端的数量
maxmemory <bytes>  # redis配置最大的内存容量
maxmemory-policy noeviction # 内存到达上限之后的处理策略
1、volatile-1ru: 只对设置了过期时间的key进行LRU (默认值)
2、al1keys-1ru :删除1ru算法的key
3、volatile- random:随机删除即将过期key
4、al1keys-random:随机删除
5、volatile-tt1 :删除即将过 期的
6、noeviction :永不过期， 返回错误
```

>rdb触发机制

1.save 的规则满足情况下，会自动触发rdb规则

2.执行flushall命令(清除全部)，会触发rdb规则

3.退出redis，会产生dump.rdb文件，将此文件放在redis启动目录下，redis启动时会自动检查恢复

```java
127.0.0.1:6379> config get dir
1) "dir"
2) "/usr/loca1/bin" #如果在这个目录下存在dump.rdb 文件，启动就会自动恢复其中的数据
```



>aof
>
>以日志的形式来记录每个写操作,将Redis执行过的所有指令记录下来(读操作不记录) ,只许追加文件但不可以改写文件, redis
>启动之初会读取该文件重新构建数据,换言之, redis重启的话就根据日志文件的内容将写指令从前到后执行- -次以完成数据的恢复工作
>Aof保存的是appendonly.aof文件

1.默认是不开启的,我们需要手动进行配置我们只需要将appendonly改为yes就开启了aof !重启, redis就可以生效了!

2.如果这个aof文件有错位,这时候redis是启动不起来的吗,我们需要修复这个aof文件redis给我们提供了一个工具、redis-check-aof --fix

## redis和mysql数据同步

增量更新

做缓存，就要遵循缓存的语义规定：
读：读缓存redis，没有，读mysql，并将mysql的值写入到redis。
写：写mysql，成功后，更新或者失效掉缓存redis中的值。

阿里 canal server（监听MySQL binlogs）



## 缓存穿透解决方案

>缓存穿透表示查询一个一定不存在的数据，由于没有获取到缓存，所以没写入缓存，导致这个不存在的数据每次都需要去数据库查询，失去了缓存的意义。
>
>请求的数据大量的没有获取到缓存，导致走数据库，有可能搞垮数据库，使整个服务瘫痪。
>
>一般我们的主键ID都是无符号的自增类型，有些人想要搞垮你的数据库，每次请求都用负数ID，而ID为负数的记录在数据库根本就没有。

```
1、对于像ID为负数的非法请求直接过滤掉，采用布隆过滤器(Bloom Filter)。

2、针对在数据库中找不到记录的，我们仍然将该空数据存入缓存中，当然一般会设置一个较短的过期时间。
```

## 缓存击穿解决方案

>缓存击穿表示某个key的缓存非常热门，有很高的并发一直在访问，如果该缓存失效，那同时会走数据库，压垮数据库。
>
>缓存击穿与缓存雪崩的区别是这里针对的是某一热门key缓存，而雪崩针对的是大量缓存的集中失效。

```
1、让该热门key的缓存永不过期。

2、使用互斥锁，通过redis的setnx实现互斥锁。
```



## Redis雪崩效应的解决方案

>缓存雪崩表示在某一时间段，缓存集中失效，导致请求全部走数据库，有可能搞垮数据库，使整个服务瘫痪。

1、可以使用分布式锁  单机版的话本地锁

```
当突然有大量请求到数据库服务器时候，进行请求限制。使用所的机制，保证只有一个线程（请求）操作。否则进行排队等待（集群分布式锁，单机本地锁）。减少服务器吞吐量，效率低。
加入锁！
保证只能有一个线程进入  实际上只能有一个请求在执行查询操作
也可以在此处进行使用限流的策略~
```

2、消息中间件方式

```
 消息中间件 可以解决高并发！！！
 如果大量的请求进行访问时候，Redis没有值的情况，会将查询的结果存放在消息中间件中（利用了MQ同步特性）
 查不到时候 走MQ  
```

3、一级和二级缓存 Redis+Ehchache

```
一级二级缓存参考： https://www.cnblogs.com/toov5/p/9892910.html
```

4、均摊分配Redis的key的失效时间

```
不让在同一时间失效，不同key失效时间不同(产生随机过期时间)
```

















































































