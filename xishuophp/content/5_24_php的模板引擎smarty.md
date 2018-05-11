## 第24章 php的模板引擎smarty
### 24.1 什么是模板引擎

---

### 24.2 自定义模板引擎
#### 24.2.1 自定义模板引擎类
#### 24.2.2 使用自己的模板引擎
```php
<?php
    /**
		file: mytpl.class.php 类名为MyTpl是自定义的模板引擎
		通过该类对象加载模板文件并解析，将解析后的结果输出 
	*/
	class MyTpl {
		public $template_dir = 'templates';       //定义模板文件存放的目录  
		public $compile_dir = 'templates_c';      //定义通过模板引擎组合后文件存放目录
		public $left_delimiter  =  '<{';          //在模板中嵌入动态数据变量的左定界符号
		public $right_delimiter =  '}>';          //在模板中嵌入动态数据变量的右定界符号
		private $tpl_vars = array();              //内部使用的临时变量
		
        /** 
			将PHP中分配的值会保存到成员属性$tpl_vars中，用于将板中对应的变量进行替换  
			@param	string	$tpl_var	需要一个字符串参数作为关联数组下标，要和模板中的变量名对应    
			@param	mixed	$value		需要一个标量类型的值，用来分配给模板中变量的值  	
		*/
		function assign($tpl_var, $value = null) {   
			if ($tpl_var != '')                   
				$this->tpl_vars[$tpl_var] = $value;
		}
		
        /** 
			加载指定目录下的模板文件，并将替换后的内容生成组合文件存放到另一个指定目录下
			@param	string	$fileName	提供模板文件的文件名                                         	
		*/
         function display($fileName) { 
			/* 到指定的目录中寻找模板文件 */
			$tplFile = $this->template_dir.'/'.$fileName;  
			/* 如果需要处理的模板文件不存在,则退出并报告错误 */
			if(!file_exists($tplFile)) {               	
				die("模板文件{$tplFile}不存在！");
			}
            /* 获取组合的模板文件，该文件中的内容都是被替换过的 */
			$comFileName = $this->compile_dir."/com_".$fileName.'.php';  
            /* 判断替换后的文件是否存在或是存在但有改动，都需要重新创建 */
			if(!file_exists($comFileName) || filemtime($comFileName) < filemtime($tplFile)) {
				/* 调用内部替换模板方法 */
				$repContent = $this->tpl_replace(file_get_contents($tplFile));  
				/* 保存由系统组合后的脚本文件 */
				file_put_contents($comFileName, $repContent);
			}
			/* 包含处理后的模板文件输出给客户端 */
			include($comFileName);		      	
		}
        
		/**  
			内部使用的私有方法，使用正则表达式将模板文件'<{ }>'中的语句替换为对应的值或PHP代码 
			@param	string	$content	提供从模板文件中读入的全部内容字符串   
			@return	$repContent			返回替换后的字符串
		*/
		private function tpl_replace($content) {
			/* 将左右定界符号中，有影响正则的特殊符号转义  例如，<{ }>转义\<\{ \}\> */
			$left = preg_quote($this->left_delimiter, '/');
			$right = preg_quote($this->right_delimiter, '/');

			/* 匹配模板中各种标识符的正则表达式的模式数组 */
			$pattern = array(       
				/* 匹配模板中变量 ,例如，"<{ $var }>"  */
				'/'.$left.'\s*\$([a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*)\s*'.$right.'/i', 	
				/* 匹配模板中if标识符，例如 "<{ if $col == "sex" }> <{ /if }>" */
				'/'.$left.'\s*if\s*(.+?)\s*'.$right.'(.+?)'.$left.'\s*\/if\s*'.$right.'/ies', 
				/* 匹配elseif标识符, 例如 "<{ elseif $col == "sex" }>" */
				'/'.$left.'\s*else\s*if\s*(.+?)\s*'.$right.'/ies', 
				/* 匹配else标识符, 例如 "<{ else }>" */
				'/'.$left.'\s*else\s*'.$right.'/is',   
				/* 用来匹配模板中的loop标识符，用来遍历数组中的值,  例如 "<{ loop $arrs $value }> <{ /loop}>" */
				'/'.$left.'\s*loop\s+\$(\S+)\s+\$([a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*)\s*'.$right.'(.+?)'.$left.'\s*\/loop\s*'.$right.'/is',
				/* 用来遍历数组中的键和值,例如 "<{ loop $arrs $key => $value }> <{ /loop}>"  */
				'/'.$left.'\s*loop\s+\$(\S+)\s+\$([a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*)\s*=>\s*\$(\S+)\s*'.$right.'(.+?)'.$left.'\s*\/loop \s*'.$right.'/is', 
				/* 匹配include标识符, 例如，'<{ include "header.html" }>' */
				'/'.$left.'\s*include\s+[\"\']?(.+?)[\"\']?\s*'.$right.'/ie'                	
			);
			
			/* 替换从模板中使用正则表达式匹配到的字符串数组 */
			$replacement = array(  
				/* 替换模板中的变量 <?php echo $this->tpl_vars["var"]; */
				'<?php echo $this->tpl_vars["${1}"]; ?>',      
				/* 替换模板中的if字符串 <?php if($col == "sex") { ?> <?php } ?> */
				'$this->stripvtags(\'<?php if(${1}) { ?>\',\'${2}<?php } ?>\')', 	
				/* 替换elseif的字符串 <?php } elseif($col == "sex") { ?> */
				'$this->stripvtags(\'<?php } elseif(${1}) { ?>\',"")',  
				/* 替换else的字符串 <?php } else { ?> */
				'<?php } else { ?>',   
				/* 以下两条用来替换模板中的loop标识符为foreach格式 */
				'<?php foreach($this->tpl_vars["${1}"] as $this->tpl_vars["${2}"]) { ?>${3}<?php } ?>',  
				'<?php foreach($this->tpl_vars["${1}"] as $this->tpl_vars["${2}"] => $this->tpl_vars["${3}"]) { ?>${4}<?php } ?>',    
				/*替换include的字符串*/
				'file_get_contents($this->template_dir."/${1}")'         	
			);
			
			/* 使用正则替换函数处理 */
			$repContent = preg_replace($pattern, $replacement, $content); 	
			/* 如果还有要替换的标识,递归调用自己再次替换 */
			if(preg_match('/'.$left.'([^('.$right.')]{1,})'.$right.'/', $repContent)) {       
				$repContent = $this->tpl_replace($repContent);         	
			} 
			/* 返回替换后的字符串 */
			return $repContent;                                   	
		}
		
         /**
			内部使用的私有方法，用来将条件语句中使用的变量替换为对应的值
			@param	string	$expr		提供模板中条件语句的开始标记           
			@param	string	$statement  提供模板中条件语句的结束标记  
			@return	strin				将处理后的条件语句相连后返回	
		*/
		private function stripvtags($expr, $statement='') {
			/* 匹配变量的正则 */
			$var_pattern = '/\s*\$([a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*)\s*/is'; 
			/* 将变量替换为值 */
			$expr = preg_replace($var_pattern, '$this->tpl_vars["${1}"]', $expr); 
			/* 将开始标记中的引号转义替换 */
			$expr = str_replace("\\\"", "\"", $expr);
			/* 替换语句体和结束标记中的引号 */
			$statement = str_replace("\\\"", "\"", $statement); 
			/* 将处理后的条件语句相连后返回 */
			return $expr.$statement;                         	
		}
	}



```
```php

<{ include "header.html" }>
	
<table border="1" align="center" width="90%" cellpadding="3" cellspacing="0">
	<caption><h1> <{ $tableName }> <h1></caption>
	<tr bgcolor="#cccccc">
		<th>编号</th><th>姓名</th><th>性别</th><th>年龄</th><th>电子邮件</th>
	</tr>
	
	<{ loop $users $user }>
		<tr>	
			<{ loop $user $colKey => $colValue }>
				<{ if $colKey == "sex" }>
					<{ if $colValue=="男" }>
						<td bgColor="red"> <{ $colValue }> </td>
					<{ elseif $colValue=="女" }>
						<td bgColor="green"> <{ $colValue }> </td>
					<{ else }>
						<td bgColor="blue"> 未知 </td>
					<{ /if }>
				<{ else }>
					<td> <{ $colValue }> </td>
				<{ /if }>
			<{ /loop }>
		</tr>
	<{ /loop }>	
	
</table>
<center>共查找到<b> <{ $rowNum }> </b>条记录</center>

<{ include 'footer.html' }>
 

```
```php
<html>
	<head>
		<meta http-equiv="content-type" content="text/html; charset=utf-8" />
		<title> <{ $title }> </title>
	</head>
	<body>


```
```php


```
```php
		<hr><center> ############### 作者：<{ $author }> ############## </center>
	</body>
</html>


```
```php


```
```php
<?php
	/* 包含模板引擎类所在文件 */
	require "mytpl.class.php";                                    	      
	
	/* 创建PDO对象并连接数据库 */
	try {
		$pdo = new PDO("mysql:host=localhost;dbname=mydb", "root", "123456"); 
	}catch(PDOException $e) {
		die("连接失败：".$e->getMessage());                         
	}

	/* 从数据表user中获取全部记录，以二维数组的格式保存到$users变量中 */
	$stmt = $pdo -> prepare("SELECT id, name, sex, age, email FROM user ORDER BY id");	
	$stmt -> execute();
	$users = $stmt -> fetchAll(PDO::FETCH_ASSOC);
	
	$tpl=new MyTpl;          						//创建模板引擎类对象
	
	$tpl->assign("title", "自定义模板引擎示例");    //分配标题变量给头部模板header.tpl
	$tpl->assign("tableName", "用户信息表");        //分配表名变量给主模板
	$tpl->assign("author", "高洛峰");               //分配作者变量给尾部模板footer.tpl
	$tpl->assign("users", $users);                  //分配存有表User的二维数组给主模板
	$tpl->assign("rowNum", $stmt->rowCount());      //分配所取的数据行数变量给主模板
	
	$tpl->display("main.html");                     //包括替换模板中的变量输出模板页面
	
	
	
	
	


```
```php
<?php
	/* 注意Smarty.class.php中的'S'是大写的，并指定了Smarty.class.php所在位置 */
	require './libs/Smarty.class.php';
	/* 实例化Smarty类的对象$smarty */
	$smarty = new Smarty();

```
```php
<?php
	/**
		file: init.inc.php Smarty对象的实例化及初使化文件
	*/
	define("ROOT", str_replace("\\", "/",dirname(__FILE__)).'/');//指定项目的根路径
	require ROOT.'libs/Smarty.class.php';                        //加载Smarty类文件         
	$smarty = new Smarty();                                      //实例化Smarty类的对象$smarty

    /* 推荐使用Smarty3以上版本方式设置默认的路径,设置成功后都返回$smarty对象本身，可以使用连贯操作 */
    $smarty ->setTemplateDir(ROOT.'templates/')                  //设置所有模板文件存放的目录
  //		->addTemplateDir(ROOT.'templates2/')                		 //可以添加多个模板目录（前后台各一个）
            ->setCompileDir(ROOT.'templates_c/')                 //设置所有编译过的模板文件存放的目录
            ->setPluginsDir(ROOT.'plugins/')                     //设置为模板扩充插件存放的目录
            ->setCacheDir(ROOT.'cache/')                         //设置缓存文件存放的目录
            ->setConfigDir(ROOT.'configs');                      //设置模板配置文件存放的目录
			

	$smarty->caching = false;                             		 //设置Smarty缓存开关功能
	$smarty->cache_lifetime = 60*60*24;                 		 //设置模板缓存有效时间段的长度为1天
	$smarty->left_delimiter = '<{';                      		 //设置模板语言中的左结束符
	$smarty->right_delimiter = '}>';                     		 //设置模板语言中的右结束符

	


```
```php
<html>
	<head>
		<meta http-equiv="Content-type" content="text/html; charset=utf-8">
		<title> {$title} </title>
	</head>
	<body>
		{$content}
	</body>
</html>


```
```php
<?php
	/* 第一步：加载自定义的Smarty初使化文件 */
	require "init.inc.php";                	       
	/* 第二步：用assign()方法将变量置入模板里 */                                              	
	$smarty->assign("title", "测试用的网页标题");  
	/* 也属于第二步，分配其他变量置入模板里,可以向模板中置入任何类型的变量 */
	$smarty->assign("content", "测试用的网页内容"); 	
    /* 利用Smarty对象中的display()方法将网页输出  */                                         	
	$smarty->display("test.htm");                   		



```
```php
<html>
	<head>
		<meta http-equiv="Content-type" content="text/html; charset=utf-8">
		<title> <{$title}> </title>
	</head>
	<body>
		<{$content}>
	</body>
</html>


```
```php

<?php
    $smarty = new Smarty();
	$smarty->assign('articleTitle', 'Smokers are Productive, but Death Cuts Efficiency.');
	$smarty->display('index.tpl');


```
```php
<?php
	/* 加载Smarty初使化文件 */
	include "init.inc.php";
	/* 向模板中分配一个字符串变量var */
	$smarty -> assign("var", "这是一个字符串的数据，看看样式变量");
	
	/* 使用registerPlugin()方法，将函数test()动态注册为模板中可以使用的修改器mystyle函数 */
	$smarty -> registerPlugin("modifier", "mystyle", "test");
	/* 声明的一个函数，为Smarty扩充修改器 */
	function test($var, $color, $size) {
		return '<font color="'.$color.'" size="'.$size.'">'. $var .'</font>';
	}
	
	/* 加载并显示模板 */
	$smarty ->display("test.tpl");

```
```php
<?php
	/* 加载Smarty初使化文件 */
	include "init.inc.php";
	/* 向模板中分配一个字符串变量var */
	$smarty -> assign("var", "这是一个字符串的数据，看看样式变量");
	
	/* 直接将PHP系统函数substr()，注册成在模板中可以使用的变量修改器函数substr */
	$smarty -> registerPlugin("modifier", "substr", "substr");
	
	/* 因为PHP系统函数preg_match()函数的第一个参数不是要处理的变量，所以自定义demo()函数重新设置 */
	$smarty -> registerPlugin("modifier", "regrep", "demo");
	
	/* 自定义一个demo()函数重新设置一个参数位置，让需要处理的变量作为第一个参数 */
	function demo($var, $reg, $text) {
		return preg_match($reg, $text, $var);
	}
	
	/* 加载并显示模板 */
	$smarty ->display("test.tpl");

```


```php
<?php
	/* 自定义修改器mystyle */
	function smarty_modifier_mystyle($var, $color, $size) {
		return	'<font color="'.$color.'" size="'.$size.'">'. $var .'</font>';
	}

```

```php
<?php
	/* 使用registerPlugin()方法，将函数print_current_date()动态注册模板标签date_now函数 */
	$smarty -> registerPlugin("function", "date_now", "print_current_date");
	
	/* 自定义一个PHP函数， 作为回调函数， 成为Smarty的扩展标签 */
	function print_current_date($params, $smarty) {
		if(empty($params["format"])) {
			$format = "%b %e, %Y";
		} else {
			$format = $params["format"];
		}
		return strftime($format, time());
	}

```

```php
<?php
	/**
		Smarty 插件
		-----------------------------------------------------------
		文件：	function.eightball.php
		类型：	function
		名称：	eightball
		目标：	输出一个随机的奇秒答案
		----------------------------------------------------------- */
	function smarty_function_eightball($params, $smarty) {
		$answers = array(
				'是的',
				'不行',
				'没门儿',
				'看起来不好',
				'再回答一次',
				'根据情况决定'
			);
		$result = array_rand($answers);
		return	$answers[$result];
	}

```

```php
<?php
	/**
		Smarty 插件
		-----------------------------------------------------------
		文件：	function.eightball.php
		类型：	function
		名称：	eightball
		目标：	输出一个随机的奇秒答案
		----------------------------------------------------------- */
	function smarty_block_translate($params, $content, $smarty, &repeat) {
		//仅输出关闭标签
		if(!$repeat) {
			if(isset($content)) {
				$lang = $params['lang'];
				//在这个位置可以做一些输出$content的内容
				return $translation;
			}
		}
	}

```

```php
<?php
	include "libs/Smarty.class.php";
	$smarty = new Smarty;
	
	$smarty -> caching = true;					//启用缓存
	$smarty -> setCacheDir("./cache");			//指定缓存文件保存的目录
	$smarty -> display('index.tpl');			//也会把输出保存

```

```php
<?php
	include "libs/Smarty.class.php";
	$smarty = new Smarty;
	
	$smarty -> caching = 2;						//启用缓存,在获取模板之前设置缓存生存时间
	$smarty -> setCacheDir("./cache");			//指定缓存文件保存的目录
	$smarty -> cache_lifetime = 60*60*24*7;		//设置缓存时间为1周
	$smarty -> display('index.tpl');			//也会把输出保存

```

```php
<?php
	include "libs/Smarty.class.php";
	$smarty = new Smarty;
	
	$smarty -> caching = 1;									   //启用缓存
	$smarty -> setCacheDir("./cache");						   //指定缓存文件保存的目录
	$smarty -> cache_lifetime = 60*60*24*7;					   //设置缓存时间为1周
	/*
		$news = $db -> getNews($_GET["newid"]);					   //通过表单获取的新闻标题
		$smarty->assign('newsid', $news->getNewTitle()); 			   //向模板中分配新闻标题
		$smarty->assign('newsdt', $news->getNewDataTime()); 		   //向模板中分配新闻时间
		$smarty->assign('newsContent', $news->getNewContent());	   //向模板中分配新闻主体内容
	*/
	$smarty -> display('index.tpl', $_SERVER['REQUEST_URI']);  //将新闻ID作为第二个参数提供

```

```php

<?php
	$smarty -> caching = true;		
	
	//判断模板文件index.tpl是否已经被缓存
	if(!$smarty -> isCached("index.tpl")) {
		//调用数据库，并对变量进行赋值，消除了处理数据库的开销
	}
	
	$smarty -> display('index.tpl');
```

```php


```

```php
<?php
	include "libs/Smarty.class.php";
	$smarty = new Smarty;
	
	$smarty -> caching = 1;									   //启用缓存
	$smarty -> setCacheDir("./cache");						   //指定缓存文件保存的目录
	$smarty -> cache_lifetime = 60*60*24*7;					   //设置缓存时间为1周
	
	//判断news.tpl的某个实例是否被缓存
	if(!$smarty -> isCached('news.tpl', $_SERVER["REQUEST_URI"])) {
	/*
		$news = $db -> getNews($_GET["newid"]);					   //通过表单获取的新闻标题
		$smarty->assign('newsid', $news->getNewTitle()); 			   //向模板中分配新闻标题
		$smarty->assign('newsdt', $news->getNewDataTime()); 		   //向模板中分配新闻时间
		$smarty->assign('newsContent', $news->getNewContent());	   //向模板中分配新闻主体内容
	*/
	}
	
	$smarty -> display('index.tpl', $_SERVER['REQUEST_URI']);  //将新闻ID作为第二个参数提供

```

```php


```

```php


```





#### 24.2.3 应用自定义模板引擎的示例分析

---

### 24.3 选择smarty模板引擎

---

### 24.4 安装smarty及初始化配置
#### 24.4.1 安装smarty
#### 24.4.2 初始化smarty类库的默认设置
#### 24.4.3 第一个smarty的简单示例

---

### 24.5 smarty的基本应用
#### 24.5.1 php程序员常用和smarty相关的操作
#### 24.5.2 模板设计时美工的常用操作

---

### 24.6 smarty模板设计的基本语法
#### 24.6.1 模板中的注释
#### 24.6.2 模板中的变量应用
#### 24.6.3 模板中的函数应用
#### 24.6.4 忽略smarty解析

---

### 24.7 在smarty模板中的变量应用
#### 24.7.1 从配置文件中读取变量
#### 24.7.2 在模板中使用保留变量

---

### 24.8 在smarty模板中的变量调解器
#### 24.8.1 变量调解器函数的使用方式
#### 24.8.2 smarty默认提供的变量调解器
#### 24.8.3 自定义变量调解器插件

---

### 24.9 smarty模板中自定义函数
#### 24.9.1 为smarty模板扩充函数插件
#### 24.9.2 为smarty模板扩充块函数插件

---

### 24.10 smarty模板中的内置函数
#### 24.10.1 变量声明
#### 24.10.2 流程控制
#### 24.10.3 声明和调用模板函数
#### 24.10.4 数组遍历
#### 24.10.5 smarty提供的其他内置函数

---

### 24.11 smarty的模板继承特性
#### 24.11.1 使用{extends}函数实现模板继承
#### 24.11.2 在子模板中覆盖父模板中的部分内容区域
#### 24.11.3 合并子模板和父模板的{block}标签内容

---

### 24.12 smarty的缓存控制
#### 24.12.1 在smarty中控制缓存
#### 24.12.2 每个模板多个缓存
#### 24.12.3 为缓存实例消除处理开销
#### 24.12.4 清除缓存
#### 24.12.5 关闭局部缓存

---

### 24.13 小结

---

- [ ] 本章必须掌握的知识点
- [ ] 本章需要了解的内容
- [ ] 本章需要拓展的内容