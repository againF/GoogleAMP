# AMP中CSS的使用规范
## 不允许的样式
1. 内联样式。所有样式必须定义在`<head>`中的`<style amp-custom>`中
2. `!important`禁止使用。因为AMP要求所有元素事先要给定宽高，这个属性会破坏这个规则。
3. `<link rel="stylesheet">`。禁止引用外部样式。
4. `-amp-` class and `i-amp-` tag names。自定义的class不要用`-amp-`开头，这个是预留给AMP runtime的。
## 允许使用但是有使用限制的样式
1. `transition` property。只能使用GPU加速的属性（当前包括`opacity`,`transform` and `-vendorPrefix-transform`)
2. `@keyframes {...}`。只能使用GPU加速的属性（当前包括`opacity`,`transform` and `-vendorPrefix-transform`)
## AMP页面不能引用外部样式除了自定义字体
两种方式引用自定义字体  

1. Through a `<link>` tag (white-listed font providers only)
2. Via `@font-face` (no restrictions, all fonts allowed)
### 1.Using`<link>`
	<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Tangerine">
下面是字体白名单
  
* Typography.com: https://cloud.typography.com
* Fonts.com: https://fast.fonts.net
* Google Fonts: https://fonts.googleapis.com
* Typekit: https://use.typekit.net
* Font Awesome: https://maxcdn.bootstrapcdn.com
### 2.Using `@font-face`
	<style amp-custom>
	  @font-face {
	    font-family: "Bitstream Vera Serif Bold";
	    src: url("https://somedomain.org/VeraSeBd.ttf");
	  }
	
	  body {
	    font-family: "Bitstream Vera Serif Bold", serif;
	  }
	</style>
>NOTE-Fonts included via `@font-face` must be fetched via the HTTP or HTTPS scheme.
## 使用CSS预处理
