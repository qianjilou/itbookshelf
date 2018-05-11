## 第26章 超轻量级php框架brophp

---

### 26.1 brophp框架概述


```php
<?php
	/**
		file: index.php	声明基于BroPHP框架开发的项目，单一入口文件示例
	*/
	define("BROPHP", "./brophp");		#定义BroPHP框架所在路径（相对于入口文件，不要加"/"）
	define("APP", "./");				#定义项目的应用路径('/'可加可不加)
	define(BROPHP."/brophp.php");		#加载BroPHP框架目录下的入口文件

```

```php
<?php
	/**
		file: index.php	声明基于BroPHP框架开发的项目，单一入口文件示例
	*/
	define("APP", "./");				#定义项目的应用路径('/'可加可不加)
	define("./brophp/brophp.php");		#加载BroPHP框架目录下的入口文件brophp.php

```

```php
<?php
	/**
		file: index.php	声明基于BroPHP框架开发的项目，单一入口文件示例
	*/
	define("APP", "./home/");			#定义项目的应用路径在home下('/'可加可不加)
	define("./brophp/brophp.php");		#加载BroPHP框架目录下的入口文件brophp.php

```

```php
<?php
	/**
		file: index.php	声明基于BroPHP框架开发的项目，单一入口文件示例
	*/
	define("APP", "./");				#定义项目的应用路径('/'可加可不加)
	define("./brophp/brophp.php");		#加载BroPHP框架目录下的入口文件brophp.php

```

```php
<?php
	/**
		file: index.php	声明基于BroPHP框架开发的项目，单一入口文件示例
	*/
	define("APP", "./admin/");			#定义项目的应用路径('/'可加可不加)
	define("./brophp/brophp.php");		#加载BroPHP框架目录下的入口文件brophp.php

```

```php
<?php
	/**
		file: user.class.php 用户模块控制器，必须定义在当前应用的controls目录下
	*/
	Class User {
		//声明控制器的操作
	}

```

```php
<?php
	/**
		file: user.class.php 用户模块控制器，如果写继承也只能去继承BroPHP框架中的Action类
	*/
	Class User extends Action{
		//声明控制器的操作
	}

```

```php
<?php
	/**
		file:common.class.php	所有控制器的默认父类，自动生成并定义在当前应用controls目录下
	*/
	class Common extends Action {
		function init() {
			//所有的操作都会行执行这个方法
			//通常用于设置用户登录
		}
		
		//可以自定义一些方法， 作为所有控制器的公用操作方法
	}

```

```php

<?php
	/** file: user.class.php 在controls目录下， 默认继承Common及Action */
	class User {
		/* 控制器中默认的方法，用于获取用户默认的操作，例如输出用户列表信息 */
		function index() {	
			// 这个方法的URL访问如下两种方式， 可以不写操作名（index）
			// http://www.brophp.com/index.php/user/
			// http://www.brophp.com/index.php/user/index
		}
		
		/* 控制器中声明的方法，用于获取添加用户界面的操作*/
		function add() {
			// 这个操作的访问如下， 模块（user）, 操作(add)
			// http://www.brophp.com/index.php/user/add
		}
		
		/* 控制器中声明的方法，用于修改用户的操作*/
		function mod() {
			// 这个操作的访问如下， 模块(user), 操作(add)
			// http://www.brophp.com/index.php/user/mod/id/5
			p( $_GET["5"] );   //输出结果： Array( "id"=>5 ); 
		}
		
		/* 控制器中声明的私有的其他方法， 不是一个操作，最好写到Model中 */
		private function upload() {
			//这个用于上传用户头像， 不是操作则不能用URL访问
		}
	}
```

```php

<?php
	/** file: user.class.php 在controls目录下， 默认继承Common及Action */
	class User {
		/* 控制器中默认的方法，用于获取用户默认的操作 */
		function index() {	
			//默认的操作方法
		}
		
		/* 控制器中声明的方法， 用于删除用户的操作 */
		function del() {
			//创建用户对象
			$user = D("user");
			//使用用户对象中的delete删除某指定的一个用户
			if( $user->delete($_GET["id"]) ) {
				//如果删除成功就重定向到本模板的默认操作index中
				$this -> redirect("index");
			} else {
				//如果删除失败就跳转到提示界面，并返回
				$this -> error("删除用户失败");
			}
		}
	}
```

```php

<?php
	/** file: user.class.php  定义一个控制器类User */
	class User {
		/* 控制器中默认的方法 */
		function index() {	
			//向模板中分配变量
			$this->assign("data", $data);
			//输出模板（这个方法在BroPHP框架中改写了）
			$this->display();
			//判断模板是否已经被缓存
			$this->isCached();
			//消除单个模板缓存
			$this->clearCache();
			//消除所有缓存的模板
			$this->clearAllCache();
		}
	}
```

```php

<?php
	/** file: user.class.php  定义一个控制器类User */
	class User {
		/* 控制器中默认的方法 */
		function index() {	
			/*
				如果没有提供参数，默认找和当前模块相同目录名(user)下的
				默认模板文件名为当前操作名（index）, 后缀名为tpl（配置文件中可改）
				例如： view/default/user/index.tpl 模板文件
			*/
			$this -> display();
			
			/*
				如果提供参数没有"/", 默认找和当前模块相同目录名(user)下的
				模板文件名为参数名add, 后缀名为tpl
				例如： view/default/user/add.tpl 模板文件
			*/
			$this -> display("add"); 
			
			/*
				如果提供参数有"/"，找和"/"前模块同名目录（shop）下的
				模板文件名为参数名add, 后缀名为tpl(配置文件可改)
				例如: view/default/shop/add.tpl 模板文件
			*/
			$this -> display("shop/add");
		}
	}
```

```php
<?php
	/**
		file: users.class.php 自定义处理users表的Model类，声明在当前应用下的models目录下
		默认继承系统内置DB类， 可以直接使用所有从DB类中继承过来的方法
	*/
	class Users {
		/* 声明一个用户登录的方法 */
		function isLogin() {
			//方法体
		}
		
		/* 声明一个退出系统的方法 */
		function isLogout() {
			//方法体
		}
	}

```

```php
<?php
	/**
		file: Demo.class.php 在models目录下， 将来会自动继承DB类作为多个Model的父类
	*/
	class Demo {
		//声明一个功能方法
		function fun() {
			//多个子类可以共用这个方法
		}
	}

```

```php

<?php
	/**
		file: books.class.php 声明一个类Books主动继承Demo类， 间接继承了DB类
	*/
	class Books extends Demo {
		//在这个类中可以使用Demo类和系统内置DB中的所有继承过来的成员
	}
```

```php
<?php
	/** 
		file: user.class.php 在controls目录下，声明用户模块控制器， 默认继承Common及Action 
	*/
	class User {
		/* 控制器中默认的方法，用于获取用户默认的操作 */
		function index() {	
			//创建用户对象， 参数user找模型中的User类创建
			$user = D("user");
			$data = $user->select();  	//调用父类中的方法查询表中所有记录
			//$user -> isLogin();			//也可以调用User类中声明的方法
		}
	}

```

```php
<?php
	/** 
		file: user.class.php 在controls目录下，声明用户模块控制器 
	*/
	class User {
		/* 控制器中默认的方法，用于获取用户默认的操作 */
		function index() {	
			$user = D("user");			//模型User类不存在， 参数为表名
			$data = $user -> select();	//调用系统DB类中的方法查询表中所有记录
		}
	}

```

```php
<?php
	/**
		file: index.class.php	在前台controls目录下声明的主控制器
	*/	
	class Index {
		/* 控制器中默认的方法 */
		function index() {
			/* 创建后台admin目录中models目录下的User类对象 */
			$user = D("user", "admin");
			$user -> isLogin(); 		//在前台调用后台User类中的登录方法
		}
	}

```

```php

<?php
	/**
		file: index.class.php	在前台controls目录下声明的主控制器
	*/	
	class Index {
		/* 控制器中默认的方法 */
		function index() {
			//如果没有传表名或类名， 则直拉创建DB对象，但不能对表进行操作
			$db = D(); 					//可以访问DB对象中非表的操作方法
			
			$db -> dbSize();			//获取数据库的空间使用信息
			$db -> dbVersion();			//获取数据库系统的版本
			$db -> beginTransaction();	//开启事务
			$db -> commit();			//提交事务
			$db -> rollback();			//回滚事务
		}
	}
```

```php

<?php
	/** 
		file: user.class.php 在controls目录下，声明用户模块控制器 
	*/
	class User {
		/* 控制器中添加方法 */
		function add() {
			$user = D("user");				//创建用户实例对象
			
			$_POST["ptime"] = time();		//向$_POST数组中添加用户注册时间
			$id = $user->insert();			//默认使用$_POST数组作为参数
			//$id = $user -> insert($_POST);	//也可以直接传递$_POST参数
		}
	}
```

```php

<?php
	/** 
		file: user.class.php 在controls目录下，声明用户模块控制器 
	*/
	class User {
		/* 控制器中修改方法 */
		function mod() {
			$user = D("user");				//创建用户实例对象
			
			$rows = $user -> update();		//默认使用$_POST数组作为参数
			//$rows = $user -> update($_POST); 	//也可以直接传递$_POST参数
		}
	}
```


```php
<?php
	/** 
		file: user.class.php 在controls目录下，声明用户模块控制器 
	*/
	class User {
		/* 控制器中修改方法 */
		function mod() {
			$user = D("user");				//创建用户实例对象
			
			//默认使用$_POST数组作为参数(可以默认)， 加上where()连贯操作
			$rows = $user ->where("1, 2, 3") -> update($_POST);		
		}
	}

```

```php
<?php
	/** 
		file: user.class.php 在controls目录下，声明用户模块控制器 
	*/
	class User {
		/* 控制器中删除的方法 */
		function del() {
			$user = D("user");				//创建用户实例对象
			
			//使用$_GET数组传过来的主键作为参数删除一条， 例如$_GET['id'] = 5;
			$rows = $user -> delete( $_GET['id'] );
		}
	}

```

```php
<?php
	/** 
		file: user.class.php 在controls目录下，声明用户模块控制器 
	*/
	class User {
		/* 控制器中删除的方法 */
		function del() {
			$user = D("user");				//创建用户实例对象
			
			/*
				$_POST数组中是传过来的多个主键（复选框中选中id为前5条）
				例如： $_POST = array("id" => array(1, 2, 3, 4, 5));
			*/
			$rows = $user -> delete( $_POST['id'] );
		}
	}

```

```php
<?php
	/** 
		file: user.class.php 在controls目录下，声明用户模块控制器 
	*/
	class User {
		/* 控制器中修改用户的方法 */
		function mod() {
			$user = D("user");						//创建用户实例对象
				
			$data = $user -> find( $_GET['id'] );	//$_GET['id'] = 5
			p( $data );								//打印结果数组
		}
	}

```

```php
<?php
	/** 
		file: user.class.php 在controls目录下，声明用户模块控制器 
	*/
	class User {
		/* 控制器中的默认操作方法 */
		function index() {
			$user = D("user");							//创建用户实例对象
			
			$data = $user -> field('id, name, age')		//设置查询字段
						  ->where('1, 2, 3')			//设置查询条件
						  ->order('id desc')			//设置排序条件
						  ->select();					//获取满足条件的记录
						  
			P( $data );									//打印二维数组
		}
	}

```

```php
<?php
	/**
		file: cat.class.php	声明的类别模块控制器
	*/
	class Cat {
		/* 控制器中默认的操作方法 */
		function index() {
			$cat = D("cats");
			
			//分别从cats和articles两个表中获取数据(右关联)
			$data = $cat -> field('id, name as cname')		//主键id必取
						 ->r_select(
								//数组  关联表名			字段列表		    外键
								array('articles', 'id, name, content', 'cid')
						    );
							
			P( $data );		//打印二维数组
		}
	}

```

```php
<?php
	/**
		file: cat.class.php	声明的类别模块控制器
	*/
	class Cat {
		/* 控制器中默认的操作方法 */
		function index() {
			$cat = D("cats");
			
			$data = $cat -> field('id, name')		//不需要别名
						 ->r_select(
							array('articles', 'id, name, content', 'cid', array('art', 'id desc', 5))
						 );
							
			P( $data );		
		}
	}

```


```php
<?php
	/**
		file: cat.class.php	声明的类别模块控制器
	*/
	class Cat {
		/* 控制器中默认的操作方法 */
		function index() {
			$cat = D("cats");
			
			$data = $cat -> field('id, name')		//不需要别名
						 ->r_select(
							array('articles', 'id, name, content', 'cid', array('art', 'id desc')),
							array('tests', 'id, test', 'cid', array('test'))
						 );
							
			P( $data );		
		}
	}

```

```php
<?php
	//获取并分配所有栏目
	$column -> field("id, title, picid")->order("ord asc")->where(array("pid"=>0, "display"=>1))
	  	    -> r_select(
				array("image", 'name as imgname', 'id', 'picid'),
				array("column", 'id, title', 'pid', array("subcol", 'ord asc', '4', "display=1")), 
				array("article", 'id, title', 'pid', array('art', 'id desc', 10, "audit=1"))
			);


```

```php
<?php
	/**
		file: cat.class.php	声明的类别模块控制器
	*/
	class Cat {
		/* 控制器中删除的操作方法 */
		function del() {
			$cat = D("cats");
			
			$data = $cat -> where('1, 2')
						 -> r-delete(
							array('articles', 'cid'),
							array('tests', 'cid', array('test'=>'%php%'))
						 );
							
			P( $data );		 //返回全部的影响行数
		}
	}

```

```php
<?php
	/**
		file: user.class.php 定义一个用户控制器类User
	*/
	class User {
	    /* 控制器中默认操作方法， 获取用户表记录的总数和全部记录 */
		function index(){
			$user = D("users");        
			
			$total = $user->query('SELECT [内容任意] FROM bro_users', 'total'); //获取总数 
			$data  = $user->query('SELECT * FROM bro_users','select');          //全部记录
			
			P($total, $data);                        //打印二维数组
		}
		
		/* 自定义insert语句， 使用?参数， 用最后一个数组参数绑定值，$user->tabName代表表名 */
		function add(){
			$user = D('users');       
			//返回最后插入的ID
			$id = $user->query('insert into {$user->tabName}(name, age, sex) values(?,?,?)', 
							   'insert', 
							    array('zhangsan',10, '男'));  
			p($id);
		}

		/* 自定义删除SQL语句， 删除age > 20 */
		function del(){
			$user = D('users');       
			//返回影响的行数
			$num = $user->query('DELETE FROM {$user->tabName} WHERE age > ?', 'delete', array(20));
			p($num)
		}
		
		/*自定久update语句， 更新id=2的数据 */
		function mod(){
			$user = D('users');        
			//返回影响的行数
			$num = $user->query('UPDATE {$user->tabName} set name=?,age=?,sex=? WHERE id=?', 
								'update', 
								array('zhangsan', 15, '女', 2));    
			p($num);
		}
		
		/* 自定义创建表hello语句， 在第二个参数使用空字符串 */
		function create(){
			$user = D('users');        
		    //成功返回true
			$user->query('CREATE TABLE IF NOT EXISTS hello(id INT, name VARCHAR(30))','');            
		}
	}


```

```php
<?php
	/**
		file: user.class.php	定义一个用户控制器类User
	*/
	class User {
		/* 处理用户从添加表单提交过来的数据，加入到数据表users中， 并设置通过users.xml自动验证 */
		function insert(0 {
			$user = D("users");
			
			if($user -> insert($_POST, 1, 1)) {
				$this -> success("添加用户成功", 1, 'index');  //第三个参数“1”， 开启XML验证
			} else {
				$this -> error($user->getMsg(), 3, 'add');	   //使用getMsg()获取XML中的提示信息
			}
		}
		
		/* 处理用户从修改表单提交过来的数据，修改数据表users一条记录，并设置通过users.xml自动验证 */
		function update() {
			$user = D("users");
			
			if($user -> update(null, 1, 1)) {
				$this -> success("修改用户成功", 2, 'index');   //第三个参数“1”， 开启XML验证
			} else {
				$this -> error($user->getMsg(), 3, 'mod');      //使用getMsg()获取XML中的提示信息
			}
		}
	}

```

```php
<?php
	/**
		file: user.class.php	定义一个用户控制器类User
	*/
	class User {
		/* 控制器中的默认操作方法 */
		function index() {
			/* 如果有对应的缓存文件则不再去连接数据库和执行SQL查询，使用缓存Smarty的isCached（）方法判断 */
			if( !$this -> isCached(null, $_SERVER["REQUEST_URI"]) ) {
				//连接了数据库，读取表的数据
				$user = D('users');
				$this -> assign('data', $user -> select());
			}
			
			$this -> display(null, $_SERVER['REQUEST_URI']);	//使用URI作为缓存ID
		}
	}

```

```php
<?php
	/**
		file: index.class.php	定义一个控制器类Index
	*/
	class Index {
		/* 控制器中默认的操作方法 */
		function index() {
			//关闭调试模式的输出，或使用$GLOBALS['debug']=0 也可以 
			debug( 0 );
		}
	}

```

```php
<?php
	/**
		file: user.class.php	定义一个用户控制器类User
	*/
	class User {
		/* 控制器中的默认操作方法, 以分页形式显示所有用户 */
		function index() {
			$user = D("users");
			//不需要加载分页类，直接创建分页对象，只使用前两个参数， 每页显示5条数据
			$page = new Page($user->total(), 5);
			//获取每页数据， 使用分页类中的$limit属性， 获取limit限制
			$data = $user -> limit($page->limit) -> select();
			//将数据分配给模板
			$this -> assign('data', $data);
			//分配分页内容给模板， 使用分页类中的fpage()方法获取分页内容
			$this -> assign('fpage', $page -> fpage());
			//显示输出模板
			$this -> display();
		}
	}

```

```php
<?php
	/**
		file: user.class.php	定义一个用户控制器类User
	*/
	class User {
		/* 控制器中的操作 */
		function code() {
			//直接输出验证码对象，使用默认参数
			echo new Vcode();
			//或 new Vcode(100, 25, 5);	//使用参数设置验证码样式
		}
	}

```

```php
<?php
	/**
		file: user.class.php	定义一个用户控制器类User
	*/
	class User {
		/* 控制器中添加用户的操作方法， 需要添加用户头像 */
		function add() {
			$user = D('users');
			//使用返回的图片名称追加到$_POST数组中， 随表单的其它元素一起插入到数据库当中
			$_POST['picname'] = $this->upload();
			//将用户信息添加到数据表users中
			$user -> insert($_POST);
		}
		
		/* 文件上传方法， 这个方法最好不要声明在控制器中，最好定义到Model类中去 */
		private function upload() {
			//可以通过参数指定上传位置， 也可以通过set()方法设置
			$up = new FileUpload();
			//pic为上传表单的名称， 通过upload()方法实现
			if($up->upload('pic')) {
				//返回上传后的文件名， 通过getFileName()方法
				return $up -> getFileName();
			} else {
				//如果上传失败提示出错原因， 通过getErrorMsg()方法
				$this -> error($up -> getErrorMsg(), 3, 'index');
			}
		}
	}

```


```php

<?php
/* 文件上传方法，这个方法最好不要声明在控制器，最好定义到Model类中去 */
	private function upload() {
		//可以通过参数指定上传位置，也可通过set()方法设置
		$up = new FileUpload();
		
		    //设置上传文件存方位置
		$up -> set('path', '/usr/www/uploads')
			//设置上传文件充许的大小，单位为字节
			-> set('maxSize', 1000000)
			//设置充许上传的文件类型
			-> set('allowType', array('gif', 'jpg', 'png'))
			//设置启有上传后随机文件名， true启用（默认）， false使用原文件名
			-> set('israndname', true)
			//设置上传图片的缩放大小， 还可以通过prefix指定新名前缀
			-> set('thumb', array('width'=>300, 'height'=>200))
			//设置为上传图片加水印， 也可以通过prefix指定新名前缀
			-> set('watermark', array('water'=>'php.gif', 'position'=>5)));
			
		//pic为上传表单的名称， 通过upload()方法实现
		if($up->upload('pic')) {
			//返回上传后的文件名， 通过getFileName()方法
			return $up -> getFileName();
		} else {
			//如果上传失败提示出错原因， 通过getErrorMsg()方法
			$this -> error($up -> getErrorMsg(), 3, 'index');
		}
	}
```

```php


```

```php


```

```php


```

```php


```

```php


```

```php


```







#### 26.1.1 系统特点
#### 26.1.2 环境要求
#### 26.1.3 brophp框架源码的目录结构

---

### 26.2 单一入口
#### 26.2.1 基于brophp框架的单一入口编写规则

---

### 26.3 部署项目应用目录
#### 26.3.1 项目部署方式
#### 26.3.2 url访问

---

### 26.4 brophp框架的基本设置
#### 26.4.1 默认开启
#### 26.4.2 配置文件
#### 26.4.3 内置函数

---

### 26.5 声明控制器（control）
#### 26.5.1 控制器的声明（模块）
#### 26.5.2 操作的声明
#### 26.5.3 页面跳转
#### 26.5.4 重定向

---

### 26.6 设计视图（view）
#### 26.6.1 视图与控制器之间的交互
#### 26.6.2 切换模板风格
#### 26.6.3 模板文件的声明规则
#### 26.6.4 display()用新用法
#### 26.6.5 在模板中的几个常用变量应用
#### 26.6.6 在php程序中定义资源位置

---

### 26.7 应用模型（model）
#### 26.7.1 brophp数据库操作接口的特性
#### 26.7.2 切换数据库驱动
#### 26.7.3 声明和实例化model
#### 26.7.4 数据库的统一操作接口

---

### 26.8 自动验证

---

### 26.9 缓存设置
#### 26.9.1 基于memcached缓存设置
#### 26.9.2 基于smarty的缓存机制

---

### 26.10 调试模式

---

### 26.11 内置扩展类库
#### 26.11.1 分页类page
#### 26.11.2 验证码类vcode
#### 26.11.3 图像处理类image
#### 26.11.4 文件上传类fileupload

---

### 26.12 自定义功能扩展
#### 26.12.1 自定义扩展类库
#### 26.12.2 自定义扩展函数库

---

### 26.13 小结

- [ ] 本章必须掌握的知识点
- [ ] 本章需要了解的内容