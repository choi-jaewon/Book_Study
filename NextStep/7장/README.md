# πνμ λ°μ΄ν°λ₯Ό DBμ μ μ₯νκΈ° μ€μ΅

## 1. μ€μ΅ μ½λ λ¦¬λ·° λ° JDBC λ³΅μ΅

### νμ΄λΈ μμ± jwp.sql νμΌ

```sql
DROP TABLE IF EXISTS USERS;

CREATE TABLE USERS ( 
	userId          varchar(12)		NOT NULL, 
	password		varchar(12)		NOT NULL,
	name			varchar(20)		NOT NULL,
	email			varchar(50),	
  	
	PRIMARY KEY               (userId)
);

INSERT INTO USERS VALUES('admin', 'password', 'μλ°μ§κΈ°', 'admin@slipp.net');
```

- νμ μ λ³΄μ λν νμ΄λΈ μμ± μ€ν¬λ¦½νΈ
- μλ²κ° μμνλ μμ μ μ΄ νμΌμ μ΄μ©ν΄ νμ΄λΈ μ΄κΈ°ν
- `src/main/resources` λλ ν λ¦¬ μλ `jwp.sql` νμΌλ‘ κ΅¬νλμ΄ μμ

### μ΄κΈ°νλ₯Ό μν ContextLoaderListener ν΄λμ€

```java
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.core.io.ClassPathResource;
import org.springframework.jdbc.datasource.init.DatabasePopulatorUtils;
import org.springframework.jdbc.datasource.init.ResourceDatabasePopulator;

import core.jdbc.ConnectionManager;

@WebListener
public class ContextLoaderListener implements ServletContextListener {
    private static final Logger logger = LoggerFactory.getLogger(ContextLoaderListener.class);

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        ResourceDatabasePopulator populator = new ResourceDatabasePopulator();
        populator.addScript(new ClassPathResource("jwp.sql"));
        DatabasePopulatorUtils.execute(populator, ConnectionManager.getDataSource());

        logger.info("Completed Load ServletContext!");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
    }
}
```

ν°μΊ£ μλ²κ° μμν  λ, μ΄κΈ°νν  μ μλλ‘ `ContextLoaderListener` ν΄λμ€μ κ΅¬νλμ΄ μλ€.

- `ContextLoaderListener`κ° `ServletContextListener` μΈν°νμ΄μ€λ₯Ό κ΅¬ννκ³  μμΌλ©°, `@WebListener` μ λΈνμ΄μμ΄ μ€μ λμ΄ μκΈ° λλ¬Έμ μλΈλ¦Ώ μ»¨νμ΄λλ₯Ό μμνλ κ³Όμ μμ `contextInitialized()` λ©μλλ₯Ό νΈμΆν΄ μ΄κΈ°ν μμμ μ§ννλ€.
- `ServletContextListener`μ λν μ΄κΈ°νλ μλΈλ¦Ώ μ΄κΈ°νλ³΄λ€ λ¨Όμ  μ§νλκΈ° λλ¬Έμ, μΉ μ νλ¦¬μΌμ΄μ μ μ²΄μ μν₯μ λ―ΈμΉλ μ΄κΈ°νκ° νμν κ²½μ°μ νμ©ν  μ μλ€.

### DAO(Data Access Object)

λ°μ΄ν°λ² μ΄μ€μ λν μ κ·Ό λ‘μ§ μ²λ¦¬λ₯Ό λ΄λΉνλ κ°μ²΄λ₯Ό λ³λλ‘ λΆλ¦¬ν΄ κ΅¬ννλ ν¨ν΄μΌλ‘, νμ¬ μΌλ°μ μΈ ν¨ν΄μΌλ‘ λλ¦¬ νμ©λκ³  μλ€.

```java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import core.jdbc.ConnectionManager;
import next.model.User;

public class UserDao {
    public void insert(User user) throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            con = ConnectionManager.getConnection();
            String sql = "INSERT INTO USERS VALUES (?, ?, ?, ?)";
            pstmt = con.prepareStatement(sql);
            pstmt.setString(1, user.getUserId());
            pstmt.setString(2, user.getPassword());
            pstmt.setString(3, user.getName());
            pstmt.setString(4, user.getEmail());

            pstmt.executeUpdate();
        } finally {
            if (pstmt != null) {
                pstmt.close();
            }

            if (con != null) {
                con.close();
            }
        }
    }

    public void update(User user) throws SQLException {
        // TODO κ΅¬ν νμν¨.
    }

    public List<User> findAll() throws SQLException {
        // TODO κ΅¬ν νμν¨.
        return new ArrayList<User>();
    }

    public User findByUserId(String userId) throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            con = ConnectionManager.getConnection();
            String sql = "SELECT userId, password, name, email FROM USERS WHERE userid=?";
            pstmt = con.prepareStatement(sql);
            pstmt.setString(1, userId);

            rs = pstmt.executeQuery();

            User user = null;
            if (rs.next()) {
                user = new User(rs.getString("userId"), rs.getString("password"), rs.getString("name"),
                        rs.getString("email"));
            }

            return user;
        } finally {
            if (rs != null) {
                rs.close();
            }
            if (pstmt != null) {
                pstmt.close();
            }
            if (con != null) {
                con.close();
            }
        }
    }
}
```

- μ¬μ©μ λ°μ΄ν°λ₯Ό μΆκ°νκ³ , μ¬μ©μ μμ΄λμ ν΄λΉνλ μ¬μ©μ λ°μ΄ν°λ₯Ό μ‘°ννλ κΈ°λ₯λ§ κ΅¬νν΄λμ `UserDao`
- μλ `CreateUserController` μ½λμμ λ³΄μ΄λ κ²μ²λΌ μ¬μ©ν  μ μλ€.

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import core.db.DataBase;
import core.mvc.Controller;
import next.model.User;

public class CreateUserController implements Controller {
    private static final Logger log = LoggerFactory.getLogger(CreateUserController.class);

    @Override
    public String execute(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        User user = new User(req.getParameter("userId"), req.getParameter("password"), req.getParameter("name"),
                req.getParameter("email"));
        log.debug("User : {}", user);

        DataBase.addUser(user);
        return "redirect:/";
    }
}
```

### νμ λͺ©λ‘ μ€μ΅

νμ μ μ²΄ λͺ©λ‘μ μ‘°ννλ `findAll()` λ©μλλ₯Ό κ΅¬νν΄μΌ ν¨.

```java
public List<User> findAll() throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            con = ConnectionManager.getConnection();
            String sql = "SELECT userId, password, name, email FROM USERS";
            pstmt = con.prepareStatement(sql);

            rs = pstmt.executeQuery();

            User user = null;
            ArrayList<User> users = new ArrayList<User>();
            if (rs.next()) {
                user = new User(rs.getString("userId"), rs.getString("password"), rs.getString("name"),
                        rs.getString("email"));
                users.add(user);
            }

            return users;
        } finally {
            if (rs != null) {
                rs.close();
            }
            if (pstmt != null) {
                pstmt.close();
            }
            if (con != null) {
                con.close();
            }
        }
    }
```

### κ°μΈμ λ³΄ μμ  μ€μ΅

UPDATE SQLλ¬Έμ νμ©ν΄ κ°μΈμ λ³΄ μμ  κΈ°λ₯ μΆκ°

```java
public void update(User user) throws SQLException {
        // TODO κ΅¬ν νμν¨.
        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            con = ConnectionManager.getConnection();
            String sql = "UPDATE USERS SET password = ?, name = ?, email = ? WHERE userId = ?";
            pstmt = con.prepareStatement(sql);
            pstmt.setString(1, user.getPassword());
            pstmt.setString(2, user.getName());
            pstmt.setString(3, user.getEmail());
            pstmt.setString(4, user.getUserId());

            pstmt.executeUpdate();
        } finally {
            if (pstmt != null) {
                pstmt.close();
            }

            if (con != null) {
                con.close();
            }
        }
    }
```

---

## 2. DAO λ¦¬ν©ν λ§ μ€μ΅

νμ¬ UserDao μ½λμλ λ§μ μ€λ³΅ μ½λκ° μ‘΄μ¬νκΈ° λλ¬Έμ λ¦¬ν©ν λ§μ΄ νμνλ€.

β λ³νκ° μλ λΆλΆ(κ³΅ν΅ λΌμ΄λΈλ¬λ¦¬ λΆλΆ), λ³νκ° λ°μνλ λΆλΆ(κ°λ°μκ° κ΅¬νν  λΆλΆ)μΌλ‘ λλ μΌ ν¨

β SQL μΏΌλ¦¬, μΏΌλ¦¬μ μ λ¬ν  μΈμ, SELECT κ΅¬λ¬Έμ κ²½μ° μ‘°νν λ°μ΄ν°λ₯Ό μΆμΆνλ 3κ°μ§λ§ κ΅¬ννλ©΄ λ¨

---

SQLExceptionμΌλ‘ μΈν΄ μκΈ°λ λΆνμνκ² λ§μ try/catch μ  λλ¬Έμ μμ€μ½λμ κ°λμ±μ΄ λ¨μ΄μ§κΈ° λλ¬Έμ μ΄ μ λ λ¦¬ν©ν λ§μ΄ νμνλ€.

> ***μ»΄νμΌνμ Exceptionκ³Ό λ°νμ Exceptionμ μ¬μ©ν΄μΌ λλ κ°μ΄λλΌμΈ***
- APIλ₯Ό μ¬μ©νλ λͺ¨λ  κ³³μμ μ΄ μμΈλ₯Ό μ²λ¦¬ν΄μΌ νλκ°? μμΈκ° λ°λμ λ©μλμ λν λ°ν κ°μ΄ λμ΄μΌ νλκ°? 
  β Yes? μ»΄νμΌνμ Exceptionμ μ¬μ©
- APIλ₯Ό μ¬μ©νλ μμ μ€ μ΄ μμΈλ₯Ό μ²λ¦¬ν΄μΌ νλκ°?
  β Yes? λ°νμ ExceptionμΌλ‘ κ΅¬ν 
  (APIλ₯Ό μ¬μ©νλ λͺ¨λ  μ½λκ° Exceptionμ catchνλλ‘ κ°μ νμ§ μλ κ²μ΄ μ’μ)
- λ¬΄μμΈκ° ν° λ¬Έμ κ° λ°μνλκ°? μ΄ λ¬Έμ λ₯Ό λ³΅κ΅¬ν  λ°©λ²μ΄ μλκ°?
  β Yes? λ°νμ ExceptionμΌλ‘ κ΅¬ν
  (Exceptionμ catchνλλΌλ μλ¬μ λν μ λ³΄λ₯Ό ν΅λ³΄ λ°λ κ² μΈμ ν  μ μλ κ²μ΄ μμ)
- μμ§λ λΆλͺννκ°?
  β Yes? λ°νμ ExceptionμΌλ‘ κ΅¬ν
  (Exceptionμ λν΄ λ¬Έμννκ³  APIλ₯Ό μ¬μ©νλ κ³³μμ Exceptionμ λν μ²λ¦¬λ₯Ό κ²°μ νλλ‘)
> 

### μκ΅¬μ¬ν­ 1

INSERT, UPDATE μΏΌλ¦¬ λ©μλ μ€λ³΅ μ κ±° μμ

```java
public void insert(User user) throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            con = ConnectionManager.getConnection();
            String sql = createQueryForInsert();
            pstmt = con.prepareStatement(sql);
            setValuesForInsert(user, pstmt);
            pstmt.executeUpdate();
        } finally {
            if (pstmt != null) {
                pstmt.close();
            }

            if (con != null) {
                con.close();
            }
        }
    }

    private void setValuesForInsert(User user, PreparedStatement pstmt) throws SQLException {
        pstmt.setString(1, user.getUserId());
        pstmt.setString(2, user.getPassword());
        pstmt.setString(3, user.getName());
        pstmt.setString(4, user.getEmail());
    }

    private String createQueryForInsert(){
        return "INSERT INTO USERS VALUES (?, ?, ?, ?)";
    }
```

- Extract Method λ¦¬ν©ν λ§μ ν΅ν΄ μ μ½λμ²λΌ λ³νκ° λ°μνλ λΆλΆκ³Ό λ³νκ° λ°μνμ§ μλ λΆλΆμ λΆλ¦¬ν  μ μλ€.
- μ μ½λλ insertμ λν Extract Method λ¦¬ν©ν λ§μ΄κ³ , updateμ λν λΆλΆλ κ°μ λ°©μμΌλ‘ μ§ν

### μκ΅¬μ¬ν­ 2

λΆλ¦¬ν λ©μλ μ€ λ³νκ° λ°μνμ§ μλ λΆλΆ(κ³΅ν΅ λΌμ΄λΈλ¬λ¦¬λ‘ κ΅¬νν  μ½λ)μ μλ‘μ΄ ν΄λμ€λ‘ μΆκ°νκ³  μ΄λ

```java
public class InsertJdbcTemplate {
    public void insert(User user, UserDao userDao) throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            con = ConnectionManager.getConnection();
            String sql = userDao.createQueryForInsert();
            pstmt = con.prepareStatement(sql);
            userDao.setValuesForInsert(user, pstmt);
            pstmt.executeUpdate();
        } finally {
            if (pstmt != null) {
                pstmt.close();
            }

            if (con != null) {
                con.close();
            }
        }
    }
}
```

- κ³΅ν΅ λΌμ΄λΈλ¬λ¦¬λ‘ κ΅¬ννλ λΆλΆμ λΆλ¦¬νκΈ° μν΄ μλ‘μ΄ InsertJdbcTemplate ν΄λμ€λ₯Ό μΆκ°νκ³ ,
- UserDaoμ insert() λ©μλλ₯Ό InsertJdbcTemplateμΌλ‘ μ΄λ

```java
public class UserDao {
    public void insert(User user) throws SQLException {
        InsertJdbcTemplate jdbcTemplate = new InsertJdbcTemplate();
        jdbcTemplate.insert(user, this);
    }
[...]
```

- UserDaoμ insert() λ©μλκ° InsertJdbcTemplateμ insert() λ©μλλ₯Ό νΈμΆνλλ‘ λ³κ²½νκ³ ,
- createQueryForInsert(), setValuesForInsert()λ InsertJdbcTemplateμ insert() λ©μλκ° μ κ·Ό κ°λ₯νλλ‘ privateμμ defaultλ‘ μ κ·Όμ μ΄μ λ³κ²½
- update() λ©μλλ κ°μ λ°©μμΌλ‘ λ¦¬ν©ν λ§

> βΒ μ default??? publicμΌλ‘ νλ©΄ μλλ??
β defaultλ νμ¬ ν¨ν€μ§μμλ§ μ κ·Ό κ°λ₯νμ§λ§, publicμ λͺ¨λ  ν¨ν€μ§μμ μ κ·Ό κ°λ₯νλ€.
    νμμλ κ³³μμ μ°μ΄μ§ μκ², μμ νκ² νκΈ° μν΄ default μ κ·Όμ μ΄μλ₯Ό μ¬μ©νλ€κ³  μκ°
> 

### μκ΅¬μ¬ν­ 3

InsertJdbcTemplateκ³Ό UpdateJdbcTemplateμ UserDaoμ λν μμ‘΄κ΄κ³λ₯Ό λλλ€.

```java
public abstract class InsertJdbcTemplate {
    public void insert(User user) throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            con = ConnectionManager.getConnection();
            String sql = createQueryForInsert();
            pstmt = con.prepareStatement(sql);
            setValuesForInsert(user, pstmt);
            pstmt.executeUpdate();
        } finally {
            if (pstmt != null) {
                pstmt.close();
            }

            if (con != null) {
                con.close();
            }
        }
    }
    
    abstract String createQueryForInsert();
    abstract void setValuesForInsert(User user, PreparedStatement pstmt) throws SQLException;
}
```

- InsertJdbcTemplateμ΄ UserDaoμ μμ‘΄κ΄κ³λ₯Ό κ°μ§μ§ μλλ‘ μΆμ ν΄λμ€λ‘ λ°κΏμ€λ€.
- createQueryForInsert(), setValuesForInsert() λ©μλλ μΆμ λ©μλλ‘ λ¦¬ν©ν λ§ν΄μ€λ€.

```java
public class UserDao {
    public void insert(User user) throws SQLException {
        InsertJdbcTemplate jdbcTemplate = new InsertJdbcTemplate() {
            void setValuesForInsert(User user, PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, user.getUserId());
                pstmt.setString(2, user.getPassword());
                pstmt.setString(3, user.getName());
                pstmt.setString(4, user.getEmail());
            }

            String createQueryForInsert(){
                return "INSERT INTO USERS VALUES (?, ?, ?, ?)";
            }
        };
        jdbcTemplate.insert(user);
    }
[...]
```

- InsertJdbcTemplateμ΄ μΆμ ν΄λμ€μ΄κΈ° λλ¬Έμ λ°λ‘ μμ±ν  μ μλ€.
    
    > **μΆμ ν΄λμ€μ μΈμ€ν΄μ€λ₯Ό μμ±νλ λ°©λ²**
    1. μμνλ μλ‘μ΄ ν΄λμ€λ₯Ό μΆκ°
    2. μ΄λ¦μ κ°μ§ μλ μ΅λͺ ν΄λμ€ μΆκ°
    > 
- μ΅λͺ ν΄λμ€λ₯Ό μ¬μ©ν΄ UserDaoμ insert() λ©μλ κ΅¬ν
- μ΄μ κ°μ΄ λ°λ³΅μ μΌλ‘ λ°μνλ μ€λ³΅ λ‘μ§μ μμ ν΄λμ€κ° κ΅¬ννκ³  λ³νκ° λ°μνλ λΆλΆλ§ μΆμ λ©μλλ‘ λ§λ€μ΄ κ΅¬ννλλ‘ νλ λμμΈ ν¨ν΄μ ννλ¦Ώ λ©μλ(Template Method) ν¨ν΄μ΄λΌκ³  νλ€.
- κ°μ λ°©λ²μΌλ‘ updateλ λ¦¬ν©ν λ§

### μκ΅¬μ¬ν­ 4

InsertJdbcTemplateκ³Ό UpdateJdbcTemplate μ€ νλλ§ μ¬μ©νλλ‘ λ¦¬ν©ν λ§

```java
public abstract class JdbcTemplate {
    public void update(User user) throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            con = ConnectionManager.getConnection();
            String sql = createQuery();
            pstmt = con.prepareStatement(sql);
            setValues(user, pstmt);
            pstmt.executeUpdate();
        } finally {
            if (pstmt != null) {
                pstmt.close();
            }

            if (con != null) {
                con.close();
            }
        }
    }
    abstract String createQuery();
    abstract void setValues(User user, PreparedStatement pstmt) throws SQLException;
}
```

- λ©μλ μ΄λ¦λ€μ createQuery(), setValues()λ‘ Rename λ¦¬ν©ν λ§ μ§ν
- λ κ°μ ν΄λμ€λ‘ λΆλ¦¬ν  νμκ° μμ΄μ‘κΈ° λλ¬Έμ ν΄λμ€ μ΄λ¦μ JdbcTemplateμΌλ‘ λ°κΎΈκ³ , λ©μλ μ΄λ¦μ update()λ₯Ό μ¬μ©νλλ‘ λ¦¬ν©ν λ§

```java
public class UserDao {
    public void insert(User user) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate() {
            void setValues(User user, PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, user.getUserId());
                pstmt.setString(2, user.getPassword());
                pstmt.setString(3, user.getName());
                pstmt.setString(4, user.getEmail());
            }

            String createQuery(){
                return "INSERT INTO USERS VALUES (?, ?, ?, ?)";
            }
        };
        jdbcTemplate.update(user);
    }

    public void update(User user) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate() {
            void setValues(User user, PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, user.getPassword());
                pstmt.setString(2, user.getName());
                pstmt.setString(3, user.getEmail());
                pstmt.setString(4, user.getUserId());
            }

            String createQuery(){
                return "UPDATE USERS SET password = ?, name = ?, email = ? WHERE userId = ?";
            }
        };
        jdbcTemplate.update(user);
    }
[...]
```

- UserDaoμ insert(), update() λ©μλκ° JdbcTemplateμ μ¬μ©νλλ‘ λ¦¬ν©ν λ§
- InsertJdbcTemplateμ μ¬μ©νλ κ³³μ΄ μκΈ° λλ¬Έμ ν΄λμ€ μ­μ 

### μκ΅¬μ¬ν­ 5

JdbcTemplateκ³Ό Userμ μμ‘΄κ΄κ³λ₯Ό λλλ€.

```java
public abstract class JdbcTemplate {
    public void update() throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            con = ConnectionManager.getConnection();
            String sql = createQuery();
            pstmt = con.prepareStatement(sql);
            setValues(pstmt);
            pstmt.executeUpdate();
        } finally {
            if (pstmt != null) {
                pstmt.close();
            }

            if (con != null) {
                con.close();
            }
        }
    }
    abstract String createQuery();
    abstract void setValues(PreparedStatement pstmt) throws SQLException;
}
```

```java
public class UserDao {
    public void insert(User user) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate() {
            void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, user.getUserId());
                pstmt.setString(2, user.getPassword());
                pstmt.setString(3, user.getName());
                pstmt.setString(4, user.getEmail());
            }

            String createQuery(){
                return "INSERT INTO USERS VALUES (?, ?, ?, ?)";
            }
        };
        jdbcTemplate.update();
    }

    public void update(User user) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate() {
            void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, user.getPassword());
                pstmt.setString(2, user.getName());
                pstmt.setString(3, user.getEmail());
                pstmt.setString(4, user.getUserId());
            }

            String createQuery(){
                return "UPDATE USERS SET password = ?, name = ?, email = ? WHERE userId = ?";
            }
        };
        jdbcTemplate.update();
    }
[...]
```

- userλ₯Ό μΈμλ‘ μ λ¬νμ§ μμλ λκΈ° λλ¬Έμ Userμ λν μμ‘΄ κ΄κ³λ₯Ό λͺ¨λ λμ΄μ€λ€.

```java
public abstract class JdbcTemplate {
    public void update() throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            con = ConnectionManager.getConnection();
            String sql = createQuery();
            pstmt = con.prepareStatement(sql);
            setValues(pstmt);
            pstmt.executeUpdate();
        } finally {
            if (pstmt != null) {
                pstmt.close();
            }

            if (con != null) {
                con.close();
            }
        }
    }
    public void update2(String sql) throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            con = ConnectionManager.getConnection();
            pstmt = con.prepareStatement(sql);
            setValues(pstmt);
            pstmt.executeUpdate();
        } finally {
            if (pstmt != null) {
                pstmt.close();
            }

            if (con != null) {
                con.close();
            }
        }
    }
    abstract String createQuery();
    abstract void setValues(PreparedStatement pstmt) throws SQLException;
}
```

```java
public class UserDao {
    public void insert(User user) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate() {
            void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, user.getUserId());
                pstmt.setString(2, user.getPassword());
                pstmt.setString(3, user.getName());
                pstmt.setString(4, user.getEmail());
            }

            String createQuery(){
                return "INSERT INTO USERS VALUES (?, ?, ?, ?)";
            }
        };
        String sql = "INSERT INTO USERS VALUES (?, ?, ?, ?)";
        jdbcTemplate.update2(sql);
    }

    public void update(User user) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate() {
            void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, user.getPassword());
                pstmt.setString(2, user.getName());
                pstmt.setString(3, user.getEmail());
                pstmt.setString(4, user.getUserId());
            }

            String createQuery(){
                return "UPDATE USERS SET password = ?, name = ?, email = ? WHERE userId = ?";
            }
        };
        String sql = "UPDATE USERS SET password = ?, name = ?, email = ? WHERE userId = ?";
        jdbcTemplate.update2(sql);
    }
[...]
```

- SQL μΏΌλ¦¬λ κ΅³μ΄ μΆμ λ©μλλ₯Ό ν΅ν΄ μ λ¬νμ§ μκ³ , JdbcTemplateμ update() λ©μλ μΈμλ‘ μ λ¬νλ κ² μ¬μ©μ± μΈ‘λ©΄μμ λ μ’μ κ²μ΄λ€.
- μ»΄νμΌ μλ¬κ° λ°μνμ§ μλλ‘ μ²μ²ν λ¦¬ν©ν λ§μ νκΈ° μν΄ κΈ°μ‘΄ update() λ©μλλ μ μ§ν μνλ‘ μλ‘μ΄ update2(String sql)μ μΆκ°νλ€.
- μ€λ₯κ° μλ κ²μ νμΈνλ©΄ νμμλ update(), createQuery() λ©μλλ₯Ό μ­μ νκ³ , update2() λ©μλ μ΄λ¦μ update()λ‘ Rename λ¦¬ν©ν λ§νλ€.

### μκ΅¬μ¬ν­ 6

JdbcTemplateκ³Ό κ°μ λ°©λ²μΌλ‘ SelectJdbcTemplateμ μμ±ν΄ λ°λ³΅ μ½λλ₯Ό λΆλ¦¬

```java
public abstract class SelectJdbcTemplate {
    public List query(String sql) throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            con = ConnectionManager.getConnection();
            pstmt = con.prepareStatement(sql);
            setValues(pstmt);

            rs = pstmt.executeQuery();

            List<Object> result = new ArrayList<>();
            if (rs.next()) {
                result.add(mapRow(rs));
            }

            return result;
        } finally {
            if (rs != null) {
                rs.close();
            }
            if (pstmt != null) {
                pstmt.close();
            }
            if (con != null) {
                con.close();
            }
        }
    }

    public Object queryForObject(String sql) throws SQLException {
        List result = query(sql);
        if (result.isEmpty()) {
            return null;
        }
        return result.get(0);
    }

    abstract void setValues(PreparedStatement pstmt) throws SQLException;
    abstract Object mapRow(ResultSet rs) throws SQLException;
}
```

- μ‘°νν λ°μ΄ν°λ₯Ό μλ° κ°μ²΄λ‘ λ³ννλ λΆλΆμΈ mapRow() λ©μλλ₯Ό μΆμ λ©μλλ‘ μΆκ°ν΄ κ΅¬ν
- userIdλ‘ μ‘°ννλ κ²½μ°λ₯Ό μν΄μ queryForObject() λ©μλ κ΅¬ν

```java
public List<User> findAll() throws SQLException {
        SelectJdbcTemplate jdbcTemplate = new SelectJdbcTemplate() {
            @Override
            void setValues(PreparedStatement pstmt) throws SQLException {

            }

            @Override
            Object mapRow(ResultSet rs) throws SQLException {
                return new User(
                        rs.getString("userId"),
                        rs.getString("password"),
                        rs.getString("name"),
                        rs.getString("email"));
            }
        };
        String sql = "SELECT userId, password, name, email FROM USERS";
        return (List<User>)jdbcTemplate.query(sql);
    }

    public User findByUserId(String userId) throws SQLException {
        SelectJdbcTemplate jdbcTemplate = new SelectJdbcTemplate() {
            @Override
            void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, userId);
            }

            @Override
            Object mapRow(ResultSet rs) throws SQLException {
                return new User(
                        rs.getString("userId"),
                        rs.getString("password"),
                        rs.getString("name"),
                        rs.getString("email"));
            }
        };
        String sql = "SELECT userId, password, name, email FROM USERS WHERE userId=?";
        return (User)jdbcTemplate.queryForObject(sql);
    }
```

- UserDaoμ findAll(), findByUserId() λ©μλλ€μ΄ SelectJdbcTemplateμ μ¬μ©νλλ‘ κ΅¬ν

### μκ΅¬μ¬ν­ 7

JdbcTemplateκ³Ό SelectJdbcTemplateμ ν κ°μ ν΄λμ€λ§μ μ κ³΅νλλ‘ λ¦¬ν©ν λ§

```java
public abstract class JdbcTemplate {
    public void update(String sql) throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            con = ConnectionManager.getConnection();
            pstmt = con.prepareStatement(sql);
            setValues(pstmt);
            pstmt.executeUpdate();
        } finally {
            if (pstmt != null) {
                pstmt.close();
            }

            if (con != null) {
                con.close();
            }
        }
    }

    public List query(String sql) throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            con = ConnectionManager.getConnection();
            pstmt = con.prepareStatement(sql);
            setValues(pstmt);

            rs = pstmt.executeQuery();

            List<Object> result = new ArrayList<>();
            if (rs.next()) {
                result.add(mapRow(rs));
            }

            return result;
        } finally {
            if (rs != null) {
                rs.close();
            }
            if (pstmt != null) {
                pstmt.close();
            }
            if (con != null) {
                con.close();
            }
        }
    }

    public Object queryForObject(String sql) throws SQLException {
        List result = query(sql);
        if (result.isEmpty()) {
            return null;
        }
        return result.get(0);
    }
    abstract Object mapRow(ResultSet rs) throws SQLException;
    abstract void setValues(PreparedStatement pstmt) throws SQLException;
}
```

- JdbcTemplateκ³Ό SelectJdbcTemplate ν΄λμ€λ₯Ό ν΅ν©

```java
public class UserDao {
    public void insert(User user) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate() {
            void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, user.getUserId());
                pstmt.setString(2, user.getPassword());
                pstmt.setString(3, user.getName());
                pstmt.setString(4, user.getEmail());
            }
            Object mapRow(ResultSet rs) throws SQLException {
                return null;
            }
        };
        String sql = "INSERT INTO USERS VALUES (?, ?, ?, ?)";
        jdbcTemplate.update(sql);
    }

    public void update(User user) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate() {
            void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, user.getPassword());
                pstmt.setString(2, user.getName());
                pstmt.setString(3, user.getEmail());
                pstmt.setString(4, user.getUserId());
            }
            Object mapRow(ResultSet rs) throws SQLException {
                return null;
            }
        };
        String sql = "UPDATE USERS SET password = ?, name = ?, email = ? WHERE userId = ?";
        jdbcTemplate.update(sql);
    }

    public List<User> findAll() throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate() {
            @Override
            void setValues(PreparedStatement pstmt) throws SQLException {

            }

            @Override
            Object mapRow(ResultSet rs) throws SQLException {
                return new User(
                        rs.getString("userId"),
                        rs.getString("password"),
                        rs.getString("name"),
                        rs.getString("email"));
            }
        };
        String sql = "SELECT userId, password, name, email FROM USERS";
        return (List<User>)jdbcTemplate.query(sql);
    }

    public User findByUserId(String userId) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate() {
            @Override
            void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, userId);
            }

            @Override
            Object mapRow(ResultSet rs) throws SQLException {
                return new User(
                        rs.getString("userId"),
                        rs.getString("password"),
                        rs.getString("name"),
                        rs.getString("email"));
            }
        };
        String sql = "SELECT userId, password, name, email FROM USERS WHERE userId=?";
        return (User)jdbcTemplate.queryForObject(sql);
    }
}
```

- UserDaoμ insert(), update() λ©μλμμ mapRow() λ©μλλ₯Ό κ΅¬ννλλ‘ μμ ν΄μΌνλ λΆνμν λ¬Έμ μ μ΄ μκΉ

### μκ΅¬μ¬ν­ 8

μμ κ°μ΄ SelectJdbcTemplate ν΄λμ€λ‘ ν΅ν©νμ λμ λ¬Έμ μ μ μ°Ύμλ³΄κ³  ν΄κ²°νκΈ° μν λ°©λ²μ μ°Ύμλ³Έλ€.

```java
public interface PreparedStatementSetter {
    void setValues(PreparedStatement pstmt) throws SQLException;
}
```

```java
public interface RowMapper {
    Object mapRow(ResultSet rs) throws SQLException;
}
```

- μ λ¬Έμ μ  ν΄κ²°μ μν΄ κ°κ°μ μΆμ λ©μλλ₯Ό μΈν°νμ΄μ€λ₯Ό ν΅ν΄ λΆλ¦¬

```java
public class JdbcTemplate {
    public void update(String sql, PreparedStatementSetter pss) throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            con = ConnectionManager.getConnection();
            pstmt = con.prepareStatement(sql);
            pss.setValues(pstmt);
            pstmt.executeUpdate();
        } finally {
            if (pstmt != null) {
                pstmt.close();
            }

            if (con != null) {
                con.close();
            }
        }
    }

    public List query(String sql, PreparedStatementSetter pss, RowMapper rowMapper) throws SQLException {
        Connection con = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            con = ConnectionManager.getConnection();
            pstmt = con.prepareStatement(sql);
            pss.setValues(pstmt);

            rs = pstmt.executeQuery();

            List<Object> result = new ArrayList<>();
            if (rs.next()) {
                result.add(rowMapper.mapRow(rs));
            }

            return result;
        } finally {
            if (rs != null) {
                rs.close();
            }
            if (pstmt != null) {
                pstmt.close();
            }
            if (con != null) {
                con.close();
            }
        }
    }

    public Object queryForObject(String sql, PreparedStatementSetter pss, RowMapper rowMapper) throws SQLException {
        List result = query(sql, pss, rowMapper);
        if (result.isEmpty()) {
            return null;
        }
        return result.get(0);
    }
}
```

```java
public class UserDao {
    public void insert(User user) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        PreparedStatementSetter pss = new PreparedStatementSetter() {
            @Override
            public void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, user.getUserId());
                pstmt.setString(2, user.getPassword());
                pstmt.setString(3, user.getName());
                pstmt.setString(4, user.getEmail());
            }
        };
        String sql = "INSERT INTO USERS VALUES (?, ?, ?, ?)";
        jdbcTemplate.update(sql, pss);
    }

    public void update(User user) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        PreparedStatementSetter pss = new PreparedStatementSetter() {
            @Override
            public void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, user.getPassword());
                pstmt.setString(2, user.getName());
                pstmt.setString(3, user.getEmail());
                pstmt.setString(4, user.getUserId());
            }
        };
        String sql = "UPDATE USERS SET password = ?, name = ?, email = ? WHERE userId = ?";
        jdbcTemplate.update(sql, pss);
    }

    public List<User> findAll() throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        PreparedStatementSetter pss = new PreparedStatementSetter() {
            @Override
            public void setValues(PreparedStatement pstmt) throws SQLException {

            }
        };
        RowMapper rowMapper = new RowMapper() {
            @Override
            public Object mapRow(ResultSet rs) throws SQLException {
                return new User(
                        rs.getString("userId"),
                        rs.getString("password"),
                        rs.getString("name"),
                        rs.getString("email"));
            }
        };
        String sql = "SELECT userId, password, name, email FROM USERS";
        return (List<User>)jdbcTemplate.query(sql, pss, rowMapper);
    }

    public User findByUserId(String userId) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        PreparedStatementSetter pss = new PreparedStatementSetter() {
            @Override
            public void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, userId);
            }
        };
        RowMapper rowMapper = new RowMapper() {
            @Override
            public Object mapRow(ResultSet rs) throws SQLException {
                return new User(
                        rs.getString("userId"),
                        rs.getString("password"),
                        rs.getString("name"),
                        rs.getString("email"));
            }
        };
        String sql = "SELECT userId, password, name, email FROM USERS WHERE userId=?";
        return (User)jdbcTemplate.queryForObject(sql, pss, rowMapper);
    }
}
```

- JdbcTemplateκ³Ό UserDaoμμλ PreparedStatementSetter, RowMapper μΈν°νμ΄μ€λ₯Ό νμ©νλλ‘ λ¦¬ν©ν λ§

> μ? findAllμ μν JdbcTemplateμ query λ©μλμμ PreparedStatementSetterλ μ νμ????
β
> 

### μκ΅¬μ¬ν­ 9

SQLExceptionμ λ°νμ ExceptionμΌλ‘ λ³νν΄ throwνλλ‘ νλ€. 

Connection, PreparedStatement μμ λ°λ©μ close() λ©μλλ₯Ό μ¬μ©νμ§ λ§κ³  try-with-resources κ΅¬λ¬Έμ μ μ©ν΄ ν΄κ²°νλ€.

```java
public class DataAccessException extends RuntimeException{
    private static final long serialVersionUID = 1L;

    public DataAccessException() {
        super();
    }

    public DataAccessException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace) {
        super(message, cause, enableSuppression, writableStackTrace);
    }

    public DataAccessException(String message, Throwable cause) {
        super(message, cause);
    }

    public DataAccessException(String message) {
        super(message);
    }

    public DataAccessException(Throwable cause) {
        super(cause);
    }
}
```

- RuntimeExceptionμ μμνλ μλ‘μ΄ Exception μΆκ°

```java
public class JdbcTemplate {
    public void update(String sql, PreparedStatementSetter pss) throws DataAccessException {
        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            con = ConnectionManager.getConnection();
            pstmt = con.prepareStatement(sql);
            pss.setValues(pstmt);
            pstmt.executeUpdate();
        } catch (SQLException e) {
            throw new DataAccessException(e);
        } finally {
            if (pstmt != null) {
                try {
                    pstmt.close();
                } catch (SQLException e) {
                    throw new DataAccessException(e);
                }
            }
            if (con != null) {
                try {
                    con.close();
                } catch (SQLException e) {
                    throw new DataAccessException(e);
                }

            }
        }
    }
[...]
```

- JdbcTemplateμμ μμ κ°μ΄ λ¦¬ν©ν λ§νμ¬ SQLExceptionμ μ²λ¦¬νμ§ μκ² λ°κΏ μ μμ
- νμ§λ§, finally μ μ λ³΅μ‘λκ° λλ¬΄ λμμ§λ λ¬Έμ μ  μκΉ

```java
public class JdbcTemplate {
    public void update(String sql, PreparedStatementSetter pss) throws DataAccessException {
        try (Connection conn = ConnectionManager.getConnection();
            PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pss.setValues(pstmt);
            pstmt.executeUpdate();
        } catch (SQLException e) {
            throw new DataAccessException(e);
        }
    }
[...]
```

- μμμ νμ©ν ν λ°λ©νκΈ° μν λͺ©μ μΌλ‘ μ¬μ©νλ close() λ©μλ νΈμΆ λΆλΆλ€μ μλ° 7 λ²μ λΆν° μ κ³΅νλ `java.io.AutoClosable` μΈν°νμ΄μ€λ₯Ό ν΅ν΄ ν΄κ²°ν  μ μλ€.
- `AutoClosable` μΈν°νμ΄μ€λ₯Ό κ΅¬ννλ ν΄λμ€λ try-with-resources κ΅¬λ¬Έμ νμ©ν΄ μμμ λ°λ©ν  μ μλ€.

### μκ΅¬μ¬ν­ 10

SELECTλ¬Έμ κ²½μ° μ‘°νν λ°μ΄ν°λ₯Ό μΊμ€ννλ λΆλΆμ΄ μλ€.

μΊμ€ννμ§ μκ³  κ΅¬ννλλ‘ κ°μ 

```java
public interface RowMapper<T> {
    T mapRow(ResultSet rs) throws SQLException;
}
```

- μ λλ¦­ λ¬Έλ²μ μ¬μ©νλλ‘ RowMapper μμ 

```java
public <T> List<T> query(String sql, PreparedStatementSetter pss, RowMapper<T> rowMapper) throws SQLException {
        ResultSet rs = null;
        try (Connection con = ConnectionManager.getConnection();
             PreparedStatement pstmt = con.prepareStatement(sql)) {
            pss.setValues(pstmt);

            rs = pstmt.executeQuery();

            List<T> result = new ArrayList<>();
            if (rs.next()) {
                result.add(rowMapper.mapRow(rs));
            }

            return result;
        } finally {
            if (rs != null) {
                rs.close();
            }
        }
    }

    public <T> T queryForObject(String sql, PreparedStatementSetter pss, RowMapper<T> rowMapper) throws SQLException {
        List<T> result = query(sql, pss, rowMapper);
        if (result.isEmpty()) {
            return null;
        }
        return result.get(0);
    }
```

```java
public List<User> findAll() throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        PreparedStatementSetter pss = new PreparedStatementSetter() {
            @Override
            public void setValues(PreparedStatement pstmt) throws SQLException {

            }
        };
        RowMapper<User> rowMapper = new RowMapper<User>() {
            @Override
            public User mapRow(ResultSet rs) throws SQLException {
                return new User(
                        rs.getString("userId"),
                        rs.getString("password"),
                        rs.getString("name"),
                        rs.getString("email"));
            }
        };
        String sql = "SELECT userId, password, name, email FROM USERS";
        return jdbcTemplate.query(sql, pss, rowMapper);
    }

    public User findByUserId(String userId) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        PreparedStatementSetter pss = new PreparedStatementSetter() {
            @Override
            public void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, userId);
            }
        };
        RowMapper<User> rowMapper = new RowMapper<User>() {
            @Override
            public User mapRow(ResultSet rs) throws SQLException {
                return new User(
                        rs.getString("userId"),
                        rs.getString("password"),
                        rs.getString("name"),
                        rs.getString("email"));
            }
        };
        String sql = "SELECT userId, password, name, email FROM USERS WHERE userId=?";
        return jdbcTemplate.queryForObject(sql, pss, rowMapper);
    }
```

- μ λλ¦­μ μ¬μ©νλλ‘ JdbcTemplate, UserDaoμ λ©μλλ€ μμ 

### μκ΅¬μ¬ν­ 11

κ° μΏΌλ¦¬μ μ λ¬ν  μΈμλ₯Ό PreparedStatementSetterλ₯Ό ν΅ν΄ μ λ¬ν  μλ μμ§λ§ μλ°μ κ°λ³μΈμλ₯Ό ν΅ν΄ μ λ¬ν  μ μλ λ©μλλ₯Ό μΆκ°νλ€.

```java
public class JdbcTemplate {
    public void update(String sql, Object... parameters) throws DataAccessException {
        try (Connection conn = ConnectionManager.getConnection();
            PreparedStatement pstmt = conn.prepareStatement(sql)) {
            for (int i = 0; i < parameters.length; i++) {
                pstmt.setObject(i + 1, parameters[i]);
            }
            pstmt.executeUpdate();
        } catch (SQLException e) {
            throw new DataAccessException(e);
        }
    }
[...]
```

- κ°λ³μΈμλ₯Ό νμ©νλλ‘ JdbcTemplate μμ 

```java
public class UserDao {
    public void insert(User user) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        String sql = "INSERT INTO USERS VALUES (?, ?, ?, ?)";
        jdbcTemplate.update(sql, user.getUserId(), user.getPassword(), user.getName(), user.getEmail());
    }

    public void update(User user) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        String sql = "UPDATE USERS SET password = ?, name = ?, email = ? WHERE userId = ?";
        jdbcTemplate.update(sql, user.getPassword(), user.getName(), user.getEmail(), user.getUserId());
    }
[...]
```

- UserDaoλ κ°λ³μΈμ νμ©νλλ‘ μμ 

### μκ΅¬μ¬ν­ 12

UserDaoμμ PreparedStatementSetter, RowMapper μΈν°νμ΄μ€λ₯Ό κ΅¬ννλ λΆλΆμ JDK 8μμ μΆκ°ν λλ€ ννμμ νμ©νλλ‘ λ¦¬ν©ν λ§

```java
public User findByUserId(String userId) throws SQLException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        PreparedStatementSetter pss = new PreparedStatementSetter() {
            @Override
            public void setValues(PreparedStatement pstmt) throws SQLException {
                pstmt.setString(1, userId);
            }
        };
        String sql = "SELECT userId, password, name, email FROM USERS WHERE userId=?";
        return jdbcTemplate.queryForObject(sql, pss, (ResultSet rs) -> {
            return new User(
                    rs.getString("userId"),
                    rs.getString("password"),
                    rs.getString("name"),
                    rs.getString("email"));
        });
 }
```

```java
@FunctionalInterface
public interface RowMapper<T> {
    T mapRow(ResultSet rs) throws SQLException;
}
```

- λλ€ ννμμ μ¬μ©νκΈ° μν΄ μΈν°νμ΄μ€μ `@FunctionalInterface` μ΄λΈνμ΄μ μΆκ°
