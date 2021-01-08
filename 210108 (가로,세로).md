```
[ box.html ]


<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

	<form action="box_result.jsp" method="post">
		가로 : <input type="text" name="width"><br />
		세로 : <input type="text" name="height"><br />
		<input type="submit" value="제출">
	</form>
	
</body>
</html>
```

```
[ box_result.jsp ]

방법 1.

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
    
<jsp:useBean id="boxInfo" class="edu.bit.ex.BoxInfo" scope="page" />
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

	<%
		double width = Double.parseDouble(request.getParameter("width"));
		double height = Double.parseDouble(request.getParameter("height"));
	%>
	
	<%
		boxInfo.setHeight(height);
		boxInfo.setWidth(width);
	
		out.println("box의 넓이는 : " + boxInfo.area());
	%>
	
</body>
</html>
```

```
[ box_result.jsp ]

방법 2.


<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
    
<jsp:useBean id="boxInfo" class="edu.bit.ex.BoxInfo" scope="page" />
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

		<jsp:setProperty name="boxInfo" property="width" />
		<jsp:setProperty name="boxInfo" property="height" /> 
	
	<%
		out.println("box의 넓이는 : " + boxInfo.area());
	%>
	
</body>
</html>
```

```
[ BoxInfo.java ]


package edu.bit.ex;

public class BoxInfo {

	private double width;
	private double height;
	
	public BoxInfo() {
		
	}
	
	public double getWidth() {
		return width;
	}



	public void setWidth(double width) {
		this.width = width;
	}



	public double getHeight() {
		return height;
	}



	public void setHeight(double height) {
		this.height = height;
	}

	public double area() {
		return width * height;
	}
}

```

