
###页面更换背景

![image](https://raw.githubusercontent.com/qianjilou/jQuery/master/images/huanbeijing.gif "页面更换背景")


```javascript

$(function(){

	var imgList = {

		0 : {
			"preview" : {
				0 : "./img/0/preview/1.jpg",
				1 : "./img/0/preview/2.jpg",
				2 : "./img/0/preview/3.jpg"
			},
			"theme" : {
				0 : "url(./img/0/theme/1.jpg)",
				1 : "url(./img/0/theme/2.jpg)",
				2 : "url(./img/0/theme/3.jpg)",
			}
		},
		1 : {
			"preview" : {
				0 : "./img/1/preview/1.jpg",
				1 : "./img/1/preview/2.jpg"
			},
			"theme" : {
				0 : "url(./img/1/theme/1.jpg)",
				1 : "url(./img/1/theme/2.jpg)",
			}
		},
		2 : {
			"preview" : {
				0 : "./img/2/preview/1.jpg"
			},
			"theme" : {
				0 : "url(./img/2/theme/1.jpg)"
			}
		}

	};


	var index = 0;
	$(".tab-header li").click(function(){
		index = $(this).index();
		$(".tab-body .item").eq(index).addClass("active").siblings(".item").removeClass("active");
		$(".tab-header li").eq(index).addClass("active").siblings("li").removeClass("active");
		toggleBg();
	});

	toggleBg();

	function toggleBg () {
		$(".tab-body .item li").click(function(){
			var index2 = $(this).index();
			$("body").css( "background", imgList[index]['theme'][index2] + " center top no-repeat" );
			$(".tab-body .preview img").attr( "src", imgList[index]['preview'][index2] );
		});
	}

	

});

```


HTML部分
```html

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>2345换皮肤</title>
	<script src="../js/jquery.min.js"></script>
	<script src="1.js"></script>
	<link rel="stylesheet" href="./1.css">
</head>
<body>
	<div class="tab">
		<div class="tab-header">
			<ul>
				<li class="active"><a href="javascript:;">最热</a></li>
				<li><a href="javascript:;">护眼</a></li>
				<li><a href="javascript:;">中国风</a></li>
			</ul>	
		</div>
		<div class="clear"></div>
		<div class="tab-body">
			<div class="tab-thumbnail">
				<div class="item active">
					<ul>
						<li><a href="javascript:;"><img src="./img/0/thumb/1.jpg" alt=""></a></li>
						<li><a href="javascript:;"><img src="./img/0/thumb/2.jpg" alt=""></a></li>
						<li><a href="javascript:;"><img src="./img/0/thumb/3.jpg" alt=""></a></li>
					</ul>
				</div>	
				<div class="clear"></div>
				<div class="item">
					<ul>
						<li><a href="javascript:;"><img src="./img/1/thumb/1.jpg" alt=""></a></li>
						<li><a href="javascript:;"><img src="./img/1/thumb/2.jpg" alt=""></a></li>
					</ul>
				</div>
				<div class="clear"></div>
				<div class="item">
					<ul>
						<li><a href="javascript:;"><img src="./img/2/thumb/1.jpg" alt=""></a></li>
					</ul>
				</div>
			</div>	
			<div class="tab-preview">
				<div class="preview">
					<img src="./img/0/preview/1.jpg" alt="">
					<span class="grid"></span>
				</div>	
			</div>
		</div>		
	</div>	
</body>
</html>

```
