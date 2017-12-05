# phpcms
### phpcms的手机站
- 首先给phpcms添加自适配的代码，找到并打开/modules/content/index.php文件，在里面找到如下代码：include template('content',$template);
- 将其修改为：if(substr($_SERVER['SERVER_NAME'], 0,1) == 'm'){
	 include template('content_m',$template);
	 }else{
	 include template('content',$template);
	 }
-  以上代码的意思是当前页面url中第一个字符为m时则调用content_m模板，否则调用content模板
  但是由于phpcms把文章的url都固定写死在数据表中，所以页面中的标签不能在使用{$r[url]}
  而要改成{str_replace('http://www.','http://m.',$r[url])}
  意思是截取url，把http://www.你的域名/ 替换成http://m.你的域名/
  这里我们就完成了手机版的设置了，然后我们在制作一套手机端模板content_m就可以了。
- 如果我们要在PC端的内容里面加上当前页面手机端的链接，链接地址写法如下：
  http://{str_replace('www.','m.',$_SERVER['SERVER_NAME'])}{$_SERVER['REQUEST_URI']}
- 手机端加上PC端的链接：
 http://{str_replace('m.','www.',$_SERVER['SERVER_NAME'])}{$_SERVER['REQUEST_URI']}
 
### 如果你使用的是静态页面，那么只要在模板页头加上以下JS代码就可以实现判断手机端自动跳转到手机端了。
<script type="text/javascript">
	 function browserRedirect() {
	 var sUserAgent = navigator.userAgent.toLowerCase();
	 var bIsIpad = sUserAgent.match(/ipad/i) == "ipad";
	 var bIsIphoneOs = sUserAgent.match(/iphone os/i) == "iphone os";
	 var bIsMidp = sUserAgent.match(/midp/i) == "midp";
	 var bIsUc7 = sUserAgent.match(/rv:1.2.3.4/i) == "rv:1.2.3.4";
	 var bIsUc = sUserAgent.match(/ucweb/i) == "ucweb";
	 var bIsAndroid = sUserAgent.match(/android/i) == "android";
	 var bIsCE = sUserAgent.match(/windows ce/i) == "windows ce";
	 var bIsWM = sUserAgent.match(/windows mobile/i) == "windows mobile";
	 if (bIsIpad || bIsIphoneOs || bIsMidp || bIsUc7 || bIsUc || bIsAndroid || bIsCE || bIsWM) {
	 {if $catid=='' and $id==''}
	 window.location.href="{APP_PATH}/index.php";
	 {elseif $id=='' and $catid!=''}
	 window.location.href="{APP_PATH}/index.php?m=content&c=index&a=lists&catid={$catid}";
	 {else}
	 window.location.href="{APP_PATH}/index.php?m=content&c=index&a=show&catid={$catid}&id={$id}";
	 {/if}
	 }
	 }
	 browserRedirect();
	function closewindow() {
	 $("#register-box").hide();
	 }
	 function openwindow() {
	 $("#register-box").show();
	 }
	 </script>
