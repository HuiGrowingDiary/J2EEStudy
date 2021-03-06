##输出

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body>
<%
  out.print("AAA"+"<br>");
  out.print("BBB");
%>
</body>
</html>
```

##HTML请求

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body bgcolor="#ffffff">
  <%
  	  Cookie c=new Cookie("h","Hello");
	  c.setMaxAge(10000); //非临时
	  response.addCookie(c);
  %>
<!--用户代理，使服务器能够识别客户使用的浏览器版本、操作系统、浏览器渲染引擎、浏览器语言等-->
<%=request.getHeader("User-Agent")%><br> 
  ${header["User-Agent"]}<hr>
<%=request.getHeader("Accept")%><br><!-- 客户端能处理的MIME类型 -->
  ${header.Accept }<hr>
<!-- 请求页URL,例如使用<a href="index.jsp">index.jsp</a>来请求本页 -->
<%=request.getHeader("referer")%><br>
  ${header.referer }<hr>
<%=request.getHeader("Cookie")%><br>
  ${cookie.h.value}<hr>
</body>
</html>
```

---

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body bgcolor="#ffffff">
<%=request.getRemoteAddr()%><br><!-- 客户端IP -->
<%=request.getRemoteHost()%><br><!-- 客户端主机名，如无法解析则显示IP -->
<%=request.getRemotePort()%><br><!-- 客户端端口 -->
<%=request.getMethod() %><br>
<%=request.getProtocol() %>
</body>
</html>
```

##getParameter()

``` java
<html>
<body>
	<form action="b.jsp" method="get">
		用户名：<input type="text" name="user"/>
		密码：<input type="password" name="password"/>
		<input type="submit" value="提交">	
	</form>
</body>
</html>
```

---

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body>
<%
	// 取得参数值
	out.println("用户名：" + request.getParameter("user") + "<br/>");
	out.println("密码：" + request.getParameter("password") + "<br/>");
	
	// 取得参数名
	out.print("参数名：");
	java.util.Enumeration paramerNames = request.getParameterNames();
	while(paramerNames.hasMoreElements()) {
		out.print(paramerNames.nextElement() + "&nbsp;");
	}
	out.println("<br/>");
	
	// 取得客户端请求方法
	out.println("请求方法：" + request.getMethod() + "<br/>");
	
	// 取得查询字符串
	out.println("查询字符串：" + request.getQueryString() + "<br/>");
%>
</body>
</html>
```

##getParameterValues()

``` html
<html>
<body>
<form action="b.jsp" method="get">
	选择喜欢的商品<br>
	
	<!--- 注意：传递的是value，所以是“笔记”，而不是“笔记本” --->
	<input type="checkbox" name="hobby" value="笔记" >笔记本
	<input type="checkbox" name="hobby" value="手机" >手机
	<input type="checkbox" name="hobby" value="PDA" >PDA
	<input type="checkbox" name="hobby" value="MP3" >MP3
	<input type="submit" value="提交">
</form>
</body>
</html>
```

---

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body>
<%
	String[] multiValues = request.getParameterValues("hobby");
	
	// 注意：需要注意multiValues是否为空，可能没有任何参数
	for(int i=0;i<multiValues.length;i++)
	  out.print(new String(multiValues[i].getBytes("ISO-8859-1"))+" ");  //汉字

%>
</body>
</html>
```

##setAttribute()

通过```<A>```，```<form>```和```response.sendRedirect()```请求另一个页面，使用```request.getAttribute()```无效，```pageContext.forward("a.jsp")```有效

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body>
<%
	// 增加属性
	request.setAttribute("attr1","sample");
	request.setAttribute("attr2",new Integer(100));
	// 显示属性值
	out.print("属性值一：" + request.getAttribute("attr1") + "<br/>");
	out.print("属性值二：" + request.getAttribute("attr2") + "<br/>");
	// 显示属性名
	out.print("属性名：");
	java.util.Enumeration attributeNames = request.getAttributeNames();
	while(attributeNames.hasMoreElements()) {
		out.print(attributeNames.nextElement() + "&nbsp;");
	}
	out.println("<br/>");
	// 移除属性
	request.removeAttribute("attr1");
	request.removeAttribute("attr2");
%>
</body>
</html>
```

##中文乱码问题

``` html
<%@ page contentType="text/html; charset=gbk" %>
<html>
<body>
<form action="a.jsp" method="post">
  <input type=text name="aaa">
  <input type=submit>
</form>
</body>
</html>
```

---

``` java
<%@ page contentType="text/html; charset=gbk" import="java.net.*" %>
<html>
<body bgcolor="#ffffff">
<%
    //request.setCharacterEncoding("gbk");   //post
    out.print(request.getParameter("aaa"));  //可以含空格
    out.print(new String(request.getParameter("aaa").getBytes("ISO-8859-1"),"gbk" ));  //汉字
    out.print(URLDecoder.decode(URLEncoder.encode(request.getParameter("aaa"), "iso-8859-1"),"gbk"));  //汉字
%>
</body>
</html>
```

##编码解码

``` java
import java.net.URLDecoder;
import java.net.URLEncoder;

public class Hello {
	public static void main(String[] args)throws Exception {
		//java中字符文本是以unicode编码存在，转换成某种字符编码的字节数组
		String a=URLEncoder.encode("中国", "unicode");//unicode即utf-16
		String a1=URLEncoder.encode("中国", "utf-16");
		String b=URLEncoder.encode("中国", "gbk");
		String c=URLEncoder.encode("中国", "iso-8859-1");//不可以用iso-8859-1编码汉字之类字符
		String d=URLEncoder.encode("中国", "utf-8");
		System.out.println(a);
		System.out.println(a1);
		System.out.println(b);
		System.out.println(c);
		System.out.println(d);
		System.out.println(URLDecoder.decode(a,"unicode"));
		System.out.println(URLDecoder.decode(b,"gbk"));
		System.out.println(URLDecoder.decode(c,"iso-8859-1"));
		System.out.println(URLDecoder.decode(d,"utf-8"));
	}
}
```

##response

``` html
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body>
<form action="b.jsp">
  <input type=text name="aaa">
  <input type=submit>
</form>
</body>
</html>
```

---

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body bgcolor="#ffffff">
<%
  if(request.getParameter("aaa").equals("zzz")){
    response.sendRedirect("a.jsp");
  }
  else{
    out.print(request.getParameter("aaa"));  //可以含空格
    //out.print(new String(request.getParameter("aaa").getBytes("ISO-8859-1")));  //汉字
  }
%>
</body>
</html>
```

---

``` java
<%@ page contentType="text/html;charset=gbk"%>
<%@ page import="java.util.*"%>
<html>
	<head>
		<title>定时刷新页面</title>
	</head>
	<body>
		<%
			//设置刷新页面的时间，第隔1秒钟刷新一次
			response.setHeader("refresh", "1");
		%>
		当前的系统时间是：
		<%		
			//输出当前时间
			Calendar now = Calendar.getInstance();
			out.print(now.get(Calendar.YEAR) + "年");
			out.print(now.get(Calendar.MONTH) + 1 + "月");
			out.print(now.get(Calendar.DATE) + "日");
			out.print(now.get(Calendar.HOUR_OF_DAY) + "时");
			out.print(now.get(Calendar.MINUTE) + "分");
			out.print(now.get(Calendar.SECOND) + "秒  ");
			out.print("星期");
			switch (now.get(Calendar.DAY_OF_WEEK)) {
			case 1:
				out.print("日");
				break;
			case 2:
				out.print("一");
				break;
			case 3:
				out.print("二");
				break;
			case 4:
				out.print("三");
				break;
			case 5:
				out.print("四");
				break;
			case 6:
				out.print("五");
				break;
			case 7:
				out.print("六");
			}
		%>
	</body>
</html>
```

---

``` java
<%@ page contentType="text/html;charset=gbk"%>
<h3>3秒后自动转到www.163.com</h3>
<%
  response.setHeader("Refresh","3;URL=http://www.163.com");
%>
```

##HTTP关联

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body>
<%
  application.setAttribute("a","aaa");
  session.setAttribute("b","bbb");
  request.setAttribute("c","ccc");
  pageContext.forward("a.jsp");
%>
</body>
</html>
```

---

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body bgcolor="#ffffff">
<%=application.getAttribute("a")%><br>
<%=session.getAttribute("b")%><br>
<%=request.getAttribute("c")%><br>
</body>
</html>
```

##pageContext

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body>
<%
  pageContext.setAttribute("a","aaa",PageContext.PAGE_SCOPE);
  pageContext.setAttribute("b","bbb",PageContext.REQUEST_SCOPE);
  pageContext.setAttribute("c","ccc",PageContext.SESSION_SCOPE);
  pageContext.setAttribute("d","ddd",PageContext.APPLICATION_SCOPE);
  pageContext.forward("a.jsp");
%>
</body>
</html>
```

---

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body bgcolor="#ffffff">
<%=pageContext.getAttribute("a",PageContext.PAGE_SCOPE)%><br> <!-- PAGE_SCOPE可以省略 -->
<%=pageContext.getAttribute("a")%><br>
<%=pageContext.getAttribute("b",PageContext.REQUEST_SCOPE)%><br>
<%=pageContext.getAttribute("c",PageContext.SESSION_SCOPE)%><br>
<%=pageContext.getAttribute("d",PageContext.APPLICATION_SCOPE)%><br>
</body>
</html>
```

##pageContext.findAttribute()

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body>
<%
  application.setAttribute("a","aaa");
  session.setAttribute("b","bbb");
  pageContext.getServletContext().setAttribute("c","ccc");
  pageContext.getSession().setAttribute("d","ddd");
%>
</body>
</html>
```

---

``` java
<%@ page contentType="text/html; charset=GBK" %>
<html>
<body bgcolor="#ffffff">
<%=pageContext.findAttribute("a")%><br>
<%=pageContext.getAttribute("a",PageContext.APPLICATION_SCOPE)%><br>
<%=pageContext.findAttribute("b")%><br>
<%=pageContext.getAttribute("b",PageContext.SESSION_SCOPE)%><br>
<%=application.getAttribute("c")%><br>
<%=session.getAttribute("d")%>
</body>
</html>
```
