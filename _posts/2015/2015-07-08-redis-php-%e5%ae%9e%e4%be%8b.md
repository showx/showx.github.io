---
layout: post
title: redis php 实例
date: 2015-07-08 16:28
author: admin
comments: true
categories: []
---
redis的操作很多的，以前看到一个比较全的博客，但是现在找不到了。查个东西搜半天，下面整理一下php处理redis的例子，个人觉得常用一些例子。下面的例子都是基于php-redis这个扩展的。

1，connect

描述：实例连接到一个Redis.
参数：host: string，port: int
返回值：BOOL 成功返回：TRUE;失败返回：FALSE
查看复制打印?

    示例：  
      
    <?php  
    $redis = new redis();  
    $result = $redis->connect('127.0.0.1', 6379);  
    var_dump($result); //结果：bool(true)  
    ?>  

2，set

描述：设置key和value的值
参数：Key Value
返回值：BOOL 成功返回：TRUE;失败返回：FALSE

示例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $result = $redis->set('test',"11111111111");  
    var_dump($result);    //结果：bool(true)  
    ?>  

3，get

描述：获取有关指定键的值
参数：key
返回值：string或BOOL 如果键不存在，则返回 FALSE。否则，返回指定键对应的value值。

范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $result = $redis->get('test');  
    var_dump($result);   //结果：string(11) "11111111111"  
    ?>  

4，delete

描述：删除指定的键
参数：一个键，或不确定数目的参数，每一个关键的数组：key1 key2 key3 … keyN
返回值：删除的项数
范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->set('test',"1111111111111");  
    echo $redis->get('test');   //结果：1111111111111  
    $redis->delete('test');  
    var_dump($redis->get('test'));  //结果：bool(false)  
    ?>  

5，setnx

描述：如果在数据库中不存在该键，设置关键值参数
参数：key value
返回值：BOOL 成功返回：TRUE;失败返回：FALSE

范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->set('test',"1111111111111");  
    $redis->setnx('test',"22222222");  
    echo $redis->get('test');  //结果：1111111111111  
    $redis->delete('test');  
    $redis->setnx('test',"22222222");  
    echo $redis->get('test');  //结果：22222222  
    ?>  

6，exists

描述：验证指定的键是否存在
参数key
返回值：Bool 成功返回：TRUE;失败返回：FALSE

范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->set('test',"1111111111111");  
    var_dump($redis->exists('test'));  //结果：bool(true)  
    ?>  

7，incr

描述：数字递增存储键值键.
参数：key value：将被添加到键的值
返回值：INT the new value
实例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->set('test',"123");  
    var_dump($redis->incr("test"));  //结果：int(124)  
    var_dump($redis->incr("test"));  //结果：int(125)  
    ?>  

8，decr

描述：数字递减存储键值。
参数：key value：将被添加到键的值
返回值：INT the new value
实例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->set('test',"123");  
    var_dump($redis->decr("test"));  //结果：int(122)  
    var_dump($redis->decr("test"));  //结果：int(121)  
    ?>  

9，getMultiple

描述：取得所有指定键的值。如果一个或多个键不存在，该数组中该键的值为假
参数：其中包含键值的列表数组
返回值：返回包含所有键的值的数组
实例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->set('test1',"1");  
    $redis->set('test2',"2");  
    $result = $redis->getMultiple(array('test1','test2'));  
    print_r($result);   //结果：Array ( [0] => 1 [1] => 2 )  
    ?>  

10，lpush

描述：由列表头部添加字符串值。如果不存在该键则创建该列表。如果该键存在，而且不是一个列表，返回FALSE。
参数：key,value
返回值：成功返回数组长度，失败false

实例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    var_dump($redis->lpush("test","111"));   //结果：int(1)  
    var_dump($redis->lpush("test","222"));   //结果：int(2)  
    ?>  

11，rpush

描述：由列表尾部添加字符串值。如果不存在该键则创建该列表。如果该键存在，而且不是一个列表，返回FALSE。
参数：key,value
返回值：成功返回数组长度，失败false
范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    var_dump($redis->lpush("test","111"));   //结果：int(1)  
    var_dump($redis->lpush("test","222"));   //结果：int(2)  
    var_dump($redis->rpush("test","333"));   //结果：int(3)  
    var_dump($redis->rpush("test","444"));   //结果：int(4)  
    ?>  

12，lpop

描述：返回和移除列表的第一个元素
参数：key
返回值：成功返回第一个元素的值 ，失败返回false

范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->lpush("test","111");  
    $redis->lpush("test","222");  
    $redis->rpush("test","333");  
    $redis->rpush("test","444");  
    var_dump($redis->lpop("test"));  //结果：string(3) "222"  
    ?>  

12，rpop

描述：返回和移除列表的最后一个元素
参数：key
返回值：成功返回最后一个元素的值 ，失败返回false

范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->lpush("test","111");  
    $redis->lpush("test","222");  
    $redis->rpush("test","333");  
    $redis->rpush("test","444");  
    var_dump($redis->rpop("test"));  //结果：string(3) "444"  
    ?>  

13，lsize,llen

描述：返回的列表的长度。如果列表不存在或为空，该命令返回0。如果该键不是列表，该命令返回FALSE。
参数：Key
返回值：成功返回数组长度，失败false
范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->lpush("test","111");  
    $redis->lpush("test","222");  
    $redis->rpush("test","333");  
    $redis->rpush("test","444");  
    var_dump($redis->lsize("test"));  //结果：int(4)  
    ?>  

14，lget

描述：返回指定键存储在列表中指定的元素。 0第一个元素，1第二个… -1最后一个元素，-2的倒数第二…错误的索引或键不指向列表则返回FALSE。
参数：key index
返回值：成功返回指定元素的值，失败false

范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->lpush("test","111");  
    $redis->lpush("test","222");  
    $redis->rpush("test","333");  
    $redis->rpush("test","444");  
    var_dump($redis->lget("test",3));  //结果：string(3) "444"  
    ?>  

15，lset

描述：为列表指定的索引赋新的值,若不存在该索引返回false.
参数：key index value
返回值：成功返回true,失败false

范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->lpush("test","111");  
    $redis->lpush("test","222");  
    var_dump($redis->lget("test",1));  //结果：string(3) "111"  
    var_dump($redis->lset("test",1,"333"));  //结果：bool(true)  
    var_dump($redis->lget("test",1));  //结果：string(3) "333"  
    ?>  

16，lgetrange

描述：
返回在该区域中的指定键列表中开始到结束存储的指定元素，lGetRange(key, start, end)。0第一个元素，1第二个元素… -1最后一个元素，-2的倒数第二…
参数：key start end
返回值：成功返回查找的值，失败false

范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->lpush("test","111");  
    $redis->lpush("test","222");  
    print_r($redis->lgetrange("test",0,-1));  //结果：Array ( [0] => 222 [1] => 111 )  
    ?>  

17,lremove

描述：从列表中从头部开始移除count个匹配的值。如果count为零，所有匹配的元素都被删除。如果count是负数，内容从尾部开始删除。
参数：key count value
返回值：成功返回删除的个数，失败false
范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->lpush('test','a');  
    $redis->lpush('test','b');  
    $redis->lpush('test','c');  
    $redis->rpush('test','a');  
    print_r($redis->lgetrange('test', 0, -1)); //结果：Array ( [0] => c [1] => b [2] => a [3] => a )  
    var_dump($redis->lremove('test','a',2));   //结果：int(2)  
    print_r($redis->lgetrange('test', 0, -1)); //结果：Array ( [0] => c [1] => b )  
    ?>  

18，sadd

描述：为一个Key添加一个值。如果这个值已经在这个Key中，则返回FALSE。
参数：key value
返回值：成功返回true,失败false
范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    var_dump($redis->sadd('test','111'));   //结果：bool(true)  
    var_dump($redis->sadd('test','333'));   //结果：bool(true)  
    print_r($redis->sort('test')); //结果：Array ( [0] => 111 [1] => 333 )  
    ?>  

19，sremove

描述：删除Key中指定的value值
参数：key member
返回值：true or false
范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->sadd('test','111');  
    $redis->sadd('test','333');  
    $redis->sremove('test','111');  
    print_r($redis->sort('test'));    //结果：Array ( [0] => 333 )  
    ?>  

20,smove

描述：将Key1中的value移动到Key2中
参数：srcKey dstKey member
返回值：true or false
范例
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->delete('test1');  
    $redis->sadd('test','111');  
    $redis->sadd('test','333');  
    $redis->sadd('test1','222');  
    $redis->sadd('test1','444');  
    $redis->smove('test',"test1",'111');  
    print_r($redis->sort('test1'));    //结果：Array ( [0] => 111 [1] => 222 [2] => 444 )  
    ?>  

21，scontains

描述：检查集合中是否存在指定的值。
参数：key value
返回值：true or false
范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->sadd('test','111');  
    $redis->sadd('test','112');  
    $redis->sadd('test','113');  
    var_dump($redis->scontains('test', '111')); //结果：bool(true)  
    ?>  

22,ssize

描述：返回集合中存储值的数量
参数：key
返回值：成功返回数组个数，失败0
范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->sadd('test','111');  
    $redis->sadd('test','112');  
    echo $redis->ssize('test');   //结果：2  
    ?>  

23，spop

描述：随机移除并返回key中的一个值
参数：key
返回值：成功返回删除的值，失败false

范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->sadd("test","111");  
    $redis->sadd("test","222");  
    $redis->sadd("test","333");  
    var_dump($redis->spop("test"));  //结果：string(3) "333"  
    ?>  

24,sinter

描述：返回一个所有指定键的交集。如果只指定一个键，那么这个命令生成这个集合的成员。如果不存在某个键，则返回FALSE。
参数：key1, key2, keyN
返回值：成功返回数组交集，失败false

范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->sadd("test","111");  
    $redis->sadd("test","222");  
    $redis->sadd("test","333");  
    $redis->sadd("test1","111");  
    $redis->sadd("test1","444");  
    var_dump($redis->sinter("test","test1"));  //结果：array(1) { [0]=> string(3) "111" }  
    ?>  

25,sinterstore

描述：执行sInter命令并把结果储存到新建的变量中。
参数：
Key: dstkey, the key to store the diff into.
Keys: key1, key2… keyN. key1..keyN are intersected as in sInter.
返回值：成功返回，交集的个数，失败false
范例:
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->sadd("test","111");  
    $redis->sadd("test","222");  
    $redis->sadd("test","333");  
    $redis->sadd("test1","111");  
    $redis->sadd("test1","444");  
    var_dump($redis->sinterstore('new',"test","test1"));  //结果：int(1)  
    var_dump($redis->smembers('new'));  //结果:array(1) { [0]=> string(3) "111" }  
    ?>  

26,sunion

描述：
返回一个所有指定键的并集
参数：
Keys: key1, key2, … , keyN
返回值：成功返回合并后的集，失败false
范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->sadd("test","111");  
    $redis->sadd("test","222");  
    $redis->sadd("test","333");  
    $redis->sadd("test1","111");  
    $redis->sadd("test1","444");  
    print_r($redis->sunion("test","test1"));  //结果：Array ( [0] => 111 [1] => 222 [2] => 333 [3] => 444 )  
    ?>  

27,sunionstore

描述：执行sunion命令并把结果储存到新建的变量中。
参数：
Key: dstkey, the key to store the diff into.
Keys: key1, key2… keyN. key1..keyN are intersected as in sInter.
返回值：成功返回，交集的个数，失败false
范例:
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->sadd("test","111");  
    $redis->sadd("test","222");  
    $redis->sadd("test","333");  
    $redis->sadd("test1","111");  
    $redis->sadd("test1","444");  
    var_dump($redis->sunionstore('new',"test","test1"));  //结果：int(4)  
    print_r($redis->smembers('new'));  //结果:Array ( [0] => 111 [1] => 222 [2] => 333 [3] => 444 )  
    ?>  

28,sdiff

描述：返回第一个集合中存在并在其他所有集合中不存在的结果
参数：Keys: key1, key2, … , keyN: Any number of keys corresponding to sets in redis.
返回值：成功返回数组，失败false
范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->sadd("test","111");  
    $redis->sadd("test","222");  
    $redis->sadd("test","333");  
    $redis->sadd("test1","111");  
    $redis->sadd("test1","444");  
    print_r($redis->sdiff("test","test1"));  //结果：Array ( [0] => 222 [1] => 333 )  
    ?>  

29,sdiffstore

描述：执行sdiff命令并把结果储存到新建的变量中。
参数：
Key: dstkey, the key to store the diff into.
Keys: key1, key2, … , keyN: Any number of keys corresponding to sets in redis
返回值：成功返回数字，失败false
范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->sadd("test","111");  
    $redis->sadd("test","222");  
    $redis->sadd("test","333");  
    $redis->sadd("test1","111");  
    $redis->sadd("test1","444");  
    var_dump($redis->sdiffstore('new',"test","test1"));  //结果：int(2)  
    print_r($redis->smembers('new'));  //结果:Array ( [0] => 222 [1] => 333 )  
    ?>  

30,smembers, sgetmembers

描述：
返回集合的内容
参数：Key: key
返回值：An array of elements, the contents of the set.
范例：
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('127.0.0.1', 6379);  
    $redis->delete('test');  
    $redis->sadd("test","111");  
    $redis->sadd("test","222");  
    print_r($redis->smembers('test'));  //结果:Array ( [0] => 111 [1] => 222 )  
    ?>  

php-redis当中，有很多不同名字，但是功能一样的函数，例如：lrem和lremove，这里就不例举了。




==========
前面一篇博客主要是string类型，list类型和set类型，下面hash类型和zset类型

1,hset

描述：将哈希表key中的域field的值设为value。如果key不存在，一个新的哈希表被创建并进行HSET操作。如果域field已经存在于哈希表中，旧值将被覆盖。
参数：key field value
返回值：如果field是哈希表中的一个新建域，并且值设置成功，返回1。如果哈希表中域field已经存在且旧值已被新值覆盖，返回0。

实例1

2，hsetnx

描述：将哈希表key中的域field的值设置为value，当且仅当域field不存在。若域field已经存在，该操作无效。如果key不存在，一个新哈希表被创建并执行HSETNX命令。
参数：key field value
返回值：设置成功，返回1。如果给定域已经存在且没有操作被执行，返回0。

实例1

3，hget

描述：返回哈希表key中给定域field的值。
参数：key field
返回值：给定域的值。当给定域不存在或是给定key不存在时，返回nil。

实例1

4,hmset

描述：同时将多个field - value(域-值)对设置到哈希表key中。此命令会覆盖哈希表中已存在的域。如果key不存在，一个空哈希表被创建并执行HMSET操作。
参数：key field value [field value ...]
返回值：如果命令执行成功，返回OK。当key不是哈希表(hash)类型时，返回一个错误。

实例1

5,hmget

描述:返回哈希表key中，一个或多个给定域的值。如果给定的域不存在于哈希表，那么返回一个nil值。因为不存在的key被当作一个空哈希表来处理，所以对一个不存在的key进行HMGET操作将返回一个只带有nil值的表。
参数：key field [field ...]
返回值：一个包含多个给定域的关联值的表，表值的排列顺序和给定域参数的请求顺序一样。

实例1

6,hgetall

描述:返回哈希表key中，所有的域和值。在返回值里，紧跟每个域名(field name)之后是域的值(value)，所以返回值的长度是哈希表大小的两倍。
参数：key
返回值:以列表形式返回哈希表的域和域的值。 若key不存在，返回空列表。

实例1

7,hdel

描述:删除哈希表key中的一个或多个指定域，不存在的域将被忽略。
参数：key field [field ...]
返回值:被成功移除的域的数量，不包括被忽略的域。

实例1

8,hlen

描述:返回哈希表key中域的数量。
参数：key
返回值：哈希表中域的数量。当key不存在时，返回0。

实例1

9,hexists

描述:查看哈希表key中，给定域field是否存在。
参数：key field
返回值：如果哈希表含有给定域，返回1。如果哈希表不含有给定域，或key不存在，返回0。

实例1

10,hincrby

描述:为哈希表key中的域field的值加上增量increment。增量也可以为负数，相当于对给定域进行减法操作。
参数：key field increment
返回值：执行HINCRBY命令之后，哈希表key中域field的值。

实例1

11,hkeys

描述:返回哈希表key中的所有域。
参数：key
返回值：一个包含哈希表中所有域的表。当key不存在时，返回一个空表。

实例1

12,hvals

描述:返回哈希表key中的所有值。
参数：key
返回值：一个包含哈希表中所有值的表。当key不存在时，返回一个空表。
实例1
查看复制打印?

    <?php  
    $redis = new redis();  
    $redis->connect('192.168.1.108', 6379);  
    $redis->delete('test');  
    $redis->hset('test', 'key1', 'hello');  
    echo $redis->hget('test', 'key1');     //结果：hello  
      
    echo "<br>";  
    $redis->hSetNx('test', 'key1', 'world');  
    echo $redis->hget('test', 'key1');   //结果：hello  
      
    $redis->delete('test');  
    $redis->hSetNx('test', 'key1', 'world');  
    echo "<br>";  
    echo $redis->hget('test', 'key1');   //结果：world  
      
    echo $redis->hlen('test');   //结果：1  
    var_dump($redis->hdel('test','key1'));  //结果：bool(true)   
      
    $redis->delete('test');  
    $redis->hSet('test', 'a', 'x');  
    $redis->hSet('test', 'b', 'y');  
    $redis->hSet('test', 'c', 'z');  
    print_r($redis->hkeys('test'));  //结果：Array ( [0] => a [1] => b [2] => c )   
      
    print_r($redis->hvals('test'));  //结果：Array ( [0] => x [1] => y [2] => z )   
      
    print_r($redis->hgetall('test'));  //结果：Array ( [a] => x [b] => y [c] => z )   
      
    var_dump($redis->hExists('test', 'a'));  //结果：bool(true)   
      
    $redis->delete('test');  
    echo $redis->hIncrBy('test', 'a', 3);    //结果：3  
    echo $redis->hIncrBy('test', 'a', 1);    //结果：4  
      
    $redis->delete('test');  
    var_dump($redis->hmset('test', array('name' =>'tank', 'sex'=>"man"))); //结果：bool(true)  
    print_r($redis->hmget('test', array('name', 'sex')));  //结果：Array ( [name] => tank [sex] => man )  
    ?>  

13,zadd

描述：
增加一个或多个元素，如果该元素已经存在，更新它的socre值
虽然有序集合有序，但它也是集合，不能重复元素，添加重复元素只会
更新原有元素的score值

参数：
key
score : double
value: string

返回值：1 or 0

实例2

14，zrange

描述：取得特定范围内的排序元素,0代表第一个元素,1代表第二个以此类推。-1代表最后一个,-2代表倒数第二个...

参数：
key
start: long
end: long
withscores: bool = false

返回值：数组

实例2

15，zdelete, zrem

描述：从有序集合中删除指定的成员。

参数：
key
member

返回值：1 or 0

实例2

16，zrevrange

描述：返回key对应的有序集合中指定区间的所有元素。这些元素按照score从高到低的顺序进行排列。对于具有相同的score的元素而言，将会按照递减的字典顺序进行排列。该命令与ZRANGE类似，只是该命令中元素的排列顺序与前者不同。

参数：
key
start: long
end: long
withscores: bool = false

返回值：数组

实例2

17，zrangebyscore, zrevrangebyscore

描述：返回key对应的有序集合中score介于min和max之间的所有元素（包哈score等于min或者max的元素）。元素按照score从低到高的顺序排列。如果元素具有相同的score，那么会按照字典顺序排列。
可选的选项LIMIT可以用来获取一定范围内的匹配元素。如果偏移值较大，有序集合需要在获得将要返回的元素之前进行遍历，因此会增加O(N)的时间复杂度。可选的选项WITHSCORES可以使得在返回元素的同时返回元素的score，该选项自从Redis 2.0版本后可用。

参数：
key
start: string
end: string
options: array

返回值：数组

实例2

18，zcount

描述：返回key对应的有序集合中介于min和max间的元素的个数。

参数：
key
start: string
end: string

返回值：数组长度

实例2

19，zremrangebyscore, zreleterangebyscore

描述：移除key对应的有序集合中scroe位于min和max（包含端点）之间的所哟元素。从2.1.6版本后开始，区间端点min和max可以被排除在外，这和ZRANGEBYSCORE的语法一样。

参数：
key
start: double or "+inf" or "-inf" string
end: double or "+inf" or "-inf" string

返回值：删除元素个数

实例2

20，zremrangebyrank, zdeleterangebyrank

描述：移除key对应的有序集合中rank值介于start和stop之间的所有元素。start和stop均是从0开始的，并且两者均可以是负值。当索引值为负值时，表明偏移值从有序集合中score值最高的元素开始。例如：-1表示具有最高score的元素，而-2表示具有次高score的元素，以此类推。

参数：
key
start: LONG
end: LONG

返回值：删除元素个数

实例2

21，zsize, zcard

描述：返回存储在key对应的有序集合中的元素的个数。
参数：key
返回值：元素个数

实例2

22，zscore

描述：返回key对应的有序集合中member的score值。如果member在有序集合中不存在，那么将会返回null。
参数：key member

实例2

23,zrank, zrevrank

描述：返回key对应的有序集合中member元素的索引值，元素按照score从低到高进行排列。rank值（或index）是从0开始的，这意味着具有最低score值的元素的rank值为0。使用ZREVRANK可以获得从高到低排列的元素的rank（或index）。
参数：key member
返回值：数字

实例2

24，zincrby

将key对应的有序集合中member元素的scroe加上increment。如果指定的member不存在，那么将会添加该元素，并且其score的初始值为increment。如果key不存在，那么将会创建一个新的有序列表，其中包含member这一唯一的元素。如果key对应的值不是有序列表，那么将会发生错误。指定的score的值应该是能够转换为数字值的字符串，并且接收双精度浮点数。同时，你也可用提供一个负值，这样将减少score的值。
参数：key value member
返回值：字符型数据

实例2

25，zunion

描述：keys对应的numkeys个有序集合计算合集，并将结果存储在destination中
参数：keyOutput arrayZSetKeys arrayWeights aggregateFunction
返回值：并集数组

实例2

26，zinter

描述：keys对应的numkeys个有序集合计算交集，并将结果存储在destination中
参数：keyOutput arrayZSetKeys arrayWeights aggregateFunction
返回值：交集数组

实例2

查看复制打印?

    $redis = new redis();  
    $redis->connect('192.168.1.108', 6379);  
    $redis->delete('test');  
    $redis->zadd('test', 1, 'val1');  
    $redis->zadd('test', 0, 'val2');  
    $redis->zadd('test', 3, 'val3');  
      
    print_r($redis->zrange('test', 0, -1)); //结果：Array ( [0] => val2 [1] => val1 [2] => val3 )  
      
    $redis->zdelete('test', 'val2');  
    print_r($redis->zrange('test', 0, -1)); //结果：Array ( [0] => val1 [1] => val3 )   
      
    $redis->zadd('test',4, 'val0');  
    print_r($redis->zrevrange('test', 0, -1));  //结果：Array ( [0] => val0 [1] => val3 [2] => val1 )  
    print_r($redis->zrevrange('test', 0, -1,true));  //结果：Array ( [val0] => 4 [val3] => 3 [val1] => 1 )   
      
    echo "<br>";  
    $redis->zadd('key', 0, 'val0');  
    $redis->zadd('key', 2, 'val2');  
    $redis->zadd('key', 10, 'val10');  
      
    print_r($redis->zrangebyscore('key', 0, 3, array('limit' => array(1, 1),'withscores' => TRUE))); //结果：Array ( [val2] => 2 )  
    print_r($redis->zrangebyscore('key', 0, 3, array('limit' => array(1, 1)))); //结果：Array ( [0] => val2 )   
      
    echo $redis->zcount('key', 0, 3); //结果：2  
      
    $redis->zremrangebyscore('key', 0, 3);  
    print_r($redis->zrange('key', 0, -1));  //结果：Array ( [0] => val10 )   
      
    echo $redis->zsize('key');   //结果：1  
      
    $redis->zadd('key', 2.5, 'aaaa');  
    echo $redis->zscore('key', 'aaaa');   //结果：2.5  
      
    echo $redis->zrank('key', 'aaaa');   //结果：0  
    echo $redis->zrevrank('key', 'aaaa');    //结果：1  
      
    $redis->delete('key');  
      
    echo $redis->zincrby('key', 2, 'aaaa');  //结果：2  
    echo $redis->zincrby('key', 1, 'aaaa');  //结果：3  
      
    $redis->delete('key');  
    $redis->delete('test');  
      
    $redis->zadd('key', 0, 'val0');  
    $redis->zadd('key', 1, 'val1');  
    $redis->zadd('key', 4, 'val2');  
    $redis->zadd('test', 2, 'val2');  
    $redis->zadd('test', 3, 'val3');  
    $redis->zunion('k01', array('key', 'test'));  
    print_r($redis->zrange('k01',0, -1)); //结果：Array ( [0] => val0 [1] => val1 [2] => val3 [3] => val2 )  
      
    $redis->zunion('k03', array('key', 'test'), array(5, 1));  
    print_r($redis->zrange('k03',0, -1)); //结果：Array ( [0] => val0 [1] => val3 [2] => val1 [3] => val2 )   
      
    $redis->zinter('k02', array('key', 'test'));  
    print_r($redis->zrange('k02',0, -1)); //结果：Array ( [0] => val2 )  
    ?>  
