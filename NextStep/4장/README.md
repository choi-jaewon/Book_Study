# π© HTTP μΉ μλ² κ΅¬ν

## μκ΅¬μ¬ν­ 1 - index.html μλ΅νκΈ°

```java
public void run() {
	log.debug("New Client Connect! Connected IP : {}, Port : {}", connection.getInetAddress(),
     connection.getPort());

	try (InputStream in = connection.getInputStream(); OutputStream out = connection.getOutputStream()) {
	  BufferedReader br = new BufferedReader(new InputStreamReader(in, "UTF-8"));
	  String line = br.readLine();
    log.debug("request line: {}", line);

    if (line == null) {
	    return;
    }

		String[] tokens = line.split(" ");

    while (!line.equals("")) {
	    line = br.readLine();
      log.debug("header : {}", line);
		}

		DataOutputStream dos = new DataOutputStream(out);
    byte[] body = Files.readAllBytes(new File("./webapp" + tokens[1]).toPath());
    response200Header(dos, body.length);
    responseBody(dos, body);
	}
[...]
```

**μ½μ νλ©΄**

```java
16:49:11.032 [DEBUG] [Thread-0] [webserver.RequestHandler] - New Client Connect! Connected IP : /0:0:0:0:0:0:0:1, Port : 63062
16:49:11.032 [DEBUG] [Thread-1] [webserver.RequestHandler] - New Client Connect! Connected IP : /0:0:0:0:0:0:0:1, Port : 63063
16:49:11.917 [DEBUG] [Thread-0] [webserver.RequestHandler] - line: GET /index.html HTTP/1.1
16:49:11.917 [DEBUG] [Thread-0] [webserver.RequestHandler] - line: Host: localhost:8080
16:49:11.917 [DEBUG] [Thread-0] [webserver.RequestHandler] - line: Connection: keep-alive
[...]
```

- ν΄λΌμ΄μΈνΈλ‘λΆν° 2κ°μ μμ²­μ΄ λ°μνλ©°, μλ‘ λ€λ₯Έ portλ‘ μ°κ²°
- μλ²λ κ° μμ²­μ λμνλ Threadλ₯Ό μμ±ν΄μ λμμ μ€ν
- μμ²­μ μ²« λ²μ§Έ λΌμΈμ `GET /index.html HTTP/1.1` μ νν
- μ²« λ²μ§Έ λΌμΈμ μ μΈν λλ¨Έμ§ μμ²­ λ°μ΄ν°λ `<νλ μ΄λ¦>: <νλ κ°>` νν
- μμ²­μ λ§μ§λ§μ λΉ λ¬Έμμ΄(`ββ`)λ‘ κ΅¬μ±

> HTML μμ²­μ νλ² λ³΄λ΄λλΌλ κ·Έ HTML μλ΅ λ΄μ©μ CSS, μλ°μ€ν¬λ¦½νΈ, μ΄λ―Έμ§ λ±μ μμμ΄ ν¬ν¨λμ΄ μμΌλ©΄ μλ²μ ν΄λΉ μμμ λ€μ μμ²­νκ³ , μ¬λ¬ λ²μ μμ²­κ³Ό μλ΅μ μ£Όκ³  λ°μ
> 

### HTTP κ·μ½

```python
# μμ²­ λΌμΈ
POST /user/create HTTP/1.1
# μμ²­ ν€λ
HOST: localhost:8080
Connection-Length: 59
Content-Type: application/x-wwww-form-urlencoded
Accept: */*
# ν€λμ λ³Έλ¬Έ μ¬μ΄μ λΉ κ³΅λ°± λΌμΈ

# μμ²­ λ³Έλ¬Έ
userId=test&password=password
```

- μμ²­ λΌμΈ(Request Line)
    
    μμ²­ λ°μ΄ν°μ μ²« λ²μ§Έ λΌμΈμΌλ‘, `HTTP-λ©μλ URI HTTP-λ²μ ` μΌλ‘ κ΅¬μ±
    
    - HTTP-λ©μλ
        
        μμ²­μ μ’λ₯
        
    - URI
        
        ν΄λΌμ΄μΈνΈκ° μλ²μ μ μΌνκ² μλ³ν  μ μλ μμ²­ μμμ κ²½λ‘
        
    - HTTP-λ²μ 
        
        νμ¬ μμ²­μ HTTP λ²μ , μ£Όλ‘ HTTP/1.1 μ¬μ©
        
- μμ²­ ν€λ(Request Headers)
    
    `<νλμ΄λ¦>: <νλ κ°>` μμΌλ‘ κ΅¬μ±
    
- μν λΌμΈ(Status Line)
    
    μλ²μμ ν΄λΌμ΄μΈνΈλ‘μ μλ΅ λν μμ²­κ³Ό λΉμ·νκ² κ΅¬μ±λμ΄ μλλ°, μ²« λ²μ§Έ λΌμΈμ΄ μν λΌμΈμΌλ‘ λ€λ¦
    
    `HTTP-λ²μ  μνμ½λ μλ΅κ΅¬λ¬Έ` μΌλ‘ κ΅¬μ±
    

## μκ΅¬μ¬ν­ 2 - GET λ°©μμΌλ‘ νμκ°μνκΈ°

μΉ λΈλΌμ°μ λ HTML form νκ·Έ κ΅¬νμ λ°λΌ μμ²­ λΌμΈμ μμ±ν΄ μλ²μ μμ²­μ λ³΄λ

`GET /user/create?usesrId=test&password=password&name=anonymous&email=test%40abc.net HTTP/1.1`

μμ μμ²­ λΌμΈ μμμμ GETμ form νκ·Έ method μμ± κ°μ΄κ³ , μμ²­ URIλ action μμ± κ°

μμ²­ URIμμ `/user/create`λ **κ²½λ‘(path)**, λ¬Όμν λ€μ λ§€κ°λ³μλ **μΏΌλ¦¬ μ€νΈλ§(query string)**μ΄λΌκ³  λΆλ¦

```java
public void run() {
	log.debug("New Client Connect! Connected IP : {}, Port : {}", connection.getInetAddress(),
	  connection.getPort());
	
	try (InputStream in = connection.getInputStream(); OutputStream out = connection.getOutputStream()) {
		[...]

		String url = tokens[1];
		if (url.startsWith("/user/create")) {
			int index = url.indexOf("?");
			String queryString = url.substring(index+1);
      Map<String, String> params = HttpRequestUtils.parseQueryString(queryString);
      User user = new User(params.get("userId"), params.get("password"), params.get("name"), params.get("email"));
      log.debug("User: {}", user);
		} else {
				DataOutputStream dos = new DataOutputStream(out);
		    byte[] body = Files.readAllBytes(new File("./webapp" + url).toPath());
		    response200Header(dos, body.length);
		    responseBody(dos, body);
		}
		[...]
```

μμ§ μλ΅μ λ³΄λ΄μ§ μμμ βμμ λ λ°μ΄ν° μμ" μ΄λΌλ μλ¬ λ©μΈμ§κ° μΆλ ₯λ¨

> **GET λ°©μμ λ¬Έμ μ **
1. μ¬μ©μκ° μλ ₯ν λ°μ΄ν°κ° λΈλΌμ°μ  URL μλ ₯μ°½μ λΈμΆλ¨ (λ³΄μμ΄ μ€μν κ²½μ° **POST** λ°©μ μ νΈ)
2. μμ²­ λΌμΈ κΈΈμ΄μ μ νμ΄ μμ
> 

## μκ΅¬μ¬ν­ 3 - POST λ°©μμΌλ‘ νμκ°μνκΈ°

POST λ°©μμΌλ‘ λ°μ΄ν°λ₯Ό μ λ¬νκΈ° μν΄ form.htmlμ form νκ·Έ method μμ±μ getμμ postλ‘ μμ 

`POST /user/create HTTP/1.1` νμμΌλ‘ μμ²­ λΌμΈμ΄ λ°λκ³ , 

μΏΌλ¦¬ μ€νΈλ§μ HTTP μμ²­μ λ³Έλ¬Έ(body)λ₯Ό ν΅ν΄ μ λ¬λ¨. 

(λ³Έλ¬Έ λ°μ΄ν°μ κΈΈμ΄λ Content-LengthλΌλ νλ μ΄λ¦μΌλ‘ ν€λμ μ λ¬)

```java
public void run() {
	log.debug("New Client Connect! Connected IP : {}, Port : {}", connection.getInetAddress(),
	  connection.getPort());
	
	try (InputStream in = connection.getInputStream(); OutputStream out = connection.getOutputStream()) {
		[...]
		String[] tokens = line.split(" ");
    int contentLength = 0;
    while (!line.equals("")) {
			 log.debug("header: {}", line);
       line = br.readLine();
       if (line.contains("Content-Length")) {
	       contentLength = getContentLength(line);
       }
		}

		String url = tokens[1];
		if ("/user/create".equals(url)) {
			String body = IOUtils.readData(br, contentLength);
      Map<String, String> params = HttpRequestUtils.parseQueryString(body);
      User user = new User(params.get("userId"), params.get("password"), params.get("name"), params.get("email"));
      log.debug("User: {}", user);
		} 
		[...]
}

private int getContentLength(String line) {
	String[] headerTokens = line.split(":");
	return Integer.parseInt(headerTokens[1].trim());
}
[...]
```

- HTTPλ μμ²­ ννμ λ°λΌ GET, POST, HEAD, PUT, DELETE λ± μ¬λ¬ λ©μλλ₯Ό μ§μν¨
- νμ§λ§ κΈ°λ³ΈμΌλ‘ GET, POSTλ§ μ£Όλ‘ μ¬μ©ν¨
    - Why? <form> νκ·Έκ° μ§μνλ method μμ±μ΄ GET, POST λ°μ μκΈ° λλ¬Έμ-
    - λλ¨Έμ§ λ©μλλ μλ²μμ λΉλκΈ° ν΅μ μ λ΄λΉνλ AJAXμμ μ¬μ©

> **GET, POST λ μ€μ λ¬΄μμ μ¬μ©ν κΉ?**
**GET**μ μλ²μ μ‘΄μ¬νλ λ°μ΄ν°λ₯Ό μ‘°ννλ μ­ν λ‘λ§ μ¬μ©
**POST**λ λ°μ΄ν°μ μνλ₯Ό λ³κ²½νλ μμμ ν  λ μ£Όλ‘ μ¬μ©
> 

## μκ΅¬μ¬ν­ 4 - 302 status code μ μ©

κ°λ¨ν λ°©λ²

```java
String url = tokens[1];
if ("/user/create".equals(url)) {
	[...]
	log.debug("User: {}", user);
	url = "/index.html";
}
```

- μμ²­ URL κ°μ β/index.htmlβ λ‘ λ³κ²½νλ©΄ μ²« νλ©΄μ λ³΄μ¬μ€ μ μμ
    - μλ‘κ³ μΉ¨μ λλ₯΄λ©΄ νμκ°μ μμ²­μ λ€μ λ³΄λ΄λ λ¬Έμ μ μ΄ λ°μ
        
        β λΈλΌμ°μ κ° μ΄μ  μμ²­ μ λ³΄λ₯Ό μ μ§νκ³  μκΈ° λλ¬Έμ
        
    - 302 μνμ½λλ₯Ό νμ©ν΄μ ν΄κ²° κ°λ₯

```java
public void run() {
	log.debug("New Client Connect! Connected IP : {}, Port : {}", connection.getInetAddress(),
	  connection.getPort());
	
	try (InputStream in = connection.getInputStream(); OutputStream out = connection.getOutputStream()) {
		[...]

		String url = tokens[1];
		if ("/user/create".equals(url)) {
			String body = IOUtils.readData(br, contentLength);
      Map<String, String> params = HttpRequestUtils.parseQueryString(body);
      User user = new User(params.get("userId"), params.get("password"), params.get("name"), params.get("email"));
      log.debug("User: {}", user);
			DataOutputStream dos = new DataOutputStream(out);
      response302Header(dos, "/index.html");
		} 
		[...]
}

private void response302Header(DataOutputStream dos, String url) {
	try {
	  dos.writeBytes("HTTP/1.1 302 Redirect \r\n");
    dos.writeBytes("Location: " + url + "\r\n");
    dos.writeBytes("\r\n");
  } catch (IOException e) {
    log.error(e.getMessage());
  }
}
[...]
```

- 200 μν μ½λλ₯Ό μ΄μ©νλ©΄ νμκ°μ μμ²­μ μ²λ¦¬ν ν, index.html νμΌμ μ½μ΄ μλ΅μ λ³΄λ΄λ λ°©μμΌλ‘
    
    ν΄λΌμ΄μΈνΈμ μλ² κ°μ μμ²­κ³Ό μλ΅μ΄ ν λ²μ©λ§ λ°μ
    
- 302 μνμ½λλ₯Ό μ΄μ©νλ©΄ νμκ°μ μμ²­μ μ²λ¦¬νκ³  /index.htmlλ‘ 302 μλ΅νκ³ ,
    
    λ€μ /index.html μμ²­ν΄μ νμΌμ μ½μ λ€ 200 μλ΅μ νλ λ°©μμΌλ‘
    
    ν΄λΌμ΄μΈνΈμ μλ² κ°μ μμ²­κ³Ό μλ΅μ΄ λ λ² λ°μ
    

> **2XX** : μ±κ³΅. ν΄λΌμ΄μΈνΈκ° μμ²­ν λμμ μμ νμ¬ μ΄ν΄νκ³  μΉλνμΌλ©° μ±κ³΅μ μΌλ‘ μ²λ¦¬
**3XX** : λ¦¬λ€μ΄λ μ. ν΄λΌμ΄μΈνΈλ μμ²­μ λ§μΉκΈ° μν΄ μΆκ° λμμ΄ νμν¨
**4XX** : μ€λ₯. ν΄λΌμ΄μΈνΈμ μ€λ₯κ° μμ
**5XX** : μλ² μ€λ₯. μλ²κ° μ ν¨ν μμ²­μ λͺλ°±νκ² μννμ§ λͺ»νμ
> 

## μκ΅¬μ¬ν­ 5 - λ‘κ·ΈμΈνκΈ°

### **λ¬΄μν νλ‘ν μ½**

HTTPλ ν΄λΌμ΄μΈνΈμ μλ² κ°μ μμ²­, μλ΅μ΄ λλλ©΄ μ°κ²°μ λλλ€.

(HTTP 1.1 λΆν° νλ² λ§Ίμ μ°κ²°μ μ¬μ¬μ©νκΈ΄ νμ§λ§, κ° μμ²­κ°μ μν λ°μ΄ν°λ₯Ό κ³΅μ ν  μλ μλ€.)

β λ°λΌμ, μλ²κ° μμμ ν΄λΌμ΄μΈνΈκ° ν νλμ κΈ°μ΅ν  μ μλ€.

β μ΄λ₯Ό ν΄κ²°νκΈ° μν΄ μΏ ν€(Cookie)κ° μ¬μ©λ¨

### μΏ ν€

μλ²μμ μμ²­μ λν μλ΅ ν€λμ Set-Cookieλ‘ κ²°κ³Ό κ°μ μ μ₯ν΄μ μ μ‘

ν΄λΌμ΄μΈνΈλ μλ΅ ν€λμ Set-Cookieκ° μ‘΄μ¬νλ κ²½μ°, Set-Cookie κ°μ μ½μ΄μ

μλ²μ λ³΄λ΄λ μμ²­ ν€λμ Cookie ν€λ κ°μΌλ‘ λ€μ μ μ‘

```java
public void run() {
	log.debug("New Client Connect! Connected IP : {}, Port : {}", connection.getInetAddress(),
	  connection.getPort());
	
	try (InputStream in = connection.getInputStream(); OutputStream out = connection.getOutputStream()) {
		[...]

		String url = tokens[1];
		if ("/user/create".equals(url)) {
			[...]
      User user = new User(params.get("userId"), params.get("password"), params.get("name"), params.get("email"));
      DataBase.addUser(user);
		} else if (url.equals("/user/login")) {
	    String body = IOUtils.readData(br, contentLength);
      Map<String, String> params = HttpRequestUtils.parseQueryString(body);
      User user = DataBase.findUserById(params.get("userId"));
      if (user == null) {
	      responseResource(out, "/user/login_failed.html");
      }
      if (user.getPassword().equals(params.get("password"))) {
	      DataOutputStream dos = new DataOutputStream(out);
        response302LoginSuccessHeader(dos);
      } else {
        responseResource(out, "/user/login_failed.html");
      }
    } else {
	    responseResource(out, url);
    }
		[...]
}

private void responseResource(OutputStream out, String url) throws IOException {
	DataOutputStream dos = new DataOutputStream(out);
  byte[] body = Files.readAllBytes(new File("./webapp" + url).toPath());
  response200Header(dos, body.length);
  responseBody(dos, body);
}

private void response302LoginSuccessHeader(DataOutputStream dos) {
	try {
	  dos.writeBytes("HTTP/1.1 302 Redirect \r\n");
    dos.writeBytes("Location: /index.html" + "\r\n");
    dos.writeBytes("Set-Cookie: logined=true \r\n");
    dos.writeBytes("\r\n");
  } catch (IOException e) {
    log.error(e.getMessage());
  }
}
[...]
```

νμκ°μν μ¬μ©μλ₯Ό DataBase ν΄λμ€λ₯Ό ν΅ν΄μ λΆλ¬μ€κ³ , 

λ‘κ·ΈμΈμ΄ μ±κ³΅νλ©΄ μλ΅ ν€λμ Set-Cookie ν€λ κ°μΌλ‘ logined=trueλ₯Ό μ λ¬

`Cookie: logined=true`

## μκ΅¬μ¬ν­ 6 - μ¬μ©μ λͺ©λ‘ μΆλ ₯

Cookieμ ν€λ κ°μ νμ©ν΄ νμ¬ μμ²­μ λ³΄λ΄κ³  μλ ν΄λΌμ΄μΈνΈκ° λ‘κ·ΈμΈμ ν μνμΈμ§ νλ¨

```java
public void run() {
	log.debug("New Client Connect! Connected IP : {}, Port : {}", connection.getInetAddress(),
	  connection.getPort());
	
	try (InputStream in = connection.getInputStream(); OutputStream out = connection.getOutputStream()) {
		[...]
		String[] tokens = line.split(" ");
		boolean logined = false;
    while (!line.equals("")) {
			 log.debug("header: {}", line);
       line = br.readLine();
       if (line.contains("Cookie")) {
	       logined = isLogin(line);
       }
		}
		
		String url = tokens[1];
		if ("/user/create".equals(url)) {
			[...]
		} else if (url.equals("/user/login")) {
			[...]
		} else if (url.equals("/user/list")) {
	    if (!logined) {
	      responseResource(out, "/user/login.html");
        return;
      }
      Collection<User> users = DataBase.findAll();
      StringBuilder sb = new StringBuilder();
      sb.append("<table border='1'>");
      for (User user : users) {
	      sb.append("<tr>");
        sb.append("<td>" + user.getUserId() + "</td>");
        sb.append("<td>" + user.getName() + "</td>");
        sb.append("<td>" + user.getEmail() + "</td>");
        sb.append("</tr>");
      }
      sb.append("</table>");
      byte[] body = sb.toString().getBytes();
      DataOutputStream dos = new DataOutputStream(out);
      response200Header(dos, body.length);
      responseBody(dos, body);
		}
[...]

private boolean isLogin(String line) {
	String[] headerTokens = line.split(":");
  Map<String, String> cookies = HttpRequestUtils.parseCookies(headerTokens[1].trim());
  String value = cookies.get("logined");
  if (value == null) {
		return false;
	}
  return Boolean.parseBoolean(value);
}
```

μΏ ν€μ κ²½μ°μλ μλ²κ° μ λ¬νλ μΏ ν€ μ λ³΄λ₯Ό ν΄λΌμ΄μΈνΈμ μ μ₯νκΈ° λλ¬Έμ λ³΄μ μ΄μκ° μμ.

β λ³΄μμ κ°ννκΈ° μν΄ μΏ ν€μ λΉμ·νμ§λ§ μλ²μ μ μ₯νλ μΈμμ νμ©νκΈ°λ ν¨

## μκ΅¬μ¬ν­ 7 - CSS μ§μνκΈ°

λΈλΌμ°μ λ μλ΅ λ°μ ν, Content-Type ν€λ κ°μ ν΅ν΄ μλ΅ λ³Έλ¬Έμ ν¬ν¨λμ΄ μλ μ»¨νμΈ κ° μ΄λ€ μ»¨νμΈ μΈμ§λ₯Ό νλ¨ν¨.

μμ²­ URLμ νμ₯μκ° cssμΈ κ²½μ°μ Content-Type ν€λ κ°μ text/cssλ‘ μλ΅μ λ³΄λ΄μ€μΌ CSSκ° μ λλ‘ μ μ©

```java
public void run() {
	log.debug("New Client Connect! Connected IP : {}, Port : {}", connection.getInetAddress(),
	  connection.getPort());
	
	try (InputStream in = connection.getInputStream(); OutputStream out = connection.getOutputStream()) {
		[...]
	
		String url = tokens[1];
		if ("/user/create".equals(url)) {
			[...]
		} else if (url.endWith(".css")) {
			DataOutputStream dos = new DataOutputStream(out);
      byte[] body = Files.readAllBytes(new File("./webapp" + url).toPath());
      response200CssHeader(dos, body.length);
      responseBody(dos, body);
		}
		[...]
}

private void response200CssHeader(DataOutputStream dos, int lengthOfBodyContent) {
	try {
	  dos.writeBytes("HTTP/1.1 200 OK \r\n");
    dos.writeBytes("Content-Type: text/css\r\n");
    dos.writeBytes("Content-Length: " + lengthOfBodyContent + "\r\n");
    dos.writeBytes("\r\n");
	} catch (IOException e) {
    log.error(e.getMessage());
  }
}
```

**λ©νλ°μ΄ν°**

: μ€μ  λ°μ΄ν°κ° μλ λ°μ΄ν°μ λν μ λ³΄λ₯Ό λ΄κ³  μλ ν€λ μ λ³΄λ€
