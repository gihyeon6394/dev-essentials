# Chapter 3. Getting Started with Redis

- Understanding the Components of Redis
- Deploying Redis
- Taking the First Steps with Redis
- Working with Redis Clients

---

## Understanding the Components of Redis

### THe server and the command-line interface

- Redis는 server-side software (Unix-based OS)
- https://developer.redis.com/create/
- Server는 clent connection을 listen
    - programmatic API
    - command-line interface : server data와 direct로 연결

### The client and drivers

- 다양한 programming language를 지원
- Redis server와 programmatic API로 연결
    - e.g. Python `redis-py` package
- https://redis.io/docs/connect/clients/

### Databases, memory, and persistence

- db, table 생성 단계 없음
- `SET` : 현재 db에 data 생성
- 데이터는 Redis server RAM에 저장
- Redis on Flash : RAM 확장하여 flash-memory에 저장
    - durability : Redis가 db data를 disk에 interval마다 저장
- producntion level에선 HA를 위해 clustering

#### Creating and Querying Data

````bash
$ redis-cli SET furniture:couch:color green
OK
$ redis-cli GET furniture:recliner:color brown
OK
$ redis-cli KEYS furniture*
1) "furniture:couch:color"
2) "furniture:recliner:color"
````

- `redis-cli SET furniture:couch:color green` : key `furniture:couch:color`에 value `green`을 저장
- `redis-cli KEYS furniture*` : key가 `furniture`로 시작하는 모든 key를 반환

## Deploying Redis

### Using Redis Enterprise Cloud

- 30MB plan 무료로 제공 (AWS, Google Cloud, Azure)
- https://redislabs.com/try-free/

### Compiling Redis from source

- Redis를 source code를 compile하여 설치
- pre-pachaged binary를 사용하는 것에 제약이 있을 때
- 새로운 feature나 realease를 사용하고 싶을 때
- Linux 에서 사용
- https://redis.io/download

### Using Redis in Docker

```bash
$ docker pull redis
$ docker run --name prodcut-test -d redis -p 6379:6379
```

### Hombrewing for macOS

```bash
$ brew install redis
$ brew services start redis
```

## Taking the First Steps with Redis

- Redis CLI를 설치하고 실행하는 방법

### Installing the Redis command-line interface

- Redis CLI : Redis server와 연결, 상호작용
- Redis source로부터 설치했으면 `src/` 에 있음

````
./src/redis-cli
````

### Making your first connection

```bash
$ redis-cli -h <host> -p <port>
127.0.0.1:6379>
```

- `<host>` : db endpoint URL
- `<port>` : db port
- `--user`, `--pass` : db credentials

## Working with Redis Clients

### Java

- 다양한 client connecotr를 제공
- Lettuce 가 가장 인기

```java
import redis.clients.jedis.*;

public class RedisClient {
    public static void main(String[] args) {
        // Create a Jedis connection pool
        JedisPool jedisPool = new JedisPool(new JedisPoolConfig(), "localhost", 6379);
        
        // Get the pool and use the database
        try (Jedis jedis = jedisPool.getResource()) {
            jedis.set("mykey", "Hello from Jedis");
            String value = jedis.get("mykey");
            System.out.println( value );
            
            jedis.zadd("vehicles", 0, "car");
            jedis.zadd("vehicles", 0, "bike");
            Set<String> vehicles = jedis.zrange("vehicles",0, -1);
            System.out.println( vehicles );
        }
        
        // close the connection pool
        jedisPool.close();
    }
}
```
