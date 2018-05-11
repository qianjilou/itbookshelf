# 第5部分 php开发高级篇
## 第22章 memcache管理与应用

---

### 22.1 memcache概述
#### 22.1.1 初识memcache
#### 22.1.2 memcache在web中的应用

---

### 22.2 memcached的安装及管理
```php
<?php
	/* 实例化Memcache类的对象 */
	$memcache = new Memcache; 
	
	/* 通过 $mecache中connect()方法连接到"memcache_host"位置和11211端口对应的memcached服务器 */
	$memcache -> connect('memcache_host', 11211);
	
	/* 关闭对象（对常连接不起作用） */
	$memcache ->close();
	
	


```


```php
<?php
	/* 实例化Memcache类的对象 */
	$memcache = new Memcache; 
	/* 连接本机的memcached服务器 */
	$memcache -> connect('localhost', 11211);
	
	/* 向本机的memcached服务器中添加一组数据 */
	$is_add1 = $memcache->add('brophp', 'BroPHP框架');
	/* 向本机添加每一数组作为数据,数组或对象将会被序列化 */
	$is_add2 = $memcache->add('lamp', array('Linux', 'Apache', 'MySQL', 'PHP'));
	/* 如果添加的key已经存在，则添加将会失败 , MEMCACHE_COMPRESSED 使用zlib压缩， 0表示不过期*/
	$is_add3 = $memcache->add('lamp', 'LAMP兄弟连',  MEMCACHE_COMPRESSED, 0);
	
	/*设置一个指定 key 的缓存变量内容, 如果key不存中则新添加，如果存在则为修改 */
	$is_set1 = $memcache->set('phpfw', 'BroPHP框架');
	/* 指定的key已经存在，则修改内容 ，缓存一周*/
	$is_set2 = $memcache->set('brophp', 'BroPHP超经量组框架',MEMCACHE_COMPRESSED, 7*24*60*60);
	
	/* 使用replace()替换一个指定已存在key 的的缓存变量内容 是set()方法的别名 ,设置大于30天的缓存*/
	$is_replace = $memcache->replace('lamp', 'LAMP兄弟连',  MEMCACHE_COMPRESSED, time()+31*24*60*60);
	
	/* 关闭与memcached服务器的连接 */
	$memcache -> close();
	
	
	
	
	
	


```

```php
<?php
	/* 实例化Memcache类的对象 */
	$memcache = new Memcache; 
	/* 连接本机的memcached服务器 */
	$memcache -> connect('localhost', 11211);

	/* 返回缓存的指定 brophp 的变量内容 */
	$var1 = $memcache->get('brophp');
	/*
		如果键brophp，lamp不存在 $var2 = array();
		如果brophp，lamp存在$var = array(‘brophp‘=>‘BroPHP超经量组框架‘, ‘lamp‘=>‘LAMP兄弟连‘);
	*/
	$var2 = $memcache->get(array('brophp', 'lamp'));
	
	var_dump($var1);
	var_dump($var2);
	
	/* 关闭与memcached服务器的连接 */
	$memcache -> close();
	


```

```php
<?php
	/* 实例化Memcache类的对象 */
	$memcache = new Memcache; 
	/* 连接本机的memcached服务器 */
	$memcache -> connect('localhost', 11211);

	$memcache -> delete('phpfw');       //立即删除phpfw的项
	$memcache -> delete('brophp', 0);   //立即删除brophp的项
	$memcache -> delete('lamp', 30);    //在30秒内删除lamp的项
	
	/* 关闭与memcached服务器的连接 */
	$memcache -> close();

	
	


```

```php
<?php
	/* 实例化Memcache类的对象 */
	$memcache = new Memcache; 
	/*
	$mem->addServer('192.168.0.11', 11211);   //添加第一个memcached服务器
	$mem->addServer('192.168.0.12', 11211);   //添加第二个memcached服务器
	$mem->addServer('192.168.0.13', 11211);   //添加第三个memcached服务器
	*/
	/* 通过配置文件可以动态设置多台memcached服务器的参数 */
	$mem_conf = array(
		array('host'=>'192.168.0.11', 'port'=>'11211'),
		array('host'=>'192.168.0.12', 'port'=>'11211'),
		array('host'=>'192.168.0.13', 'port'=>'11211')
	);

	/* 通过循环按$mem_conf数组中的内容设置多台memcached服务器 */
	foreach ( $mem_conf as $v ) {
		$memcache->addServer ( $v ['host'], $v ['port'] ); 
	}  

	/* 
		使用循环向3台memcached服务器中添加100条数据
		会使用“crc32(key) % current_server_num”哈希算法将 key 平均哈希到3台服务器中
	*/
	for($i=0; $i < 100; $i++) {
		$mem->set('key'.$i, md5($i).'This is a memcached test!', 0, 60); 
	}

	
	


```

```php
<?php
	/* 实例化Memcache类的对象 */
	$memcache = new Memcache; 
	
	/* 注意第6个参数值15的作用 */
	$is_add = $memcache->addServer('localhost', 11211, true, 1, 1, 15, true); 
	
	/* 向本机的memcached服务器中添加一组数据 */
	$is_set = $memcache->set('brophp', 'BroPHP超经量组框架');
	
	
	


```

```php

<?php
    /** 该函数用于执行有结果集的SQL语句，并将结果缓存到memcached服务器中
		@param	string	$sql 		有结果集的查询语句SQL
		@param	object	$memcache	Memcache类的对象
		@return	$data				返回结果集的数据    */
	function select($sql, Memcache $memcache){
		/* md5 SQL命令 作为 memcache的唯一标识符*/ 
		$key = md5($sql);
		/* 先从memcached服务器中获取数据 */
		$data = $memcache->get($key);
		/* 如果$data为false那么就是没有数据, 那么就需要从数据库中获取 */
		if(!$data) {
			try{     //很有必要将连接数据库的过程单独处理
				$pdo = new PDO("mysql:host=localhost;dbname=dbtest", "mysql_user", "mysql_pass");
			}catch(PDOException $e){
				die("连接失败：".$e->getMessage());
			}
			$stmt = $pdo->prepare($sql);
			$stmt->execute();
			/* 从数据库中获取数据,返回二维数组$data */
			$data = $stmt->fetchAll(PDO::FETCH_ASSOC);
			/* 这里向memcached服务器写入从数据库中获取的数据*/
			$memcache -> add($key, $data,  MEMCACHE_COMPRESSED, 0);
		}
		return $data;
	}
	
	$memcache = new Memcache; 
	/* 可以使用addServer()方法添加多台memcached服务器 */
	$memcache -> connect('localhost', 11211);  
	/* 第一次运行还没有缓存数据, 会读取一次数据库, 当再次访问程序时, 就直接从memcache获取*/
	$data = select("SELECT * FROM user", $memcache);
	var_dump($data);   //输出数据


	
	

```

```php


```


#### 22.2.1 linux下安装memcache软件
#### 22.2.2 windows下安装memcached软件
#### 22.2.3 memcached服务器的管理

---

### 22.3 使用telnet作为memcached的客户端管理
#### 22.3.1 连接memcached服务器
#### 22.3.2 基本的memcached客户端命令
#### 22.3.3 查看当前memcached服务器的运行状态信息
#### 22.3.4 数据管理指令

---

### 22.4 php的memcached管理接口
#### 22.4.1 安装php中的memcache应用程序扩展接口
#### 22.4.2 memcache应用程序扩展接口
#### 22.4.3 memcache的实例应用

---

### 22.5 memcached服务器的安全防护

---

### 22.6 小结
- [ ] 本章必须掌握的知识点
- [ ] 本章需要了解的内容
- [ ] 本章需要拓展的内容