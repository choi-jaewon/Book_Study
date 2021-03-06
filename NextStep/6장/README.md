# **π§ μλΈλ¦Ώ/JSPλ‘ νμκ΄λ¦¬ κΈ°λ₯ λ€μ κ°λ°νκΈ°**
## **π² μλΈλ¦Ώ/JSP λ³΅μ΅**
### **νμκ°μ**

`/user/form.html` μ κ·Έλλ‘ μ¬μ©νλ€. μ¬μ©μ μλ ₯ λ°μ΄ν°λ₯Ό μΆμΆν ν DBμ μΆκ°νλ λ°©μμ΄λ€.

```java
@WebServlet("/user/create")
public class CreateUserServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    private static final Logger log = LoggerFactory.getLogger(CreateUserServlet.class);

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        User user = new User(req.getParameter("userId"), req.getParameter("password"), req.getParameter("name"),
                req.getParameter("email"));
        log.debug("user : {}", user);
        DataBase.addUser(user);
        resp.sendRedirect("/user/list");
    }
}
```

- νμκ°μμ μλ£ν ν μ¬μ©μ λͺ©λ‘ μΆλ ₯μ μν΄ `"/user/list"` λ‘ λ¦¬λ€μ΄λ νΈνλ€.

### **μ¬μ©μ λͺ©λ‘**

νμκ°μν  λ μ μ₯ν μ¬μ©μ λͺ©λ‘μ μ‘°νν ν JSPμ `"users"` λΌλ μ΄λ¦μΌλ‘ μ λ¬νλ€. μ΄μ  μ₯μμλ `ListUserController` μμ λμ μΌλ‘ `StringBuilder`λ₯Ό μ΄μ©νμ¬ HTMLμ μμ±νμ§λ§ μ¬κΈ°μλ JSPνμΌλ‘ μμνλ€.

```java
@WebServlet("/user/list")
public class ListUserServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setAttribute("users", DataBase.findAll());
        RequestDispatcher rd = req.getRequestDispatcher("/user/list.jsp");
        rd.forward(req, resp);
    }
}
```

- μλΈλ¦Ώλ λμ μΌλ‘ HTMLμ μμ±νκΈ° μν΄μλ 5μ₯μ `ListUserController` μ κ°μ λ°©μμΌλ‘ κ΅¬νν΄μΌ νλ€.
- μ΄μ κ°μ μλΈλ¦Ώμ νκ³λ₯Ό κ·Ήλ³΅νκΈ° μν΄ JSPκ° λ±μ₯νλ€.

### **JSP**

μ μ μΈ HTMLμ κ·Έλλ‘ λκ³ , λμ μΌλ‘ λ³κ²½λλ λΆλΆλ§μ κ΅¬ννλ€. **Java Server Page**λΌλ μ΄λ¦λ΅κ² μλ° κ΅¬λ¬Έμ κ·Έλλ‘ μ¬μ©ν  μ μλ€. (**μ€ν¬λ¦½νλ¦Ώ(scriptlet)**μ΄λΌκ³  νλ `<% %>` λ΄μ μ¬μ©)

νμ§λ§ μΉ μ νλ¦¬μΌμ΄μ μκ΅¬μ¬ν­μ λ³΅μ‘λκ° μ¦κ°νλ©΄μ **λ§μ λ‘μ§**μ΄ κ΅¬νλλ€λ³΄λ **JSPμ μ μ§λ³΄μκ° μ΄λ €μμ§λ λ¬Έμ **κ° λ°μνλ€.

μ΄λ₯Ό ν΄κ²°νκΈ° μν΄ **JSTL(JavaServer Pages Standard Tag Library)κ³Ό EL(Expression Language)κ° λ±μ₯**νκ³ , JSPμ λ³΅μ‘λλ₯Ό λ?μΆκΈ° μν΄ **MVC ν¨ν΄μ μ μ©ν νλ μμν¬κ° λ±μ₯**νλ€.

```jsx
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

...

<c:forEach items="${users}" var="user" varStatus="status">
    <tr>
        <th scope="row">${status.count}</th>
        <td>${user.userId}</td>
        <td>${user.name}</td>
        <td>${user.email}</td>
        <td><a href="#" class="btn btn-success" role="button">μμ </a>
        </td>
    </tr>
</c:forEach>

...
```

- **JSTLκ³Ό EL**μ νμ©νλ©΄ JSPμμ μλ° κ΅¬λ¬Έμ μμ ν μ κ±°ν  μ μλ€.
    - μ¬μ€ μ΄λ₯Ό μν΄μλ JSPκ° μΆλ ₯ν  λ°μ΄ν°(ex. `users`)λ₯Ό μ λ¬ν΄μ€ **μ»¨νΈλ‘€λ¬κ° νμ**νλ€. μ¦, **MVC ν¨ν΄ κΈ°λ°μΌλ‘ κ°λ°**ν΄μΌ μλ° κ΅¬λ¬Έμ μμ ν μ κ±°ν  μ μλ€λ κ²μ΄λ€.
    - μ¬κΈ°μλ μ¬μ©μ λͺ©λ‘ μ‘°ν ν JSPμ μ λ¬νλ `ListUserServlet` μ΄ MVC ν¨ν΄μμ μ»¨νΈλ‘€λ¬ μ­ν μ μννλ€.

## **π€Ά κ°μΈμ λ³΄μμ  μ€μ΅**

κ°μΈμ λ³΄μμ  κΈ°λ₯μ λν κ΅¬νμ μμνλ€. μμ  νλ©΄μ νμκ°μ νλ©΄μΈ `/user/form.html` μ μ¬μ¬μ©νλ€.

`list.jsp`

```html
<li><a href="../user/updateForm.jsp" role="button">κ°μΈμ λ³΄μμ </a></li>
```

- λ¨Όμ , κ°μΈμ λ³΄μμ  λ²νΌ ν΄λ¦­ μ, `../user/updateForm.jsp` λ‘ μ΄λνλλ‘ μ§μ 

`updateForm.jsp`

```html
<div class="container" id="main">
    <div class="col-md-6 col-md-offset-3">
        <div class="panel panel-default content-main">
            <form name="question" method="post" action="/user/update">
                <input type="hidden" name="userId" value="${user.userId}" />
                <div class="form-group">
                    <label>μ¬μ©μ μμ΄λ</label>
                    ${user.userId}
                </div>
                <div class="form-group">
                    <label for="password">λΉλ°λ²νΈ</label>
                    <input type="password" class="form-control" id="password" name="password" placeholder="Password">
                </div>
                <div class="form-group">
                    <label for="name">μ΄λ¦</label>
                    <input class="form-control" id="name" name="name" placeholder="Name" value="${user.name}">
                </div>
                <div class="form-group">
                    <label for="email">μ΄λ©μΌ</label>
                    <input type="email" class="form-control" id="email" name="email" placeholder="Email" value="${user.email}">
                </div>
                <button type="submit" class="btn btn-success clearfix pull-right">κ°μΈμ λ³΄μμ </button>
                <div class="clearfix" />
            </form>
        </div>
    </div>
</div>
```

- `/user/update` λ‘ POST μμ²­μ λ³΄λ΄λ `form` νκ·Έ μμ±

`UpdateUserServlet.java`

```java
@WebServlet(value = {"/user/update", "/user/updateForm"})
public class UpdateUserServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    private static final Logger log = LoggerFactory.getLogger(CreateUserServlet.class);

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        RequestDispatcher rd = req.getRequestDispatcher("/user/updateForm.jsp");
        rd.forward(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        User user = DataBase.findUserById(req.getParameter("userId"));

        User updateUser = new User(req.getParameter("userId"), req.getParameter("password"), req.getParameter("name"),
                req.getParameter("email"));
        log.debug("updateUser : {}", updateUser);
        user.update(updateUser);
        resp.sendRedirect("/user/list");
    }
}
```

- `/user/updateForm` λ‘μ GET μμ²­κ³Ό, `/user/update` λ‘μ POST μμ²­μ μ²λ¦¬νκΈ° μν΄ `value` μ¬μ© (The URL patterns of the servlet)
- `userId` λ₯Ό μ΄μ©ν΄ DBμμ μ¬μ©μλ₯Ό μ‘°ννκ³  μλ ₯λ°μ κ°μΌλ‘ `user.update` μν

> π‘ μ GETκ³Ό POSTλ₯Ό λ°λ URLμ λ€λ₯΄κ² νλμ§?

`model/User.java`

```java
public void update(User updateUser) {
    this.password = updateUser.password;
    this.name = updateUser.name;
    this.email = updateUser.email;
}
```

- `userId` λ₯Ό μ μΈν μ¬μ©μ μ λ³΄ updateλ₯Ό μν λ©μλ

## **π³ββοΈ λ‘κ·ΈμΈ/λ‘κ·Έμμ κΈ°λ₯ μ€μ΅**

νμ¬ μνκ° λ‘κ·ΈμΈ μνμ΄λ©΄ μλ¨ λ©λ΄κ° **λ‘κ·Έμμ**, **κ°μΈμ λ³΄μμ **μ΄ λνλμΌ νλ©°, λ‘κ·Έμμ μνμ΄λ©΄ **λ‘κ·ΈμΈ**, **νμκ°μ**μ΄ λνλμΌ νλ€.

`LoginController.java`

```java
@WebServlet(value = { "/users/login", "/users/loginForm" })
public class LoginController extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        forward("/user/login.jsp", req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String userId = req.getParameter("userId");
        String password = req.getParameter("password");
        User user = DataBase.findUserById(userId);

        if (user == null) {
            req.setAttribute("loginFailed", true);
            forward("/user/login.jsp", req, resp);
            return;
        }

        if (user.matchPassword(password)) {
            HttpSession session = req.getSession();
            session.setAttribute(UserSessionUtils.USER_SESSION_KEY, user);
            resp.sendRedirect("/");
        } else {
            req.setAttribute("loginFailed", true);
            forward("/user/login.jsp", req, resp);
        }
    }

    private void forward(String forwardUrl, HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        RequestDispatcher rd = req.getRequestDispatcher(forwardUrl);
        rd.forward(req, resp);
    }
}
```

- `/user/login`, `/user/loginForm` μ λν μλ΅μ μ²λ¦¬νλ€.
- **GET μμ²­**μ κ²½μ°, `/user/login.jsp` λ₯Ό λ°ννλ€.
- **POST μμ²­**μ κ²½μ°, μλ ₯λ idλ₯Ό μ΄μ©ν΄ DBμ μ‘΄μ¬νλ μ¬μ©μμΈμ§ μ‘°ννκ³ , λΉλ°λ²νΈκΉμ§ μΌμΉνλ©΄ `HttpSession` μΌλ‘ sessionμ μ¬μ©μλ₯Ό μΆκ°νλ€.
    - `USER_SESSION_KEY` : μ μ₯νλ `user` λ₯Ό μλ³νκΈ° μν ν€
    - λμ€μ μνΈνλ₯Ό νκ² λλ€λ©΄, 
    IDλ₯Ό λ¨Όμ  μ‘°νν΄μ DBμ μ μ₯λ PWDλ₯Ό κ°μ Έμ€κ³ , 
    μ¬μ©μ μλ ₯ κ°μ μνΈνν κ°κ³Όμ λΉκ΅λ₯Ό μν

`include/~.jspf`

```java
<c:if test="${not empty sessionScope.user}">
    <ul class="nav dropdown-menu">
        <li><a href="/users/profile?userId=${sessionScope.user.userId}"><i class="glyphicon glyphicon-user" style="color:#1111dd;"></i> Profile</a></li>
        <li class="nav-divider"></li>
        <li><a href="#"><i class="glyphicon glyphicon-cog" style="color:#dd1111;"></i> Settings</a></li>
    </ul>
</c:if>

<c:choose>
<c:when test="${not empty sessionScope.user}">
<li><a href="/users/logout" role="button">λ‘κ·Έμμ</a></li>
<li><a href="/users/updateForm?userId=${sessionScope.user.userId}" role="button">κ°μΈμ λ³΄μμ </a></li>
</c:when>
<c:otherwise>
<li><a href="/users/loginForm" role="button">λ‘κ·ΈμΈ</a></li>
<li><a href="/users/form" role="button">νμκ°μ</a></li>
</c:otherwise>
</c:choose>
```

- **JSP**μμ sessionμ λ‘κ·ΈμΈν μ¬μ©μ μ¬λΆλ₯Ό μ²΄ν¬νκ³  **JSTL**μμ λΆκΈ°λ¬Έ μ²λ¦¬νμ¬ λ‘κ·ΈμΈν μ¬μ©μμ κ·Έλ μ§ μμ μ¬μ©μκ° μ¬μ©ν  μ μλ κΈ°λ₯μ λ€λ₯΄κ² μ²λ¦¬νλ€.

`LogoutController.java`

```java
@WebServlet("/users/logout")
public class LogoutController extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession();
        session.removeAttribute(UserSessionUtils.USER_SESSION_KEY);
        resp.sendRedirect("/");
    }
}
```

- `HttpSession` μ λ λ €λ²λ¦¬λ κ²μΌλ‘ λ‘κ·Έμμμ κ΅¬ννλ€.

## **πββοΈ νμ λͺ©λ‘ λ° κ°μΈμ λ³΄ μμ  λ³΄μ κ°ν μ€μ΅**

**λ‘κ·ΈμΈν μ¬μ©μλ§μ΄ μ¬μ©μ λͺ©λ‘ μ‘°ν**κ° κ°λ₯ν΄μΌ νλ©°, **κ°μΈμ λ³΄ μμ μ μμ μ μ λ³΄λ§** μμ  κ°λ₯ν΄μΌ νλ€.

`ListUserController.java`

```java
if (!UserSessionUtils.isLogined(req.getSession())) {
    resp.sendRedirect("/users/loginForm");
    return;
}
```

- sessionμ λ‘κ·ΈμΈν μ¬μ©μκ° μλμ§ νμΈνλ€.
    - λ‘κ·ΈμΈν μ¬μ©μκ° μλ κ²½μ°, `loginForm`μΌλ‘ λ¦¬λ€μ΄λ μνλ€.

`UpdateUserController.java`

```java
if (!UserSessionUtils.isSameUser(req.getSession(), user)) {
    throw new IllegalStateException("λ€λ₯Έ μ¬μ©μμ μ λ³΄λ₯Ό μμ ν  μ μμ΅λλ€.");
}
```

- μ λ³΄ μμ  μ, μμ νλ €λ μ¬μ©μκ° νμ¬ λ‘κ·ΈμΈν λ³ΈμΈμΈμ§ νμΈνλ€.

`UserSessionUtils.java`

```java
public class UserSessionUtils {
    public static final String USER_SESSION_KEY = "user";

    public static User getUserFromSession(HttpSession session) {
        Object user = session.getAttribute(USER_SESSION_KEY);
        if (user == null) {
            return null;
        }
        return (User) user;
    }

    public static boolean isLogined(HttpSession session) {
        if (getUserFromSession(session) == null) {
            return false;
        }
        return true;
    }

    public static boolean isSameUser(HttpSession session, User user) {
        if (!isLogined(session)) {
            return false;
        }

        if (user == null) {
            return false;
        }

        return user.isSameUser(getUserFromSession(session));
    }
}
```

- `getUserFromSession` : sessionμ μΈμλ‘ λ°μ sessionμΌλ‘λΆν° `user` attributeλ₯Ό μΆμΆνλ€.
- `isLogined` : `getUserFromSession` λ©μλλ₯Ό νΈμΆνμ¬ κ·Έ λ°νκ°μΌλ‘ λ‘κ·ΈμΈν μ¬μ©μκ° μλμ§ νμΈνλ€.
- `isSameUser` : `isLogined` λ©μλ νΈμΆλ‘ sessionμ λ‘κ·ΈμΈν μ¬μ©μκ° μλμ§μ μΈμλ‘ λ°μ `user` μ `null` μ¬λΆλ₯Ό νμΈνμ¬ `boolean` κ°μ λ°ννλ€. ??? (μ μΈμκ° 1κ°λ§ λκ²¨μ£Όμ§)

---

# **π¨βπ μΈμ(`HttpSession`) μκ΅¬μ¬ν­ λ° μ€μ΅**

μ΄μ  μ₯μμ μΈκΈνλ― **HTTPλ λ¬΄μν νλ‘ν μ½**μ΄λ€. νμ§λ§ λ‘κ·ΈμΈκ³Ό κ°μ΄ μνλ₯Ό μ μ§ν  νμκ° μλ μκ΅¬μ¬ν­μ΄ λ°μνλ€. μ΄μ κ°μ κ²½μ° μ¬μ©ν  μ μλ λ°©λ²μ΄ "μΏ ν€ ν€λλ₯Ό μ¬μ©νλ κ²"μ΄λ€.

`"Set-Cookie"` ν€λλ₯Ό ν΅ν΄ μΏ ν€λ₯Ό μμ±νλ©΄, μ΄ν λ°μνλ λͺ¨λ  μμ²­μ `"Set-Cookie"` λ‘ μΆκ°ν κ°μ "Cookie" ν€λλ‘ μ λ¬νλ λ°©μμ΄λ€.

νμ§λ§ μΏ ν€λ₯Ό μ¬μ©νλ λ° **λ³΄μμ μ·¨μ½νλ€**λ λ¬Έμ μ μ΄ μλ€.

μλλ©΄, λκ΅¬λ λΈλΌμ°μ  κ°λ°μ λκ΅¬λ HTTP λΆμ λκ΅¬λ₯Ό νμ©νμ¬ HTTP μμ²­ λ° μλ΅ ν€λλ₯Ό νμΈν  μ μκΈ° λλ¬Έμ΄λ€. λ°λΌμ, **μΏ ν€λ₯Ό ν΅ν΄ μ€μν κ°μΈμ λ³΄λ₯Ό μ λ¬νλ κ²μ λ³΄μμ μ ν©νμ§ μλ€.**

**Sessionμ λ±μ₯**

μΏ ν€μ λ³΄μμ λ¨μ μ λ³΄μνκΈ° μν΄ **Session**μ΄ λ±μ₯νλ€. μ΄λ μν κ°μΌλ‘ μ μ§νκ³  μΆμ μ λ³΄λ₯Ό ν΄λΌμ΄μΈνΈλ¨μ μ μ₯νλ κ²μ΄ μλ **μλ²λ¨μ μ μ₯**νλ€.

- μλ²μ μ μ₯ ν, κ° ν΄λΌμ΄μΈνΈλ§λ€ κ³ μ ν μμ΄λλ₯Ό λ°κΈν΄ μ΄ μμ΄λλ₯Ό `"Set-Cookie"` ν€λλ₯Ό ν΅ν΄ μ λ¬νλ€.
    - HTTPμμ μνλ₯Ό μ μ§νλ λ°©λ²μ **μΏ ν€**λ°μ μκΈ°μ, κ²°κ΅­μ μν κ° μ μ§λ₯Ό μν **κ° μ λ¬**μλ μΏ ν€λ₯Ό μ¬μ©νλ€.

> "μΈμμ HTTPμ μΏ ν€λ₯Ό κΈ°λ°μΌλ‘ λμνλ€."

## **π§ μκ΅¬μ¬ν­**

μλΈλ¦Ώμμ μ§μνλ `HttpSession` APIμ μΌλΆλ₯Ό μ§μν΄μΌ νλ€. κ΅¬νν  λ©μλλ `getId()`, `setAttribute(String name, Object value)`, `getAttribute(String name)`, `removeAttribute(String name)`, `invalidate()` μ΄λ€.

- `String getId()` : νμ¬ sessionμ ν λΉλμ΄ μλ κ³ μ ν session idλ₯Ό λ°ν
- `void setAttribute(String name, Object value)` : νμ¬ sessionμ `value` μΈμλ‘ μ λ¬λλ κ°μ²΄λ₯Ό `name` μΈμ μ΄λ¦μΌλ‘ μ μ₯
- `Object getAttribute(String name)` : νμ¬ sessionμ `name` μΈμλ‘ μ μ₯λμ΄ μλ κ°μ²΄ κ°μ μ°Ύμ λ°ν
- `void removeAttribute(String name)` : νμ¬ sessionμ `name` μΈμλ‘ μ μ₯λμ΄ μλ κ°μ²΄ κ°μ μ­μ 
- `void invalidate()` : νμ¬ sessionμ μ μ₯λμ΄ μλ λͺ¨λ  κ°μ μ­μ 

## **π§ μκ΅¬μ¬ν­ λΆλ¦¬ λ° ννΈ**

- ν΄λΌμ΄μΈνΈμ μλ² κ° μ£Όκ³  λ°μ κ³ μ ν μμ΄λλ₯Ό μμ±ν΄μΌ νλ€. κ³ μ ν μμ΄λλ μ½κ² μμΈ‘ν  μ μμ΄μΌ νλ€. κ·Έλ μ§ μλ€λ©΄, μΏ ν€ κ°μ μ‘°μν΄ λ€λ₯Έ μ¬μ©μμ²λΌ μμΌ μ μκΈ° λλ¬Έμ΄λ€.
- μμ±ν κ³ μ ν μμ΄λλ₯Ό μΏ ν€λ₯Ό ν΅ν΄ μ λ¬νλ€. (`βSet-Cookieβ` ν€λ)
- μλ² μΈ‘μμ λͺ¨λ  ν΄λΌμ΄μΈνΈμ session κ°μ κ΄λ¦¬νλ μ μ₯μ ν΄λμ€λ₯Ό μΆκ°νλ€. (`Map<String, String>`, **Key**λ μμμ μμ±ν κ³ μ ν μμ΄λ)
- ν΄λΌμ΄μΈνΈλ³ session λ°μ΄ν°λ₯Ό κ΄λ¦¬ν  μ μλ ν΄λμ€(`HttpSession`)λ₯Ό μΆκ°νλ€.
    - ν΄λΉ ν΄λμ€λ μ 5κ°μ λ©μλλ₯Ό λͺ¨λ κ΅¬ννκ³ , μν λ°μ΄ν°λ₯Ό μ μ₯ν  `Map<String, Object>` κ° νμνλ€.

---

# **π©βπ» μΈμ(`HttpSession`) κ΅¬ν**

## **π€΄ κ³ μ ν μμ΄λ μμ±**

sessionμμ μ¬μ©ν  κ³ μ ν μμ΄λλ₯Ό μμ±ν΄ λ³΄μ. λλ€μΌλ‘ μμμ κ°μ μμ±ν  μ μμ§λ§, JDKμμ μ κ³΅νλ `UUID` ν΄λμ€λ₯Ό νμ©νλ€. λ¨Όμ  `UUIDTest` ν΄λμ€λ₯Ό μΆκ°ν΄ μ΄λ€ ννλ‘ μμ±λλμ§ νμΈν΄λ³Έλ€.

`UUID.randomUUID()` λ₯Ό μΆλ ₯ν΄λ³΄λ©΄ `5736e178-a06e-422b-84be-2309a3741eff` μ κ°μ ννλ‘ κ°μ΄ μμ±λλ€.

## **πΆ μΏ ν€λ₯Ό νμ©ν΄ μμ΄λ μ λ¬**

ν΄λΌμ΄μΈνΈκ° μ²μ μ κ·Όνλ κ²½μ°, ν΄λΉ ν΄λΌμ΄μΈνΈκ° μ¬μ©ν  **session μμ΄λλ₯Ό μμ±**νκ³  μ΄λ₯Ό **μΏ ν€λ‘ μ λ¬**νλ€. μ΄ν μμ²­λΆν°λ μν κ° κ³΅μ λ₯Ό μν΄ μ λ¬ν΄μ€ session μμ΄λλ₯Ό μ¬μ©νλ€.

`RequestHandler` ν΄λμ€μ session μμ΄λκ° μ‘΄μ¬νλμ§ μ¬λΆλ₯Ό νλ¨ν ν μλ κ²½μ° session μμ΄λλ₯Ό μλ‘ λ°κΈνλ€. session μμ΄λλ `JSESSIONID` λ‘ μ λ¬νλ€.

```java
public class RequestHandler extends Thread {
    ...

    public void run() {
        log.debug("New Client Connect! Connected IP : {}, Port : {}", connection.getInetAddress(),
                connection.getPort());

        try (InputStream in = connection.getInputStream(); OutputStream out = connection.getOutputStream()) {
            HttpRequest request = new HttpRequest(in);
            HttpResponse response = new HttpResponse(out);

            if (getSessionId(request.getHeader("Cookie")) == null) {
                response.addHeader("Set-Cookie", "JSESSIONID=" + UUID.randomUUID());
            }
            
            ...

        } catch (IOException e) {
            log.error(e.getMessage());
        }
    }

    private String getSessionId(String cookieValue) {
        Map<String, String> cookies = HttpRequestUtils.parseCookies(cookieValue);
        return cookies.get("JSESSIONID");
    }
    ...

}
```

- `getSessionId` λ©μλλ request ν€λμ `Cookie` κ°μ νμ±νμ¬ `JSESSIONID` κ°μ λ°ννλ€.
    - μμΌλ©΄, `JSESSIONID=5736e178-a06e-422b-84be-2309a3741eff` κ°μ νν
    - session μμ΄λκ° μλ κ²½μ°, μλ‘ λ°κΈνλ€.

`ListUserController` μμ λ‘κ·ΈμΈ μ¬λΆ νμΈμ μν΄ `Cookie` ν€λ κ°μ νμ©νλ λΆλΆμ΄ μ¦κ°νλ―λ‘ μ΄λ₯Ό κ΄λ¦¬νλ `HttpCookie` ν΄λμ€λ₯Ό μΆκ°νλ λ¦¬ν©ν λ§μ μ€μνλ€.

```java
public class HttpCookie {
    private Map<String, String> cookies;

    HttpCookie(String cookieValue) {
        cookies = HttpRequestUtils.parseCookies(cookieValue);
    }

    public String getCookie(String name) {
        return cookies.get(name);
    }
}
```

```java
public class HttpRequest {
    ...

    public HttpCookie getCookies() {
			return new HttpCookie(getHeader("Cookie"));
		}
}
```

- μ΄ν `HttpRequest` μμ `HttpCookie` μ μ κ·Όν  μ μλ λ©μλλ₯Ό μΆκ°νλ€.

μ΄μ  `RequestHandler` ν΄λμ€λ μλ‘ μΆκ°ν `HttpCookie` ν΄λμ€μ λ©μλλ₯Ό μ¬μ©νλλ‘ λ³κ²½ν  μ μλ€.

```java
if (request.getCookies().getCookie("JSESSIONID") == null) {
    response.addHeader("Set-Cookie", "JSESSIONID=" + UUID.randomUUID());
}
```

## **π¨βπ¦± λͺ¨λ  ν΄λΌμ΄μΈνΈμ μΈμ λ°μ΄ν°μ λν μ μ₯μ μΆκ°**

μλ²λ **λ€μμ ν΄λΌμ΄μΈνΈ sessionμ μ§μ**ν΄μΌ νλ€. μ΄λ₯Ό μν΄μλ sessionμ κ΄λ¦¬ν  μ μλ **μ μ₯μ**κ° νμνλ€.

- λͺ¨λ  sessionμ λ§€λ² μμ±νλ κ²μ΄ μλλΌ **ν λ² μμ± ν μ¬μ¬μ©**ν΄μΌ νλ€. (`static` μΌλ‘ `Map` μμ±ν΄ (id, session)μ μμΌλ‘ μ μ₯)

```java
public class HttpSessions {
    private static Map<String, HttpSession> sessions = new HashMap<>();
    
    public static HttpSession getSession(String id) {
        HttpSession session = sessions.get(id);
        
        if (session == null) {
            session = new HttpSession(id);
            sessions.put(id, session);
            return session;
        }
        
        return session;
    }
    
    static void remove (String id) {
        sessions.remove(id);
    }
}
```

## **π΅ ν΄λΌμ΄μΈνΈλ³ μΈμ μ μ₯μ μΆκ°**

μλΈλ¦Ώμμ session λ°μ΄ν°μ μ κ·Όν  λ μ¬μ©ν ν΄λμ€μΈ, κ° ν΄λΌμ΄μΈνΈλ³ sessionμ λ΄λΉν  `HttpSession` ν΄λμ€λ₯Ό μΆκ°νλ€.

```java
public class HttpSession {
    private Map<String, Object> values = new HashMap<>();
    
    private String id;
    
    public HttpSession(String id) {
        this.id = id;
    }
    
    public String getId() {
        return id;
    }
    
    public void setAttribute(String name, Object value) {
        values.put(name, value);
    }
    
    public Object getAttribute(String name) {
        return values.get(name);
    }
    
    public void removeAttribute(String name) {
        values.remove(name);
    }
    
    public void invalidate() {
        HttpSessions.remove(id);
    }
}
```

- `HttpRequest` μμ ν΄λΌμ΄μΈνΈμ ν΄λΉνλ `HttpSession` μ μ κ·Όν  μ μλλ‘ λ©μλλ₯Ό μΆκ°νλ€.

```java
public HttpSession getSession() {
    return HttpSessions.getSession(getCookies().getCookie("JSESSIONID"));
}
```

session μΆκ°λ₯Ό μλ£νμΌλ―λ‘, `logined=true` μ κ°μ μΏ ν€ κ°μ μΆκ°νλ κ²μ΄ μλ `User` κ°μ²΄λ₯Ό μΆκ°ν΄ λ‘κ·ΈμΈ μ¬λΆλ₯Ό νλ¨νλλ‘ μ½λλ₯Ό λ³κ²½νμ.

```java
public class LoginController extends AbstractController {
    @Override
    public void doPost(HttpRequest request, HttpResponse response) {
        User user = DataBase.findUserById(request.getParameter("userId"));
        if (user != null) {
            if (user.login(request.getParameter("password"))) {
                HttpSession session = request.getSession();
                session.setAttribute("user", user);
                response.sendRedirect("/index.html");
            } else {
                response.sendRedirect("/user/login_failed.html");
            }
        } else {
            response.sendRedirect("/user/login_failed.html");
        }
    }
}
```

sessionμ `User` κ°μ²΄λ₯Ό μΆκ°νμΌλ―λ‘, `ListUserController` μμ λ‘κ·ΈμΈ μ¬λΆ νλ¨ μ½λλ‘ λ³κ²½ν΄μ€μΌ νλ€.

```java
public class ListUserController extends AbstractController {
    @Override
    public void doGet(HttpRequest request, HttpResponse response) {
        if (!isLogined(request.getSession())) {
            response.sendRedirect("/user/login.html");
            return;
        }
        ...

    }

    private static boolean isLogined(HttpSession session) {
        Object user = session.getAttribute("user");
        if (user == null) {
            return false;
        }
        return true;
    }
}
```

sessionμ νμ©νλ©΄, ν΄λΌμ΄μΈνΈμ μλ² κ° μν κ³΅μ λ₯Ό μν΄ μ λ¬νλ λ°μ΄ν°λ session μμ΄λ λΏμ΄λ€.

- μμΈ‘ν  μ μλλ‘ μμ±νλ κ²μ λ³΄μμΈ‘λ©΄μμ μ€μνλ€.

μΏ ν€λ λ³΄μ κ°νλ₯Ό μν΄ 

- `domain` : μΏ ν€μ μ κ·Ό κ°λ₯ν λλ©μΈμ μ§μ νλ μμ±
- `path` : μΉμλ²μ νΉμ  URLμ μ§μ νλ μμ±
- `max-age` : μΏ ν€λ₯Ό μΌλ§λ μ μ§ν  κ²μΈμ§λ₯Ό μ§μ νλ μμ± (μ΄ λ¨μ)
- `expires` : max-ageμ λμΌν μ­ν , μ ν¨ν λ μ§λ₯Ό μ§μ νλ μμ±
- `secure` : httpsν΅μ μμλ§ μΏ ν€μ μ κ·Όνλλ‘ μ§μ νλ μμ±

μμ±μ μ¬μ©ν  μ μλ€.

> λ¨μν sessionλ§ μ¬μ©νλ€κ³  ν΄μ λ³΄μ λ¬Έμ κ° ν΄κ²°λλ κ²μ μλλ€.

---

# **π¨ββοΈ MVC νλ μμν¬ μκ΅¬μ¬ν­ 1λ¨κ³**

MVC ν¨ν΄μ΄ μ¬μ©λκΈ° μ , λλΆλΆμ μΉ μ νλ¦¬μΌμ΄μ κ°λ°μ JSPμ λλΆλΆμ λ‘μ§μ ν¬ν¨νκ³  μμλ€. νμ§λ§ μ΄λ μ΄κΈ° κ°λ° μλλ λΉ λ₯΄μ§λ§ **μ μ§λ³΄μ λΉμ©μ΄ μ¦κ°**νλ λ¬Έμ λ₯Ό κ°μ§κ³  μμλ€. λ°λΌμ μ μ§λ³΄μ λΉμ©μ μ€μ΄κΈ° μν΄ **MVC(Model, View, Controller) ν¨ν΄ κΈ°λ°**μΌλ‘ μΉ μ νλ¦¬μΌμ΄μμ κ°λ°νλ λ°©ν₯μΌλ‘ λ°μ νλ€.

- JSPμ μ§μ€λμλ λ‘μ§μ `Model`, `View`, `Controller` 3κ°μ μ­ν λ‘ λΆλ¦¬ κ°λ° (like μΌκΆλΆλ¦½)

**MVC ν¨ν΄ κΈ°λ° κ°λ° μ μμ²­κ³Ό μλ΅ νλ¦**

<img src="https://user-images.githubusercontent.com/33208303/158940954-8b29b986-3019-4ec8-bcb9-37574c618079.png" width="80%">

**MVC ν¨ν΄ κΈ°λ° κ°λ° μ μμ²­κ³Ό μλ΅ νλ¦ + μμ€μ½λ**

<img src="https://user-images.githubusercontent.com/33208303/158940959-c4ea361a-fe76-40d3-b4b9-c95d9df8bbe7.png" width="80%">

MVC ν¨ν΄μ μ μ©νλ κ²½μ° κΈ°μ‘΄κ³Ό λ€λ₯Έ μ μ 

**"ν΄λΌμ΄μΈνΈ μμ²­μ΄ μ²μ μ§μνλ λΆλΆμ΄ μ»¨νΈλ‘€λ¬"**λΌλ κ²μ΄λ€.

- κΈ°μ‘΄μλ λ·°μ ν΄λΉνλ JSPκ° μμ²­μ μ²« μ§μλΆμλ€.
- MVC ν¨ν΄μ μ μ©νλ©΄ **λλΆλΆμ λ‘μ§μ μ»¨νΈλ‘€λ¬μ λͺ¨λΈμ΄ λ΄λΉ**νκ³ , λ·°μ ν΄λΉνλ **JSP**λ **μ»¨νΈλ‘€λ¬μμ μ λ¬ν λ°μ΄ν°λ₯Ό μΆλ ₯νλ λ‘μ§λ§**μ ν¬ν¨νλ€.
    - λ°λΌμ JSPμ λ³΅μ‘λλ λ?μμ§λ€.

> νλ μμν¬λ κ°λ°μ κ°μ μ­λ μ°¨μ΄κ° μλλΌλ MVC ν¨ν΄μ μ μ©ν΄ **μΌκ΄μ± μλ μ½λ**λ₯Ό κ΅¬ννλλ‘ κ°μ νλ€.
> 

**νλ μμν¬μ λΌμ΄λΈλ¬λ¦¬**

λͺ¨λ μ νλ¦¬μΌμ΄μ κ°λ°μμ λ°μνλ μ€λ³΅ μ½λλ₯Ό μ κ±°ν΄ **μ¬μ¬μ©μ±μ λμ**μΌλ‘μ¨ **κ°λ° μλλ₯Ό λΉ λ₯΄κ² νκΈ° μν λͺ©μ **μ κ°μ§μ§λ§, κ°μ₯ ν° μ°¨μ΄λ‘λ

- **νλ μμν¬**λ νΉμ  ν¨ν΄ κΈ°λ°μΌλ‘ κ°λ°νλλ‘ κ°μ νλ μ­ν μ΄κ³ ,
- **λΌμ΄λΈλ¬λ¦¬**λ κ°μ νλ λΆλΆμ΄ μλ€.

## **π©βπΌ μκ΅¬μ¬ν­**

: "MVC ν¨ν΄μ μ§μνλ νλ μμν¬λ₯Ό κ΅¬ννλ κ²β

MVC ν¨ν΄μ μ§μνλ κΈ°λ³Έμ μΈ κ΅¬μ‘°λ **λͺ¨λ  μμ²­μ νλμ μλΈλ¦Ώμ΄ λ°μ ν μμ²­ URLμ λ°λΌ λΆκΈ° μ²λ¦¬νλ λ°©μ**μΌλ‘ κ΅¬ννλ©΄ λλ€.

MVC ν¨ν΄μ μ¬μ©μμ μ΅μ΄ μ§μ μ§μ μ΄ **μ»¨νΈλ‘€λ¬**μ΄λ€. λ°λΌμ JSP(λ·°) μ§μ  μ κ·Όνμ§ μλλ‘ ν΄μΌ νλ€. μλλ MVC νλ μμν¬λ₯Ό κ΅¬ννμ λμ ν΄λμ€ λ€μ΄μ΄κ·Έλ¨μ΄λ€.

<img src="https://user-images.githubusercontent.com/33208303/158940961-e96c2bd1-e0f1-4297-b01a-20f9a10e4770.png" width="80%">

- λͺ¨λ  μμ²­μ `DispatcherServlet` μ΄ λ°μ ν μμ²­ URLμ λ°λΌ ν΄λΉ μ»¨νΈλ‘€λ¬μ μμμ μμνλ€.
    - `@WebServlet` μΌλ‘ URL λ§€ν μ, `urlPatterns="/"` μ κ°μ΄ μ€μ νμ¬ λͺ¨λ  μμ²­ URLμ΄ `DispatcherServlet` μΌλ‘ μ°κ²°λλλ‘ νλ€.
- CSS, JS, Imageμ κ°μ μ μ  μμμ μ»¨νΈλ‘€λ¬κ° νμμλ€. νμ§λ§ μμ κ°μ λ§€ν λ°©μμ μ μ  μμμ λν μμ²­κΉμ§ `DispatcherServlet` μΌλ‘ λ§€νλμ΄λ²λ¦°λ€.
    - μ΄λ₯Ό μν΄ μ μ  μμμ μ²λ¦¬νλ μλΈλ¦Ώ νν°λ₯Ό μΆκ°νλ€. (`core.web.filter.ResourceFilter`)

> **μλΈλ¦Ώ νν°(Servlet Filter)**
μλΈλ¦Ώ μ€ν μ  λλ νμ νΉμ  μμμ νκ³ μ ν  λ μ¬μ©νλ€. μλ‘λ μΈμ¦ νν°, λ‘κΉ λ° κ°μ νν°, μνΈν νν° λ±μ΄ μλ€.

---

# **πΌ MVC νλ μμν¬ κ΅¬ν 1λ¨κ³**

ν΄λΌμ΄μΈνΈ μμ²­μ λν μ²λ¦¬λ₯Ό λ΄λΉνλ λΆλΆμ **`Controller` λΌλ μΈν°νμ΄μ€λ‘ μΆμν**ν  μ μλ€.

```java
public interface Controller {
    String execute(HttpServletRequest req, HttpServletResponse resp)
        throws Exception;
}
```

μ§κΈκΉμ§ `HttpServlet` μ μμν΄ κ΅¬νν μλΈλ¦Ώ μ½λλ€μ `Controller` μΈν°νμ΄μ€λ₯Ό μμν΄ κ΅¬ννλλ‘ λ³κ²½νλ€.

```java
@WebServlet("/users")
public class ListUserController implements Controller {

    @Override
    public String execute(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        if (!UserSessionUtils.isLogined(req.getSession())) {
            return "redirect:/users/loginForm";
        }

        req.setAttribute("users", DataBase.findAll());
        return "/user/list.jsp";
    }
}
```

- μλΈλ¦Ώμμ JSPλ‘ μ΄λν  λ κ΅¬νν΄μΌ νλ μ€λ³΅ μ½λκ° μ κ±°λμλ€.

νΉλ³ν λ‘μ§ κ΅¬νμ΄ νμ μμ΄ λ·°μ λν μ΄λλ§μ λ΄λΉνλ κ²½μ° μ»¨νΈλ‘€λ¬ μμ±μ΄ λΆν©λ¦¬νλ€κ³  νλ¨ν΄, λ·°μ λν μ΄λλ§μ λ΄λΉνλ μ»¨νΈλ‘€λ¬λ₯Ό μμ±νλ€. (`ForwardController`)

```java
public class ForwardController implements Controller {
    private String forwardUrl;

    public ForwardController(String forwardUrl) {
        this.forwardUrl = forwardUrl;
        if (forwardUrl == null) {
            throw new NullPointerException("forwardUrl is null. μ΄λν  URLμ μλ ₯νμΈμ.");
        }
    }

    @Override
    public String execute(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        return forwardUrl;
    }
}
```

λͺ¨λ  μλΈλ¦Ώλ€μ΄ `Controller` μΈν°νμ΄μ€λ₯Ό μμνλλ‘ κ΅¬νν ν, μμ²­ URLκ³Ό μ»¨νΈλ‘€λ¬ λ§€νμ λ΄λΉνλ `RequestMapping` μ μμ±νλ€.

```java
public class RequestMapping {
    private static final Logger logger = LoggerFactory.getLogger(DispatcherServlet.class);
    private Map<String, Controller> mappings = new HashMap<>();

    void initMapping() {
        mappings.put("/", new HomeController());
        mappings.put("/users/form", new ForwardController("/user/form.jsp"));
        mappings.put("/users/loginForm", new ForwardController("/user/login.jsp"));
        mappings.put("/users", new ListUserController());
        mappings.put("/users/login", new LoginController());
        mappings.put("/users/profile", new ProfileController());
        mappings.put("/users/logout", new LogoutController());
        mappings.put("/users/create", new CreateUserController());
        mappings.put("/users/updateForm", new UpdateFormUserController());
        mappings.put("/users/update", new UpdateUserController());

        logger.info("Initialized Request Mapping!");
    }

    public Controller findController(String url) {
        return mappings.get(url);
    }

    void put(String url, Controller controller) {
        mappings.put(url, controller);
    }
}
```

- λ°μνλ λͺ¨λ  μμ²­ URLκ³Ό μλΉμ€λ₯Ό λ΄λΉν  μ»¨νΈλ‘€λ¬λ₯Ό μ°κ²°νμ¬ `Map` μΌλ‘ μ μ₯νλ€.

λ§μ§λ§ μμμ ν΄λΌμ΄μΈνΈμ λͺ¨λ  μμ²­μ λ°μ URLμ ν΄λΉνλ μ»¨νΈλ‘€λ¬λ‘ μμμ μμνκ³ , μ€νλ κ²°κ³Ό νμ΄μ§λ‘ μ΄λνλ μμμ΄λ€. μ΄λ `DispatcherServlet` μμ κ΅¬ννλ€. (ν΄λμ€ λ€μ΄μ΄κ·Έλ¨μμ ν΄λΌμ΄μΈνΈμ μμ²­μ λ°λ μ§μ )

```java
@WebServlet(name = "dispatcher", urlPatterns = "/", loadOnStartup = 1)
public class DispatcherServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    private static final Logger logger = LoggerFactory.getLogger(DispatcherServlet.class);
    private static final String DEFAULT_REDIRECT_PREFIX = "redirect:";

    private RequestMapping rm;

    @Override
    public void init() throws ServletException {
        rm = new RequestMapping();
        rm.initMapping();
    }

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String requestUri = req.getRequestURI();
        logger.debug("Method : {}, Request URI : {}", req.getMethod(), requestUri);

        Controller controller = rm.findController(requestUri);
        try {
            String viewName = controller.execute(req, resp);
            move(viewName, req, resp);
        } catch (Throwable e) {
            logger.error("Exception : {}", e);
            throw new ServletException(e.getMessage());
        }
    }

    private void move(String viewName, HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        if (viewName.startsWith(DEFAULT_REDIRECT_PREFIX)) {
            resp.sendRedirect(viewName.substring(DEFAULT_REDIRECT_PREFIX.length()));
            return;
        }

        RequestDispatcher rd = req.getRequestDispatcher(viewName);
        rd.forward(req, resp);
    }
}
```

- `urlPatterns = "/"` λ‘ λ§€ννμ¬ λͺ¨λ  μμ²­ URLμ΄ `DispatcherServlet` μΌλ‘ λ§€νλλλ‘ νλ€.
    - μΌλ°μ μΌλ‘ λͺ¨λ  μμ²­ URLμ λ§€ννλ€κ³  νλ©΄ `"/*"` μΌλ‘ μκ°ν  μλ μλ€. νλ¦¬μ§ μμμ§λ§, μ΄λ κ² νκ² λλ©΄ λͺ¨λ  JSPμ λν μμ²­ λν λ§€νλμ΄ JSPμ λν μμ²­μ΄ μ μμ μΌλ‘ μ²λ¦¬λμ§ μλλ€.
- `"/"` λ§€νμ λ§€νλμ΄ μλ μλΈλ¦Ώ, JSP μμ²­μ΄ μλ JS, CSS, Imagesμ κ°μ μμ²­μ μ²λ¦¬νλλ‘ μ€κ³λμλ€. ν°μΊ£ μλ² κΈ°λ³Έ μ€μ μμ, `"/"` μ€μ μ `"default"` λΌλ μ΄λ¦μ κ°μ§λ μλΈλ¦Ώμ λ§€νν΄ μ μ  μμμ μ²λ¦¬νλλ‘ κ΅¬νλμ΄ μλ€.
    - ν΄λΉ μ€μ μ `DispatcherServlet` μμ μ¬μ μνμ¬ JSPμ λν μμ²­μ μ μΈν λͺ¨λ  μμ²­μ λ΄λΉνλλ‘ κ΅¬ννλ€.
- `"default"` μλΈλ¦Ώμμ μ²λ¦¬νλ μ μ  μμμ λν μ²λ¦¬λ `DispatcherServlet` μΌλ‘ μμ²­λκΈ° μ μ `ResourceFilter` μμ μ²λ¦¬νλλ‘ κ΅¬ννλ€.

**μ£Όμν  μ **

- `HomeController` μμ `"/"` λ‘ λ§€νν ν `localhost:8080` μμ²­μ κ²½μ°, μΉ μμμ κ΄λ¦¬νλ `webapp` λλ ν λ¦¬μ `index.jsp` κ° μ‘΄μ¬νλ©΄ `HomeController` κ° μλ `index.jsp` λ‘ μμ²­μ΄ μ²λ¦¬λλ€.
    - μ΄λ `path` κ° μλ κ²½μ° μ²λ¦¬λ₯Ό λ΄λΉνλ κΈ°λ³Έ νμΌμ΄ `index.jsp` λ‘ μ€μ λμ΄ μκΈ° λλ¬Έμ΄λ€.
    - μ΄λ¬ν μ΄μ λ‘ `HomeController` μμ μ΄λν  λ·°μ μ΄λ¦μ΄ `home.jsp` μ΄λ€.

**`loadOnStartup` μμ±**

- μλΈλ¦Ώ μΈμ€ν΄μ€λ₯Ό μμ±νλ μμ κ³Ό μ΄κΈ°νλ₯Ό λ΄λΉνλ `init()` λ©μλλ₯Ό μ΄λ μμ μ νΈμΆν  κ²μΈκ°λ₯Ό κ²°μ νλ μ€μ 
    - **μ΄λ₯Ό νμ§ μμ κ²½μ°** μλΈλ¦Ώ μΈμ€ν΄μ€ μμ±κ³Ό μ΄κΈ°νλ μλΈλ¦Ώ μ»¨νμ΄λκ° μμμ μλ£ν ν **ν΄λΌμ΄μΈνΈμ μμ²­μ΄ μ΅μ΄λ‘ λ°μνλ μμ μ μ§ν**λλ€.
    - **μ€μ ν κ²½μ° μλΈλ¦Ώ μ»¨νμ΄λκ° μμνλ μμ μ μλΈλ¦Ώ μΈμ€ν΄μ€ μμ±κ³Ό μ΄κΈ°νκ° μ§ν**λλ€.
- μ€μ μ μ«μ κ°μΌλ‘ μλΈλ¦Ώμ μ΄κΈ°ν μμλ₯Ό κ²°μ νλ€. (λ?μ μμΌλ‘ λ¨Όμ  μ§ν)

**`move()` λ©μλ**

- κ° μλΈλ¦Ώμμ μλΈλ¦Ώκ³Ό JSP μ¬μ΄λ₯Ό μ΄λνκΈ° μν΄ κ΅¬νν λͺ¨λ  μ€λ³΅ μ½λλ₯Ό λ΄λΉνλ€.

**νλ‘ νΈ μ»¨νΈλ‘€λ¬(front controller) ν¨ν΄**

- κ° μ»¨νΈλ‘€λ¬μ μμ λͺ¨λ  μμ²­μ λ°μ κ° μ»¨νΈλ‘€λ¬μ μμμ μμνλ λ°©μμΌλ‘ κ΅¬ννλ κ²

**MVC νλ μμν¬ κΈ°λ°μΌλ‘ μμ²­μμ μλ΅κΉμ§μ νλ¦**

- ν΄λΌμ΄μΈνΈμ μμ²­ λ°μ
- `ServletFilter`(`ResourceFilter`) μμμ μ μ²λ¦¬
- `DispatcherServlet` μΌλ‘ μμ²­ μ λ¬
- `RequestMapping` μ urlμ μ λ¬νκ³  ν΄λΉ μ²λ¦¬λ₯Ό λ§‘μ `Controller` λ₯Ό λ°ν
- `Controller` κ° μμμ μ²λ¦¬
- μλ΅μ λ³΄λ΄κΈ° μ  `ServletFilter` μμμ νμ²λ¦¬
- ν΄λΌμ΄μΈνΈλ‘ μλ΅μ μ μ‘

---

# **π¨ μ μ€ν¬λ¦½νΈλ₯Ό νμ©ν λ°°ν¬ μλν**

μκ²© μλ²μ ν°μΊ£ μλ²λ₯Ό μ€μΉνκ³ , μ§κΈκΉμ§ κ΅¬νν μμ€μ½λλ₯Ό λ°°ν¬νλ€. μλμΌλ‘ λ°°ν¬νλ μμμ μ μ€ν¬λ¦½νΈλ₯Ό νμ©ν΄ μλννλ κ³Όμ κΉμ§ μ§ννλ€.

## **π΄ μκ΅¬μ¬ν­**

- κ΅¬νν κΈ°λ₯μ κ°λ° μλ²μ ν°μΊ£ μλ²λ₯Ό μ€μΉν ν λ°°ν¬νλ€.
- ν°μΊ£ λ‘κ·Έ νμΌμ ν΅ν΄ μλ²μ μ μμ μΈ μ€νμ νμΈνλ€.
- μ μ€ν¬λ¦½νΈλ₯Ό λ§λ€μ΄ λ°°ν¬λ₯Ό μλννλ€.

`TOMCAT_HOME/webapps/ROOT` μ λ°°ν¬ν  μλΉμ€λ₯Ό μ»΄νμΌνκ³  ν¨ν€μ§(mvn clean package)νμ¬ λ§λ  `target` λλ ν λ¦¬μ νμ λλ ν λ¦¬λ₯Ό μ΄λμμΌ μλ²λ₯Ό μμνλ€

- μ΄λ, `pom.xml` μμ μ¬μ©νλ ν¨ν€μ§ λ°©μμ λ°λΌ ν¨ν€μ§μ μννλ€.
    - JAR : λμμ ν°μΊ£μ΄ νμμλ applicationμΌλ‘ `bin` λ΄μ `.class` νμΌλ€μ λ¬Άλ κ²
    - WAR : ν°μΊ£ μμμ λμνλ applicationμΌλ‘ `webapp` λ΄μ νμΌλ€μ λ¬Άλ κ²

# **π μΆμ²**

- **μλ° μΉ νλ‘κ·Έλλ° Next Step : νλμ© λ²κ²¨κ°λ μνκ»μ§ νμ΅λ²** - λ°μ¬μ±
- [https://dololak.tistory.com/543](https://dololak.tistory.com/543)
- [μΏ ν€μ document.cookie](https://ko.javascript.info/cookie)
- [μΏ ν€ μμ±λ€](https://velog.io/@y1andyu/%EC%BF%A0%ED%82%A4-%EC%86%8D%EC%84%B1%EB%93%A4)
- [Maven project μ War, JarνμΌ μ°¨μ΄](https://hoon93.tistory.com/15)
- [DAO, DTO μ°¨μ΄](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=godwings&logNo=221048712980)
- [[Servlet] μλΈλ¦Ώ νν° (Filter)](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=adamdoha&logNo=221665607853)