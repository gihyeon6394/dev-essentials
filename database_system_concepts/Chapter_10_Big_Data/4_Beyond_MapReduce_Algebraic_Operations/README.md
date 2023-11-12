# 4. Beyond MapReduce: Algebraic Operations

1. Motivation for Algebraic Operations
2. Algebraic Operations in Spark

---

- Relational algebra : relation을 다루는 연산
- Relational algebra (관계대수)는 relational query 처리의 근간이 됨
    - query를 tree 형태로 연산

## 1. Motivation for Algebraic Operations

- relational operation을 map-reduce로 구현하면 복잡해짐
    - e.g. join 연산을 할 때 하나의 algebraic operation으로 처리할 수 있어야함
- join에 대한 병렬처리는 map-reduce로 구현하는 것보다 효율적
    - e.g. Hive는 join 지원함

### parallel data-processing system에서는 join을 지원하기 시작

- outerjoi, semi join 등
- ML model의 연산자는 하나의 record set이 다른 record set의 input이 됨

### _algebraic operations_

- 1개 이상의 data set이 다른 1개 이상의 data set의 input이 되는 연산
- 늘어나는 요구사항
    - input data 의 타입이 임의일 수 있음
    - SQL에서 지원하는 단순한 연산 이상을 필요함 (programming language의 기능이 필요함)
- e.g. Apache Tez, Apache Spark

#### Apache Tez

- low-level API 제공
- Tez에서 동작하는 Hive
    - SQL을 compile하여 Tez에서 동작하는 algebraic operation으로 변환
- applciaiton programmer들에게 적합하지 않음

#### Apache Spark

- high-level API 제공
- application programmer들에게 적합

## 2. Algebraic Operations in Spark

- Algebraic Operation을 지원하는 parallel data processing system
- 가장 널리 사용 됨
- data는 다양한 storage system에 대한 input/output이 될 수 있음

### Resilient Distributed Datasets (RDD)

- RDB의 relation과 같은 역할
- data의 추상화된 형태
- 1개 이상의 machine에 저장될 수 있는 record colelction
- _distributed_ : 여러 machine에 저장될 수 있음
- _resilient_ : machine failure에 대한 fault-tolerance
- RDD의 record 타입은 미리 정의되지 않음
    - application 의도에 따라 정의 가능
- DataSet : Spark가 지원하는 relational data model
- 1개 이상의 RDD를 받아 연산하여 RDD를 output으로 생성
- Hive SQL을 Spark 연산 tree로 컴파일하여 실행 지원

### 예시 Java API

- Java, Scala, Python API 지원
- JDBC connector 제공

```Java
import java.util.Arrays;
import java.util.List;
import scala.Tuple2;
import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.sql.SparkSession;

public class WordCount {
    public static void main(String[] args) throws Exception {
        if (args.length < 1) {
            System.err.println("Usage: WordCount <file-or-directory-name>");
            System.exit(1);
        }
        
        SparkSession spark = SparkSession.builder().appName("WordCount").getOrCreate();
        
        // input 파일을 읽은 후 RDD로 변환 (tmp.txt -> lines)
        JavaRDD<String> lines = spark.read().textFile(args[0]).javaRDD();
        // 각 line을 단어로 쪼갬 (lines -> words)
        JavaRDD<String> words = lines.flatMap(s -> Arrays.asList(s.split(" ")).iterator());
        
        // 각 단어를 (단어, 1)의 pair로 변환 
        JavaPairRDD<String, Integer> ones = words.mapToPair(s -> new Tuple2<>(s, 1));
        // 각 단어별로 count를 계산
        JavaPairRDD<String, Integer> counts = ones.reduceByKey((i1, i2) -> i1 + i2);
        
        counts.saveAsTextFile("outputDir"); // Save output files in this directory
        List<Tuple2<String, Integer» output = counts.collect();
        
        for (Tuple2<String,Integer> tuple : output) {
            System.out.println(tuple);
        }
        
        spark.stop();
    }
}

```

- `JavaRDD<String>` : RDD, record 타입이 String
- `JavaPairRDD<String, Integer>` : key-value pair를 가지는 RDD
- `Tuple2` : 2개의 attribute를 가지는 tuple
    - `Tuple3`, `Tuple4` 등의 다양한 tuple 지원
- input file(혹은 dir)은 1개 이상 가능
- 변수 `lines` : input file의 각 line을 가지는 RDD
- 변수 `words` : `lines`의 각 line을 단어로 쪼갠 RDD
- `reduceBykey()` : algebraic operation
    - `reduceByKey()` : key를 기준으로 aggregate

#### parallel로 실행하는 방법

- RDD를 parititoning 하여 여러 machine에 저장
- 각 machine에서 `map()`, `reduce()`를 parallel하게 수행
    - `reduceByKey()`를 실행하는 machine 안에는 동일한 key를 가지는 record가 저장됨

#### lazy evaluation

- `textFile()` : input file을 읽어 RDD로 변환
- `flatMap()` : `textFile()`의 leaf node
- `mapToPair()` : `flatMap()`의 leaf node
- tree 형태의 view로 연산을 집합시켜놓고 나중에 실행함
- query optimizer가 tree를 평하가고, 더 최적화된 tree를 형성할 수 있음

#### Directed Acyclic Graph (DAG) structure

- operation이 1개 이상의 부모를 가질 수 있음
- tree형태는 최대 1개의 부모를 가짐

#### `DataSet` type

- record의 attribute를 정의할 수 있음
- Parquet, ORC, Avro file에서 널리 사용됨

#### 예시 : Parquet format

````
// instructor, department relation을 읽음
Dataset<Row> instructor = spark.read().parquet("...");
Dataset<Row> department = spark.read().parquet("...");

instructor.filter(instructor.col("salary").gt(100000)) // filter : salary > 100000
    .join(department, instructor.col("dept name") // join : departmentd의 dept name과
    .equalTo(department.col("dept name"))) // join condition : dept name이 같은 경우
    .groupBy(department.col("building")) // group by : building을 기준으로 group
    .agg(count(instructor.col("ID"))); // aggregate : 각 group별로 count
````

- `DataSet<Row>` : Row type으로 지정
    - column 값에 접근 시 name 사용
    - compile-time에 type check를 할 수 없음

````
Dataset<Instructor> instructor = spark.read().parquet("...")
    .as(Encoders.bean(Instructor.class));
````

- `Instructor` 클래스와 getter, setter를 지정하면,
- 위처럼 Parquet file을 읽을 수 있음
- compile-time에 type check를 할 수 있음
- `col("salary")` -> `getSalary()`, `setSalary()`로 변환됨