---
title: '关于Struts2中提交出现乱码的问题'
date: 2019-03-26 19:43:17
tags:
 - web
 - Struts2
categories:
 - Struts2
---

在 Struts2 中出现乱码，在多次试探之后发现是在提交和读取的时候出现了编码不一致的错误。

由于提交的时候是由 utf-8 编码，而在读取的时候却是采用了 GBK 读取，由于编码上不一致导致乱码。

而要解决乱码，第一是要看网页的编码方式，在以下两行中
```html
<%@ page language="java" contentType="text/html; charset=utf-8"%>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
```
可以采用过滤器的方式进行统一设置。
```java
import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebFilter(filterName = "CodeFilter", urlPatterns = { "/*" })
public class CodeFilter implements Filter {

	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws ServletException, IOException {
		HttpServletRequest rq = (HttpServletRequest) request;
		HttpServletResponse rp = (HttpServletResponse) response;
		rq.setCharacterEncoding("utf-8");
		rp.setCharacterEncoding("utf-8");
		rp.setContentType("text/html;charset=utf-8");

		rp.setHeader("Cache-Control", "no-cache");
		rp.setHeader("Pragma", "no-cache");
		rp.setDateHeader("expires", -1);
		chain.doFilter(request, response);
	}
}
```

另一方面便是可能是你的 Struts2 的配置文件中出现错误。
修改 Struts2 的配置文件

```xml
<constant name="struts.i18n.encoding" value="UTF-8"></constant>
```
检查这一行是否与使用的编码相同

完成以上两步，乱码问题基本就能解决了。
