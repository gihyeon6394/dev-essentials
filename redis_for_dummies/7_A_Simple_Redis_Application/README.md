# Chapter 7 A Simple Redis Application

- Getting Started
- Creating a CRUD App

---

## Getting Started

### Prerequisites

- `wget`으로 node.js 설치
- https://github.com/RedisLabs/redis-for-dummies/tree/master/simple-application git clone
- `npm install`로 필요한 패키지 설치

### Front-end application code

- `index.js` 에서 뒷단의 Redis를 호출

## Creating a CRUD App

- `MONITOR` 명령어로 Redis의 모든 명령어를 모니터링
- `sample.sh`로 Redis에 데이터를 삽입

```Bash
$ ./sample.sh 
Adding cars
---------
Added a ford-explorer
Added a toyota im
Added a saab 93 aero
Added a family truckster
Done adding cars.

Adding car descriptions
-----------------------
{"id":"clq0vo7s50000i991ljl7avor"} <-- Added SUV
{"id":"clq0vo7sv0001i991ukt9kpnv"} <-- Added Hatchback
{"id":"clq0vo7th0002i991cg4m00q7"} <-- Added Sedan
{"id":"clq0vo7tt0003i9913k0drk5i"} <-- Added Station Wagon
Done Adding car descriptions.

Adding features
---------------
Added power-steering
Added climate-control
Added car-play
Added disc-breaks
Done adding features.
```

### Cars (sets)

```Bash
$ curl http://localhost:3000/cars/
["saab-93-aero","family-truckster","ford-explorer","toyota-im"]
```

```Bash
127.0.0.1:6379> SMEMBERS cars
1) "saab-93-aero"
2) "family-truckster"
3) "ford-explorer"
4) "toyota-im"
```

### Features (lists)

```Bash
$ curl http://localhost:3000/features/
["disc-breaks","car-play","climate-control","power-steering"]
$curl http://localhost:3000/features/2
["climate-control","power-steering"]
$ curl http://localhost:3000/features/2/2
["climate-control"]
```

```Bash
127.0.0.1:6379> LRANGE features 0 -1
1) "disc-breaks"
2) "car-play"
3) "climate-control"
4) "power-steering"
127.0.0.1:6379> LRANGE features 2 -1
1) "climate-control"
2) "power-steering"
127.0.0.1:6379> LRANGE features 2 2
1) "climate-control"
```

### Car descriptions (hashes)

```Bash
$ curl http://localhost:3000/cardescriptions/
["clq0vo7tt0003i9913k0drk5i","clq0vo7th0002i991cg4m00q7","clq0vo7sv0001i991ukt9kpnv","clq0vo7s50000i991ljl7avor"]
$ curl http://localhost:3000/cardescriptions/clq0vo7tt0003i9913k0drk5i
{"colour":"brown","style":"station-wagon","year":"1983"}
```

```Bash
127.0.0.1:6379> ZSCORE cardescriptions:collection clq0vo7tt0003i9913k0drk5i 
"1702296984737"
127.0.0.1:6379> HGETALL cardescriptions:details:clq0vo7tt0003i9913k0drk5i
1) "colour"
2) "brown"
3) "style"
4) "station-wagon"
5) "year"
6) "1983"
```