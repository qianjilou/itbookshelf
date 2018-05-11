## 第20章 php访问mysql的扩展函数

---

### 20.1 php访问mysql数据库服务器的流程

---

### 20.2 在php脚本中连接mysql服务器  
#### 20.2.1 在php程序中选择已创建的数据库  
#### 20.2.2 执行sql命令  
#### 20.2.3 在php脚本中处理select查询结果集  
```php
<?php
	$link = mysql_connect('localhost', 'root', 'mysql_password');
	
	if(!$link) {
		die('连接失败: '.mysql_error());
	}

```


```php
<?php
	$link = mysql_connect('localhost', 'root', 'mysql_password');
	if(!$link) {
		die('连接失败: '.mysql_error());
	}
	
	echo "与MySQL服务器建立的连接成功:<br>";       	//连接成功则会输出这条提示信息
	
    echo mysql_get_client_info();                   //客户端API函数库的版本信息
    echo mysql_get_host_info();                     //与MySQL服务器的连接类型
    echo mysql_get_proto_info();                    //通信协议的版本信息
    echo mysql_get_server_info();                   //MySQL服务器的版本信息
    echo mysql_client_encoding();                   //客户端使用的默认字符集
    echo mysql_stat();                              //MySQL服务器的当前工作状态
 
	mysql_close($link);                             //关闭与MySQL服务器建立的连接


```

```php
<?php
	$link = mysql_connect('localhost', 'root', 'mysql_password');
	if(!$link) {
		die('连接失败: '.mysql_error());
	}
	
	//为后续的mysql扩展函数的操作选定一个默认的数据库，它相当于SQL命令use bookstore
    mysql_select_db('bookstore', $link) or die ('不能选定数据库 bookstore : ' . mysql_error());



```
```php

<?php
	$link = mysql_connect('localhost', 'root', 'mysql_password');
	if(!$link) {
		die('连接失败: '.mysql_error());
	}
	
	//为后续的mysql扩展函数的操作选定一个默认的数据库，它相当于SQL命令use bookstore
    mysql_select_db('bookstore', $link) or die ('不能选定数据库 bookstore : ' . mysql_error());

    //将插入3条的INSERT语句声明为一个字符串 
    $insert = "INSERT INTO books(bookName, publisher, author, price, detail) VALUES
	    ('PHP', '电子工业', '高某某', '80.00', '与PHP相关的图书'),
	    ('JSP', '人民邮电', '洛某某', '50.00', '与JSP相关的图书'),
	    ('ASP', '电子工业', '峰某某', '30.00', '与ASP相关的图书')";

    //使用mysql_query()函数发送INSERT语句，如果成功返回TRUE，失败则返回FALSE
    $result = mysql_query($insert);          
    if($result && mysql_affected_rows()>0){
	    echo "数据记录插入成功，最后一条插入的数据记录ID为：".mysql_insert_id()."<br>";
    }else{
	    echo "插入记录失败，错误号：".mysql_errno()."，错误原因：".mysql_error()."<br>";
	}

    //执行UPDATE命令修改表books中的一条记录，将图书名为PHP的记录价格修改为79.90
    $result1 = mysql_query("UPDATE books SET price='79.9' WHERE bookName='PHP'");
    if($result1 && mysql_affected_rows()>0){
	    echo "数据记录修改成功<br>";
    }else{
	    echo "修改数据失败，错误号：".mysql_errno()."，错误原因：".mysql_error()."<br>";
	}

    //执行DELETE命令删除表books中图书名为JSP的记录
    $result2 = mysql_query("DELETE FROM books WHERE bookName='JSP'");
    if($result2 && mysql_affected_rows()>0){
	    echo "数据记录删除成功<br>";
    }else{
	    echo "删除数据失败，错误号：".mysql_errno()."，错误原因：".mysql_error()."<br>";
    }
	
	//关闭与MySQL服务器建立的连接
    mysql_close($link);                              

```
```php
<?php
	$link = mysql_connect('localhost', 'root', 'mysql_password');
	if(!$link) {
		die('连接失败: '.mysql_error());
	}
	
	//为后续的mysql扩展函数的操作选定一个默认的数据库，它相当于SQL命令use bookstore
    mysql_select_db('bookstore', $link) or die ('不能选定数据库 bookstore : ' . mysql_error());

    //执行DQL命令返回结果集$result
    $result = mysql_query("SELECT bookId, bookName, author, publisher,price,detail FROM books"); 
  
	echo '<table align="center" width="80%" border="1">'; 	//以HTML表格输出结果
    echo '<caption><h1>图书信息表</h1></caption>';   		//输出表格标题
    echo '<th>编号</th><th>图书名</th><th>作者</th><th>出版社</th><th>价格</th><th>介绍</th>';
    while($row = mysql_fetch_row($result)){            		//循环从结果集中遍历每条记录到数组中
	    echo '<tr>';                               			//每遍历一条记录输出一个行标记
	    foreach($row as $data){                     		//循环遍历一条数据记录中的每个字段
	    	echo '<td>'.$data.'</td>';              		//以表格形式输出每个字段
	    }
	    echo '</tr>';                               		//输出每行的结束标记
    }
    echo '</table>';
 
	mysql_free_result($result);                       		//释放查询的结果集资源
    mysql_close($link);                             		//关闭与MySQL服务器建立的连接



```

---

### 20.3 设计完美分页类
#### 20.3.1 需求分析
#### 20.3.2 程序设计
#### 20.3.3 完美分页类的代码实现
#### 20.3.4 分页类的应用过程


```php
<?php
	/**
		file: page.class.php 
		完美分页类 Page 
	*/
	class Page {
		private $total;   		 				//数据表中总记录数
		private $listRows; 						//每页显示行数
		private $limit;   		 				//SQL语句使用limit从句,限制获取记录个数
		private $uri;     		 				//自动获取url的请求地址
		private $pageNum; 		 				//总页数
		private $page;							//当前页	
		private $config = array(
							'head' => "条记录", 
							'prev' => "上一页", 
							'next' => "下一页", 
							'first'=> "首页", 
							'last' => "末页"
						); 						//在分页信息中显示内容，可以自己通过set()方法设置
		private $listNum = 10; 					//默认分页列表显示的个数

		/**
			构造方法，可以设置分页类的属性
			@param	int	$total		计算分页的总记录数
			@param	int	$listRows	可选的，设置每页需要显示的记录数，默认为25条
			@param	mixed	$query	可选的，为向目标页面传递参数,可以是数组，也可以是查询字符串格式
			@param 	bool	$ord	可选的，默认值为true, 页面从第一页开始显示，false则为最后一页
		 */
		public function __construct($total, $listRows=25, $query="", $ord=true){
			$this->total = $total;
			$this->listRows = $listRows;
			$this->uri = $this->getUri($query);
			$this->pageNum = ceil($this->total / $this->listRows);
			/*以下判断用来设置当前面*/
			if(!empty($_GET["page"])) {
				$page = $_GET["page"];
			}else{
				if($ord)
					$page = 1;
				else
					$page = $this->pageNum;
			}

			if($total > 0) {
				if(preg_match('/\D/', $page) ){
					$this->page = 1;
				}else{
					$this->page = $page;
				}
			}else{
				$this->page = 0;
			}
			
			$this->limit = "LIMIT ".$this->setLimit();
		}

		/**
			用于设置显示分页的信息，可以进行连贯操作
			@param	string	$param	是成员属性数组config的下标
			@param	string	$value	用于设置config下标对应的元素值
			@return	object			返回本对象自己$this， 用于连惯操作
		 */
		function set($param, $value){
			if(array_key_exists($param, $this->config)){
				$this->config[$param] = $value;
			}
			return $this;
		}
		
		/* 不是直接去调用，通过该方法，可以使用在对象外部直接获取私有成员属性limit和page的值 */
		function __get($args){
			if($args == "limit" || $args == "page")
				return $this->$args;
			else
				return null;
		}
		
		/**
			按指定的格式输出分页
			@param	int	0-7的数字分别作为参数，用于自定义输出分页结构和调整结构的顺序，默认输出全部结构
			@return	string	分页信息内容
		 */
		function fpage(){
			$arr = func_get_args();

			$html[0] = "&nbsp;共<b> {$this->total} </b>{$this->config["head"]}&nbsp;";
			$html[1] = "&nbsp;本页 <b>".$this->disnum()."</b> 条&nbsp;";
			$html[2] = "&nbsp;本页从 <b>{$this->start()}-{$this->end()}</b> 条&nbsp;";
			$html[3] = "&nbsp;<b>{$this->page}/{$this->pageNum}</b>页&nbsp;";
			$html[4] = $this->firstprev();
			$html[5] = $this->pageList();
			$html[6] = $this->nextlast();
			$html[7] = $this->goPage();

			$fpage = '<div style="font:12px \'\5B8B\4F53\',san-serif;">';
			if(count($arr) < 1)
				$arr = array(0, 1,2,3,4,5,6,7);
				
			for($i = 0; $i < count($arr); $i++)
				$fpage .= $html[$arr[$i]];
		
			$fpage .= '</div>';
			return $fpage;
		}
		
		/* 在对象内部使用的私有方法，*/
		private function setLimit(){
			if($this->page > 0)
				return ($this->page-1)*$this->listRows.", {$this->listRows}";
			else
				return 0;
		}

		/* 在对象内部使用的私有方法，用于自动获取访问的当前URL */
		private function getUri($query){	
			$request_uri = $_SERVER["REQUEST_URI"];	
			$url = strstr($request_uri,'?') ? $request_uri :  $request_uri.'?';
			
			if(is_array($query))
				$url .= http_build_query($query);
			else if($query != "")
				$url .= "&".trim($query, "?&");
		
			$arr = parse_url($url);

			if(isset($arr["query"])){
				parse_str($arr["query"], $arrs);
				unset($arrs["page"]);
				$url = $arr["path"].'?'.http_build_query($arrs);
			}
			
			if(strstr($url, '?')) {
				if(substr($url, -1)!='?')
					$url = $url.'&';
			}else{
				$url = $url.'?';
			}
			
			return $url;
		}

		/* 在对象内部使用的私有方法，用于获取当前页开始的记录数 */
		private function start(){
			if($this->total == 0)
				return 0;
			else
				return ($this->page-1) * $this->listRows+1;
		}

		/* 在对象内部使用的私有方法，用于获取当前页结束的记录数 */
		private function end(){
			return min($this->page * $this->listRows, $this->total);
		}

		/* 在对象内部使用的私有方法，用于获取上一页和首页的操作信息 */
		private function firstprev(){
			if($this->page > 1) {
				$str = "&nbsp;<a href='{$this->uri}page=1'>{$this->config["first"]}</a>&nbsp;";
				$str .= "<a href='{$this->uri}page=".($this->page-1)."'>{$this->config["prev"]}</a>&nbsp;";		
				return $str;
			}

		}
	
		/* 在对象内部使用的私有方法，用于获取页数列表信息 */
		private function pageList(){
			$linkPage = "&nbsp;<b>";
			
			$inum = floor($this->listNum/2);
			/*当前页前面的列表 */
			for($i = $inum; $i >= 1; $i--){
				$page = $this->page-$i;

				if($page >= 1)
					$linkPage .= "<a href='{$this->uri}page={$page}'>{$page}</a>&nbsp;";
			}
			/*当前页的信息 */
			if($this->pageNum > 1)
				$linkPage .= "<span style='padding:1px 2px;background:#BBB;color:white'>{$this->page}</span>&nbsp;";
			
			/*当前页后面的列表 */
			for($i=1; $i <= $inum; $i++){
				$page = $this->page+$i;
				if($page <= $this->pageNum)
					$linkPage .= "<a href='{$this->uri}page={$page}'>{$page}</a>&nbsp;";
				else
					break;
			}
			$linkPage .= '</b>';
			return $linkPage;
		}

		/* 在对象内部使用的私有方法，获取下一页和尾页的操作信息 */
		private function nextlast(){
			if($this->page != $this->pageNum) {
				$str = "&nbsp;<a href='{$this->uri}page=".($this->page+1)."'>{$this->config["next"]}</a>&nbsp;";
				$str .= "&nbsp;<a href='{$this->uri}page=".($this->pageNum)."'>{$this->config["last"]}</a>&nbsp;";
				return $str;
			}
		}

		/* 在对象内部使用的私有方法，用于显示和处理表单跳转页面 */
		private function goPage(){
    			if($this->pageNum > 1) {
				return '&nbsp;<input style="width:20px;height:17px !important;height:18px;border:1px solid #CCCCCC;" type="text" onkeydown="javascript:if(event.keyCode==13){var page=(this.value>'.$this->pageNum.')?'.$this->pageNum.':this.value;location=\''.$this->uri.'page=\'+page+\'\'}" value="'.$this->page.'"><input style="cursor:pointer;width:25px;height:18px;border:1px solid #CCCCCC;" type="button" value="GO" onclick="javascript:var page=(this.previousSibling.value>'.$this->pageNum.')?'.$this->pageNum.':this.previousSibling.value;location=\''.$this->uri.'page=\'+page+\'\'">&nbsp;';
			}
		}

		/* 在对象内部使用的私有方法，用于获取本页显示的记录条数 */
		private function disnum(){
			if($this->total > 0){
				return $this->end()-$this->start()+1;
			}else{
				return 0;
			}
		}
	}

	
	
	


```
```php
<?php
	/* 第一步：必须包含分页类所在的文件page.class.php */
	include "page.class.php";
	/* 第二步：实例化分页类对象，并通过参数传递数据表的记录总数 （总记录数需要从数据库查询获取）*/
	$page = new Page(1000);
	
	/* 第三步：通过对象中limit属性，获取LIMIT从句并组合SQL语句，从数据表aritcle中获取当页的数据记录 */
	$sql = "select * from article {$page->limit}";
	echo 'SQL = "'.$sql.'"<p>';         //输出SQL语句
	
	/* 第四步：通过分页对象中的fpage()方法，输出所有分页的结构信息 */
	echo $page->fpage();      
	
	
	
	
	


```
```php

<?php
	include "page.class.php";
	$page = new Page(1000);
	//通过set()方法设置输出信息内容，使用连贯操作调用多次set()方法
	$page -> set('head', '篇文件')			//改变输出头信息
		  -> set('first', '|<')				//将首页改成'|<'
		  -> set('prev', '|<<')				//将上一页改成'|<<'
		  -> set('next', '>>|')				//将下一页改成'>>|'
		  -> set('last', '>|');				//将尾页改成'>|'
		  
	$sql = "SELECT * FROM article {$page->limit}";
	echo 'SQL = "'.$sql.'"<p>';        
	
	echo $page->fpage();      
	
	
	
	

```
```php

<?php
	include "page.class.php";
	$page = new Page(1000);

	$sql = "SELECT * FROM article {$page->limit}";
	echo 'SQL = "'.$sql.'"<p>';        
	
	//在fpage()方法中使用5个参数，设置只显示输出指定的5个部分，并调整了输出顺序
	echo $page->fpage(3, 4, 5, 6, 0);      
	
	
	
	

```
```php

<?php
	include "page.class.php";
	/* 
		通过第二个参数设置每页显示10条数据
		通过第三个参数设置跳转页面传递两个参数过去，也可以使用数组array("cid"=>5,"search"=>"php")
		通过第四个参数设置默认显示最后一页
	*/
	$page = new Page(1000, 10, 'cid=5&search=php', false);

	$sql = "SELECT * FROM article {$page->limit}";
	echo 'SQL = "'.$sql.'"<p>';        
	
	echo $page->fpage();      
	
	
	
	

```

---

### 20.4 管理books表实例
#### 20.4.1 需求分析
#### 20.4.2 程序设计

```php
<?php /** file: index.php 程序的主控制文件和主入口文件 */ ?>
<html>
	<head>
		<title>图书表管理</title>
		<meta http-equiv="Content-Type" content="text/html;charset=utf-8" />
		<style>
			body {font-size:12px;}
			td {font-size:12px;}
		</style>
	<head>
	<body>
			<h1>图书表管理</h1>
			<p>
				<a href="index.php?action=add">添加图书</a> ||
				<a href="index.php?action=list">图书列表</a> ||
				<a href="index.php?action=ser">搜索图书</a> <hr>
			</p>
			<?php
				/* 包含自定义的函数库文件 */
				include "func.inc.php";
				/* 如果用户的操作是请求添加图书表单action=add，则条件成立 */
				if($_GET["action"] == "add") {
					/* 包含add.inc.php获取用户添加表单 */
					include "add.inc.php";
				/* 如果用户提交添加表单action=insert，则条件成立 */
				} else if ($_GET["action"] == "insert") {
					/*在这里可以加上数据验证*/
					
					/* 使用func.inc.php文件中声明的 upload()函数处理图片上传 */
					$up = upload();
					/* 如果返回值$up中的第一个元素是false说明上传失败，报告错误原因并退出程序 */
					if(!$up[0]) 
						die($up[1]);
					
					/* 添加数据需要先连接并选数据库，包含conn.inc.php文件连接数据库 */
					include "conn.inc.php";
					
					/* 根据用户通过POST提交的数据组合插入数据库的SQL语句 */
					$sql = "INSERT INTO books(bookname, publisher, author, price, ptime,pic,detail) VALUES('{$_POST["bookname"]}', '{$_POST["publisher"]}', '{$_POST["author"]}', '{$_POST["price"]}', '".time()."', '{$up[1]}', '{$_POST["detail"]}')";
					/* 执行INSERT语句 */
					$result = mysql_query($sql);
					/* 如果INSERT语句执行成功，并对数据表books有行数影响，则插入数据成功 */
					if($result && mysql_affected_rows() > 0 ) {
						echo "插入一条数据成功!";
					}else {
						echo "数据录入失败!";
					}
					/* 用完后关闭数据库的连接 */
					mysql_close($link);
				/* 如果用户请求一个修改表单action=mod, 则条件成立 */
				} else if($_GET["action"] == "mod") {
					/* 包含文件mod.inc.php获取一个修改表单 */
					include "mod.inc.php";
				} else if($_GET["action"] == "update") {
					/*在这里加上数据验证*/
					
					/* 如果用户需要修改图片，用新上传的图片替换原来的图片 */
					if($_FILES["pic"]["error"] == "0"){
						$up = upload();
						/* 如果有新上传的图片，就使用上传图片名修改数据库 */
						if($up[0])  
							$pic = $up[1];
						else 
							die($up[1]);
								
					} else {
						/* 如果没有上传图片，还是使用原来图片 */
						$pic = $_POST["picname"];
					}
					/* 修改数据需要先连接并选数据库，包含conn.inc.php文件连接数据库 */
					include "conn.inc.php";
					
					/* 根据修改表单提交的POST数据组合一个UPDATE语句 */
					$sql = "UPDATE books SET bookname='{$_POST["bookname"]}', publisher='{$_POST["publisher"]}', author='{$_POST["author"]}', price='{$_POST["price"]}',pic='{$pic}', detail='{$_POST["detail"]}' WHERE id='{$_POST["id"]}'";
		
					/* 执行UPDATE语句 */
					$result = mysql_query($sql);
					
					/* 如果语句执行成功，并对记录行有所影响，则表示修改成功 */
					if($result && mysql_affected_rows() > 0 ) {
						/* 修改新图片成功后，将原来的图片要删除掉，以免占用磁盘空间 */
						if($up[0]) 
							delpic($_POST["picname"]);
						echo "记录修改成功!";
					}else {
						echo "数据修改失败!";
					}
					mysql_close($link);
				/* 如果用户请求删除一本图书action=del, 则条件成立 */
				} else if($_GET["action"] == "del") {
									
					include "conn.inc.php";
					$result = mysql_query("DELETE FROM books WHERE id='{$_GET["id"]}'");
					if($result && mysql_affected_rows() > 0 ) {
						/*删除记录成功后，也要将图书的图片一起删除 */
						delpic($_GET["pic"]);
						/* 删除记录后跳转回到原来的URL */
						echo '<script>window.location="'.$_SERVER["HTTP_REFERER"].'"</script>';
					}else {
						echo "数据删除失败!";
					}
		
					mysql_close($link);
				/* 如果用户请求一个搜索表单action=ser, 则条件成立 */
				} else if($_GET["action"] == "ser"){
					include "ser.inc.php";
				/* 默认的请求都是图书列表 */
				} else {
					include "list.inc.php";
				}
			?>
	</body>
</html>


```
```php


```
```php
<?php
    /** file: conn.inc.php 数据库连接文件 */
	/*连接本地的数据库*/
	$link = mysql_connect("localhost", "root", "123456");
				
	if (!$link) {
		die('连接数据库失败: '.mysql_error());
	}
	/* 选择bookstore作为默认的数据库 */
	if(!mysql_select_db("bookstore")) {
		die('数据库选择失败: '.mysql_error());
	}


```

```php
<?php
	/** file: func.inc.php 函数库文件 */

	include "fileupload.class.php";                            //导入文件上传类FileUpload所在文件
	include "image.class.php";                                 //导入图片处理类Image所在的文件
	
	/* 声明一个函数upload()处理图片上传 */
	function upload(){
		$path = "./uploads/";                                  //设置图片上传路径
		
		$up = new FileUpload($path);                           //创建文件上传类对象
		
		if($up->upload('pic')) {                               //上传图片
			$filename = $up->getFileName();                    //获取上传后的图片名
			
			$img = new Image($path);                           //创建图像处理类对象
			
			$img -> thumb($filename, 300, 300, "");            //将上传的图片都缩放至在300X300以内
			$img -> thumb($filename, 80, 80, "icon_");         //缩放一个80x80的图标，使用icon_作前缀
			$img -> watermark($filename, "logo.gif", 5, "");   //为上传的图片加上图片水印
			
			return array(true, $filename);                     //如果成功返回成功状态和图片名称
		} else {
			return array(false, $up->getErrorMsg());           //如果失败返回失败状态和错误消息
		}
	}
	/* 删除上传的图片 */
	function delpic($picname){
		$path = "./uploads/"; 

		@unlink($path.$picname);                                //删除原图
		@unlink($path.'icon_'.$picname);                        //删除图标
	}
	
	


```

```php


```


---

### 20.5 php的mysqli扩展介绍
#### 20.5.1 启用mysqli扩展模块
#### 20.5.2 mysqli扩展接口的应用概述
```php


```

```php


```




---

### 20.6 小结
- [ ] 本章必须掌握的知识点
- [ ] 本章需要了解的内容
- [ ] 本章需要拓展的内容
- [ ] 本章的学习建议