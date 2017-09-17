

## 51

换一波

```javascript
$(function(){
	var index = 0;
	$(".guess .guess-btn").click(function(){
		index++;
		if ( index == $(".guess-body ul").length ) {
			index = 0;
		}
		$(".guess-body ul").eq(index).addClass("active")
		.siblings("ul").removeClass("active");
	});
});
```

