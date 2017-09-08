### 全选与全不选


---
图片
![image](https://raw.githubusercontent.com/qianjilou/jQuery/master/images/quanxuan.gif "全选与全不选")

### 全选与全不选
```javascript
$(function(){
		$("input:eq(0)").click(function(){
			$("#box input").attr( "checked", true );
		});
		$("input:eq(1)").click(function(){
			$("#box input").attr( "checked", false );
		});
		$("input:eq(2)").click(function(){
			$("#box input").each(function(i){
					// alert(i);
				if ( $(this).attr("checked")) {//this代表当前元素
					$(this).attr("checked", false );
				}else{
					$(this).attr("checked", true );
				}
			});
		});
	});
```
```html
<input type="button" value="全选">
	<input type="button" value="全不选">
	<input type="button" value="反选">
	<div id="box">
		<input type="checkbox" name="" id=""><br>
		<input type="checkbox" name="" id=""><br>
		<input type="checkbox" name="" id=""><br>
		<input type="checkbox" name="" id=""><br>
		<input type="checkbox" name="" id=""><br>
		<input type="checkbox" name="" id=""><br>
		<input type="checkbox" name="" id=""><br>
		<input type="checkbox" name="" id=""><br>
		<input type="checkbox" name="" id=""><br>
		<input type="checkbox" name="" id=""><br>
</div>
```
