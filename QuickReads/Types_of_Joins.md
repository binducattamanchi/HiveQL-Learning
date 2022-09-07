# Map Join

![image](https://user-images.githubusercontent.com/32897934/124365260-69838780-dc64-11eb-84c9-6eba9790134f.png)

* Also known as Map-side Join, Broadcast Join, or Auto Map Join.
* Basically, before the original MapReduce task, its first step is to create a MapReduce local task. However, from HDFS this map/reduce task read data of the small table. Further save it into an in-memory hash table, then into a hash table file.
* Afterward, it moves the hash table file to the Hadoop Distributed Cache while original join MapReduce task starts, which will populate the file to each mapper’s local disk. Hence, in this way, all the mapper can load this hash table file into the memory and then do the join in Map stage.
* So, let’s understand this with an example. let’s suppose, for a join with big table A and small table B, for every mapper for table A, Table B is read completely. Since the smaller table is loaded into memory at first.
* Afterward, join is performed in the map phase of the MapReduce job, no reducer is needed and reduce phase is skipped. However, map joins in Hive are way faster than the regular joins since no reducers are necessary.

**Parameters related to Map Side Join**
a. hive.auto.convert.join
However, this option set true, by default. Moreover, when a table with a size less than 25 MB (hive.mapjoin.smalltable.filesize) is found, When it is enabled, during joins, the joins are converted to map-based joins.

b. Hive.auto.convert.join.noconditionaltask
When there comes a scenario while three or more tables involve in the join condition. Further, Hive generates three or more map-side joins with an assumption that all tables are of smaller size by using hive.auto.convert.join.

Moreover, we can combine three or more map-side joins into a single map-side join if the size of the n-1 table is less than 10 MB using hive.auto.convert.join.noconditionaltask. Basically, this rule define by hive.auto.convert.join.noconditionaltask.size.

**Limitations:**


- First, the major restriction is, we can never convert Full outer joins to map-side joins.
- However, it is possible to convert a left-outer join to a map-side join in the Hive. However, only possible since the right table that is to the right side of the join conditions, is lesser than 25 MB in size.
- Also, we can convert a right-outer join to a map-side join in the Hive. Similarly, only possible if the left table size is lesser than 25 MB.

```
set hive.ignore.mapjoin.hint=false;
Then:
select /*+ MAPJOIN(a) */ a.* from passwords a, passwords2 b where a.col0=b.col0 ;
```

```
set hive.auto.convert.join=true;
set hive.auto.convert.join.noconditionaltask=true;
set hive.auto.convert.join.noconditionaltask.size=10000000;
```

# Sort Merge Bucket Join

Also known as Bucket join

# Bucket Map Join

# Skew Join


# Comparison

