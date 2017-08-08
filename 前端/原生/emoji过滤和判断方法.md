附上相关的好贴


#### 一.js判断文本中是否有emoji表情

```javascript

	function isEmojiCharacter(substring) {  
	    for ( var i = 0; i < substring.length; i++) {  
	        var hs = substring.charCodeAt(i);  
	        if (0xd800 <= hs && hs <= 0xdbff) {  
	            if (substring.length > 1) {  
	                var ls = substring.charCodeAt(i + 1);  
	                var uc = ((hs - 0xd800) * 0x400) + (ls - 0xdc00) + 0x10000;  
	                if (0x1d000 <= uc && uc <= 0x1f77f) {  
	                    return true;  
	                }  
	            }  
	        } else if (substring.length > 1) {  
	            var ls = substring.charCodeAt(i + 1);  
	            if (ls == 0x20e3) {  
	                return true;  
	            }  
	        } else {  
	            if (0x2100 <= hs && hs <= 0x27ff) {  
	                return true;  
	            } else if (0x2B05 <= hs && hs <= 0x2b07) {  
	                return true;  
	            } else if (0x2934 <= hs && hs <= 0x2935) {  
	                return true;  
	            } else if (0x3297 <= hs && hs <= 0x3299) {  
	                return true;  
	            } else if (hs == 0xa9 || hs == 0xae || hs == 0x303d || hs == 0x3030  
	                    || hs == 0x2b55 || hs == 0x2b1c || hs == 0x2b1b  
	                    || hs == 0x2b50) {  
	                return true;  
	            }  
	        }  
	    }  
	} 
```

#####二.js过滤emoji表情


使用JS过滤emoji表情的主要原因：input标签中输入emoji表情，提交表单后插入数据库报错。 
原因是因为UTF-8编码有可能是两个、三个、四个字节。Emoji表情是4个字节，而MySQL的utf8编码最多3个字节，所以数据插不进去。 
于是找到两个解决方案： 
1.将MySQL的编码从utf8转换成utf8mb4 
2.前端JS校验过滤掉emoji表情

下面主要粘下过滤emoji的JS代码


```javascript

	function filteremoji(){
	    var ranges = [
	        '\ud83c[\udf00-\udfff]', 
	        '\ud83d[\udc00-\ude4f]', 
	        '\ud83d[\ude80-\udeff]'
	    ];
	    var emojireg = $("#emoji_input").val();
	    emojireg = emojireg .replace(new RegExp(ranges.join('|'), 'g'), ''));
	}
```