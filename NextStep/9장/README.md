# **π² μμ²΄ μ κ² μκ΅¬μ¬ν­**
π‘ 1. λ‘μ»¬ κ°λ° νκ²½μ ν°μΊ£ μλ²λ₯Ό μμνλ©΄ μλΈλ¦Ώ μ»¨νμ΄λμ μ΄κΈ°ν κ³Όμ μ΄ μ§νλλ€. μ΄κΈ°νλλ κ³Όμ μ λν΄ μ€λͺνλΌ. (νλ‘μ νΈμ μ΄κΈ°ν κ³Όμ )
- μλΈλ¦Ώ μ»¨νμ΄λλ μΉ μ νλ¦¬μΌμ΄μμ μνλ₯Ό κ΄λ¦¬νλ `ServletContext` λ₯Ό μμ±νλ€.
- `ServletContext` κ° μ΄κΈ°νλλ©΄, μ΄κΈ°ν μ΄λ²€νΈκ° λ°μλλ€.
- λ±λ‘λ `ServletContextListener` μ μ½λ°± λ©μλκ° νΈμΆλλ€.
    - μ¬κΈ°μλ `public class ContextLoaderListener implements ServletContextListener` λ‘ `ContextLoaderListener` κ° `ServletContextListener` λ₯Ό κ΅¬ννκ³  μμΌλ―λ‘, ν΄λΉ κ΅¬νλΆμ `contextInitialized()` κ° νΈμΆλλ€.

```java
ResourceDatabasePopulator populator = new ResourceDatabasePopulator();
populator.addScript(new ClassPathResource("jwp.sql"));
DatabasePopulatorUtils.execute(populator, ConnectionManager.getDataSource());
```

- `contextInitialized()` λ©μλλ‘, `jwp.sql` νμΌμ μ€νν΄ DB νμ΄λΈμ μ΄κΈ°ννλ€.
- μλΈλ¦Ώ μ»¨νμ΄λλ ν΄λΌμ΄μΈνΈμ μμ²­μ λ°μ `DispatcherServlet` μΈμ€ν΄μ€λ₯Ό μμ±νλλ°, `loadOnStartup` μμ±μ λ°λΌ μμ± μμ μ΄ κ²°μ λλ€.
    - μ¬κΈ°μλ `loadOnStartup` μ μ€μ ν΄μ, μλΈλ¦Ώ μ»¨νμ΄λ μμ μμ μ μΈμ€ν΄μ€λ₯Ό μμ±νλ€.
    - 6μ₯μ μ€λͺ μ°Έκ³ 
- `DispatcherServlet` μ `init()` λ©μλλ₯Ό μ€ννμ¬ `RequestMapping` κ°μ²΄λ₯Ό μμ±νλ€.
    - μμ±ν `RequestMapping` μ `initMapping()` λ©μλλ₯Ό νΈμΆν΄ μμ²­ URLκ³Ό `Controller` μΈμ€ν΄μ€λ₯Ό λ§€ννλ€.

π‘ 2. λ‘μ»¬ κ°λ° νκ²½μ ν°μΊ£ μλ²λ₯Ό μμν ν `http://localhost:8080` μΌλ‘ μ κ·Όνλ©΄ μ§λ¬Έ λͺ©λ‘μ νμΈν  μ μλ€. URLλ‘ μ κ·Όν΄μ μ§λ¬Έ λͺ©λ‘μ΄ λ³΄μ΄κΈ°κΉμ§μ μμ€μ½λμ νΈμΆ μμ λ° νλ¦μ μ€λͺνλΌ.
- μμ²­μ μ²λ¦¬νκΈ° μ , λ¨Όμ  `web.filter` μ `ResourceFilter` μ `CharacterEncodingFilter` μ `doFilter()` λ©μλκ° μ€νλλ€. (μ΄ λμ `Filter` λ₯Ό κ΅¬ν)
- νν°λ₯Ό κ±°μΉ ν, `urlPatterns="/"` μΈ `DispatcherServlet` μΌλ‘ μμ²­μ΄ μ λ¬λμ΄ `service()` λ©μλκ° νΈμΆλλ€.
    - μμ²­ URLμ λμνλ `Controller` κ°μ²΄μΈ `HomeController` λ₯Ό λ°ννλ€.
    - μ¦, `HomeController` μκ² μμμ μμνμ¬ `HomeController` μ `execute()` λ©μλκ° μ€νλλ€.
    - ν΄λΉ λ©μλλ `return jspView("home.jsp").addObject("questions", questionDao.findAll());` λ₯Ό λ°ννκ³ , `DispatcherServlet` λ μ΄λ₯Ό λ°μ `view.render(mav.getModel(), req, resp);` νλ€. `JspView` μ `render()` λ μ λ¬λ λͺ¨λΈ λ°μ΄ν°λ₯Ό `home.jsp` μ μ λ¬ν΄ HTMLμ μμ±νκ³  μλ΅νλ€.

π‘ 3. μ§λ¬Έ λͺ©λ‘μ μ μ λμνμ§λ§ μ§λ¬ΈνκΈ° κΈ°λ₯μ λμνμ§ μλλ€. μ΄λ₯Ό κ΅¬ννλ€. μ§λ¬Έ μΆκ° λ‘μ§μ `QuetionDao` ν΄λμ€μ `insert()` λ©μλλ₯Ό νμ©νλ€. μ§λ¬ΈνκΈ° μ±κ³΅ ν μ§λ¬Έ λͺ©λ‘ νμ΄μ§(`"/"`)λ‘ μ΄λν΄μΌ νλ€.
- λ¨Όμ , μ§λ¬ΈνκΈ° λ²νΌμ `href` λ₯Ό λ³΄λ©΄, `/qna/form` μΌλ‘ μ΄λνλλ‘ νλ€.
    - λ°λΌμ, `/qna/form` μμ²­μ μ²λ¦¬ν  μ»¨νΈλ‘€λ¬λ₯Ό μμ±νκ³ , μ΄λ₯Ό `RequestMapping` μ λ±λ‘νλ€.
    - μμ±ν μ»¨νΈλ‘€λ¬λ μ§κΈκΉμ§μ λμΌνκ² `AbstractController` λ₯Ό μμνκ³ , `execute()` λ₯Ό κ΅¬ννλ€.
    - μ΄ μ»¨νΈλ‘€λ¬μμλ `/qna/form.jsp` λ₯Ό λ°ννλ€.
- `form.jsp` μμ κΈμ΄μ΄, μ λͺ©, λ΄μ©μ μλ ₯ν ν, `/qna/create` λ‘ POST μμ²­μ λ³΄λ΄λλ‘ νλ€.
    - μ΄λ₯Ό μ²λ¦¬ν  μ»¨νΈλ‘€λ¬λ₯Ό μμ±νκ³ , λ§€ννλ€.
    - ν΄λΉ μ»¨νΈλ‘€λ¬μμ sessionμΌλ‘λΆν° κ°μ Έμ¨ `userId` , μ¬μ©μ μλ ₯ κ°λ€(`title` , `contents`)μ μ¬μ©νμ¬ `Question` κ°μ²΄λ₯Ό μμ±νλ€.
    - κ·Έλ¦¬κ³  μμ±ν μ§λ¬Έ κ°μ²΄λ₯Ό `QuestionDao` μ `insert()` λ©μλλ₯Ό ν΅ν΄ DBμ μ μ₯νλ€.
    - `QuestionDao` μ `insert()` λ©μλλ DBμ μΈμλ‘ λ°μ μ§λ¬Έ κ°μ²΄λ₯Ό μ½μνλ μ­ν μ μννλ€.

```java
public Question insert(Question question) {
    JdbcTemplate jdbcTemplate = new JdbcTemplate();
    String sql = "INSERT INTO QUESTIONS (writer, title, contents, createdDate) VALUES (?, ?, ?, ?)";
    PreparedStatementCreator psc = new PreparedStatementCreator() {
        @Override
        public PreparedStatement createPreparedStatement(Connection con) throws SQLException {
            PreparedStatement pstmt = con.prepareStatement(sql);
            pstmt.setString(1, question.getWriter());
            pstmt.setString(2, question.getTitle());
            pstmt.setString(3, question.getContents());
            pstmt.setTimestamp(4, new Timestamp(question.getTimeFromCreateDate()));
            return pstmt;
        }
    };

    KeyHolder keyHolder = new KeyHolder();
    jdbcTemplate.update(psc, keyHolder);
    return findById(keyHolder.getId());
}
```

π‘ 4. λ‘κ·ΈμΈνμ§ μμ μ¬μ©μλ μ§λ¬ΈνκΈ°κ° κ°λ₯νλ€. μ΄λ₯Ό λ‘κ·ΈμΈν μ¬μ©μλ§ μ§λ¬Έμ΄ κ°λ₯νλλ‘ μμ νλ€. μ΄λ κΈμ΄μ΄λ₯Ό μλ ₯νμ§ μκ³  λ‘κ·ΈμΈν μ¬μ©μ μ λ³΄λ₯Ό κ°μ Έμ κΈμ΄μ΄ μ΄λ¦μΌλ‘ μ¬μ©νλλ‘ κ΅¬ννλ€.
- μ§λ¬ΈνκΈ° λ²νΌ ν΄λ¦­ μ, λ¨Όμ  sessionμΌλ‘λΆν° μ¬μ©μ μ λ³΄λ₯Ό κ°μ Έμ λ‘κ·ΈμΈλ μ¬μ©μμΈμ§ νμΈνκ³ , κ·Έλ μ§ μμ κ²½μ°μλ `/users/loginForm` μΌλ‘ λ³΄λΈλ€.
- `CreateQuestionController` μμλ λ‘κ·ΈμΈ μ λ¬΄λ₯Ό νμΈν ν μ¬μ©μκ° μλ ₯ν κΈμ΄μ΄ μ λ³΄κ° μλ sessionμΌλ‘λΆν° κ°μ Έμ¨ `userId` λ₯Ό `Question` κ°μ²΄μ λ΄μ μ μ₯νλ€.

π‘ 5. μ§λ¬Έ λͺ©λ‘μμ μ λͺ©μ ν΄λ¦­νλ©΄ μμΈλ³΄κΈ° νλ©΄μΌλ‘ μ΄λνλ€. ν΄λΉ νλ©΄μμ λ΅λ³ λͺ©λ‘μ΄ μ μ μΈ HTMLλ‘ κ΅¬νλμ΄ μλλ°, μ΄λ₯Ό DBμ μ μ₯λμ΄ μλ λ΅λ³μ μΆλ ₯νλλ‘ κ΅¬ννλ€. μ€ν΄λ¦½νλ¦Ώμ μ¬μ©νμ§ μκ³  JSTLκ³Ό ELλ§μΌλ‘ κ΅¬ννλ€.
- λΈλμΉ λ°κΏμ λ³΄λ©΄ νμΈν  μ μλ€!

π‘ 6. μλ° κΈ°λ° μΉ νλ‘κ·Έλλ°μ ν  κ²½μ° νκΈμ΄ κΉ¨μ§λ€. μ΄λ₯Ό `ServletFilter` λ₯Ό νμ©ν΄ ν΄κ²°ν  μ μλ€. `core.web.filter.CharacterEncodingFilter` μ annotation μ€μ μ ν΅ν΄ νκΈ λ¬Έμ λ₯Ό ν΄κ²°νλ€.
- `@WebFilter(value= {"/*"}, initParams=@WebInitParam(name="encoding", value="utf-8"))` annotationμ μ€μ νλ€.
- `request.setCharacterEncoding(DEFAULT_ENCODING);` κ³Ό`response.setCharacterEncoding(DEFAULT_ENCODING);` μΌλ‘ requestμ responseμ λν νκΈ μΈμ½λ© μ²λ¦¬λ₯Ό μννλ€. (*`DEFAULT_ENCODING*= "UTF-8";`)
- `FilterChain` κ°μ²΄λ₯Ό μ¬μ©νμ¬ `chain.doFilter(request, response);` λ©μλλ₯Ό νΈμΆνλ€. μ΄λ νν°μμ(νκΈμ²λ¦¬)μ λ΄μ©μ΄ μλ²μμ λ€μ μ»΄ν¬λνΈμκ² κ³μ μ μ©λλλ‘ νκΈ° μν΄ νμνλ€.

π‘ 7. `next.web.qna` ν¨ν€μ§μ `ShowController` λ λ©ν°μ€λ λ μν©μμ λ¬Έμ κ° λ°μν  κ°λ₯μ±μ΄ μλ€. μ΄λ₯Ό λ¬Έμ κ° λ°μνμ§ μλλ‘ μμ νκ³ , λ¬Έμ κ° λλ μ΄μ λ₯Ό μ€λͺνλΌ.
- ν΄λμ€μ μΈμ€ν΄μ€λ₯Ό μμ±νλ κ²κ³Ό μΈμ€ν΄μ€λ₯Ό λ μ΄μ μ¬μ©νμ§ μλ κ²½μ° **Garbage Collection**μ ν΅ν΄ λ©λͺ¨λ¦¬μμ ν΄μ νλ κ³Όμ  λͺ¨λ **λΉμ©μ΄ λ°μ**νλ€.
    - λ°λΌμ, μΈμ€ν΄μ€λ₯Ό λ§€λ² μμ±ν  νμκ° μλ κ²½μ°, μμ±λ μΈμ€ν΄μ€λ₯Ό μ¬μ¬μ©νλ κ²μ΄ μ±λ₯ μΈ‘λ©΄μμ λ μ λ¦¬ν  μ μλ€.
    - μ΄λ **μΈμ€ν΄μ€κ° μν κ°μ μ μ§ν΄μΌ νλμ§ μ¬λΆ**μ λ°λΌ κ΅¬λΆλλ€.
    - μλ‘, `model` μ `User` , `Question` , `Answer` μ κ²½μ° ν΄λΌμ΄μΈνΈλ§λ€ μλ‘ λ€λ₯Έ μν κ°μ κ°μ§κΈ°μ, λ§€ μμ²­λ§λ€ μΈμ€ν΄μ€λ₯Ό μμ±νμ¬ μ²λ¦¬ν΄μΌ νλ€.
    - νμ§λ§, `JdbcTemplate` , λͺ¨λ  `Dao` , `Controller` λ λ§€ μμ²­λ§λ€ μλ‘ λ€λ₯Έ μν κ°μ κ°μ§μ§ μμ νλμ μΈμ€ν΄μ€λ₯Ό μ¬μ¬μ©ν  μ μλ€.
- μλΈλ¦Ώμ μλΈλ¦Ώ μ»¨νμ΄λ μμ μ μΈμ€ν΄μ€ νλλ₯Ό μμ±ν΄ μ¬μ¬μ©νλ€. `RequestMapping` κ³Ό κ° μ»¨νΈλ‘€λ¬μ μΈμ€ν΄μ€ λν λͺ¨λ νλμ΄λ€.
    - νμ§λ§ μλΈλ¦Ώ μ»¨νμ΄λλ **λ©ν°μ€λ λ νκ²½**μμ λμνλ€. **μ¬λ¬ λͺμ μ¬μ©μκ° μΈμ€ν΄μ€ νλλ₯Ό μ¬μ¬μ©**νλ€λ μλ―Έμ΄λ―λ‘, μλͺ»λ κ΅¬νμΌλ‘ μΈν΄ λ²κ·Έκ° λ°μν  μ μλ€. (μ¬λ¬ λͺμ μ¬μ©μκ° λμμ κ°μ μ½λλ₯Ό μ€ννλ κ²½μ° λ°μ)

`ShowController` λ μΈμ€ν΄μ€ νλλ₯Ό μ¬λ¬ κ°μ μ€λ λκ° κ³΅μ νλ€. λ¨Όμ  λ©λͺ¨λ¦¬μμ μ΄λ»κ² λμνλμ§λ₯Ό μ΄ν΄λ³Έλ€.

- JVMμ μ½λλ₯Ό μ€ννκΈ° μν΄ λ©λͺ¨λ¦¬λ₯Ό μ€νκ³Ό ν μμ­μΌλ‘ λλ μ κ΄λ¦¬νλ€.
    - μ€ν μμ­μ κ° λ©μλκ° μ€νλ  λ, λ©μλμ μΈμ, μ§μ­ λ³μ λ±μ κ΄λ¦¬νλ μμ­μΌλ‘, κ° μ€λ λλ§λ€ κ³ μ ν μμ­μ κ°μ§λ€.
    - ν μμ­μ ν΄λμ€μ μΈμ€ν΄μ€ μν λ°μ΄ν°λ₯Ό κ΄λ¦¬νλ μμ­μΌλ‘, μ€λ λλΌλ¦¬ κ³΅μ ν  μ μλ€.

<img src="https://user-images.githubusercontent.com/33208303/158941284-62fcb070-c141-4e7f-bf51-0f6de0138806.png" width="80%">

- JVMμ κ° λ©μλλ³λ‘ μ€ν νλ μμ μμ±νλ€.
- `ShowController` μ `execute()` λ©μλ μ€ν μ, ν΄λΉ λ©μλμ λν μ€ν νλ μμ μ§μ­ λ³μ μμ­μ μ²«λ²μ§Έ μμΉμ μκΈ° μμ (`this`)μ λν λ©λͺ¨λ¦¬ μμΉλ₯Ό μμ±νλ€.
- `ShowController` μ μΈμ€ν΄μ€λ νμ μμ±λλ©°, `Question` , `List<Answer>` νλλ₯Ό κ°μ§λ―λ‘ νμ μμ±λ `Question` κ³Ό `List<Answer>` μΈμ€ν΄μ€λ₯Ό κ°λ¦¬ν€λ κ΅¬μ‘°λ‘ μ€νλλ€.

μ κ΅¬μ‘°μμ, 2κ°μ μ€λ λκ° `execute()` λ©μλλ₯Ό μ€ννκ² λλ©΄,

- μ²« λ²μ§Έ μ€λ λκ° μ κ·Όνμ λλ λ¬Έμ κ° λμ§ μλλ€.
- νμ§λ§, μ²« λ²μ§Έ μ€λ λμ μμμ΄ μλ£λμ§ μμ μμ μμ λ λ²μ§Έ μ€λ λκ° `execute()` λ©μλλ₯Ό μ€ννκ² λλ©΄, λ©λͺ¨λ¦¬ κ΅¬μ‘°κ° λ€μκ³Ό κ°μ΄ λ³νλ€.

<img src="https://user-images.githubusercontent.com/33208303/158941293-e16abad0-ea28-42e2-88e3-51161d13bea4.png" width="80%">

- `ShowController` κ° κ°λ¦¬ν€λ νλκ° **1λ²μμ 2λ²μΌλ‘ λ³κ²½**λμλ€.
- λ λ²μ§Έ μ€λ λλ μ μμ μΌλ‘ μ§λ¬Έκ³Ό λ΅λ³μ μλ΅μΌλ‘ λ°μ§λ§, μ²« λ²μ§Έ μ€λ λλ 1λ² κΈμ λ³΄κΈ° μν μμ²­μ λ³΄λμμλ λ λ²μ§Έ μ€λ λκ° μ€νν `execute()` λ‘ μΈν΄ `ShowController` κ° κ°λ¦¬ν€λ νλκ° 2λ²μΌλ‘ λ³κ²½λμ΄ **2λ² μ§λ¬Έκ³Ό λ΅λ³μ μλ΅**μΌλ‘ λ°κ² λλ€.
- μ²« λ²μ§Έ μ€λ λκ° μλ‘κ³ μΉ¨μ νκ² λλ©΄ μ μμ μΈ μλ΅μ λ°μ μ μμ΄ λ²κ·Έκ° κ³ μ³μ§λ λ― νμ§λ§, μ΄λ¬ν λ²κ·Έκ° λ€λ₯Έ μ€μν κΈ°λ₯μλ μ μ©λλ€λ©΄, ν° νΌν΄λ₯Ό μμ μ μλ€.

> "λ©ν°μ€λ λμμ μΉ μ νλ¦¬μΌμ΄μ κ°λ° μ κ΅¬ννκ³  μλ μ½λκ° μ΄λ»κ² λμνλμ§λ₯Ό μ΄ν΄νλ κ²μ λ§€μ° μ€μνλ€."

μ΄λ¬ν λ²κ·Έλ `Question` κ³Ό `List<Answer>` λ₯Ό `ShowController` μ νλκ° μλ `execute()` λ©μλμ μ§μ­ λ³μλ‘ κ΅¬νν¨μΌλ‘μ¨ ν΄κ²°ν  μ μλ€.

```java
public class ShowController extends AbstractController {
    private QuestionDao questionDao = new QuestionDao();
    private AnswerDao answerDao = new AnswerDao();

    @Override
    public ModelAndView execute(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        Long questionId = Long.parseLong(req.getParameter("questionId"));

        Question question = questionDao.findById(questionId);
        List<Answer> answers = answerDao.findAllByQuestionId(questionId);

        return jspView("/qna/show.jsp").addObject("question", question).addObject("answers", answers);
    }
}
```

<img src="https://user-images.githubusercontent.com/33208303/158941298-a5032e02-b27c-4842-bd97-9e45e9760b3e.png" width="80%">

- μ΄μ κ°μ΄ κ΅¬ννλ©΄, `ShowController` κ° `Question` κ³Ό `List<Answer>` μΈμ€ν΄μ€μ λν μ°Έμ‘°λ₯Ό κ°μ§μ§ μκ³  `ShowController` `execute()` λ©μλμ **μ€ν νλ μμ μ§μ­ λ³μ μμ­μμ ν΄λΉ μΈμ€ν΄μ€μ λν μ°Έμ‘°**λ₯Ό κ°μ§λ€.
- 2κ°μ μ€λ λκ° μ κ·ΌνλλΌλ, μλ‘ κ°μ `ShowController` μΈμ€ν΄μ€λ₯Ό μ°Έμ‘°νμ§λ§ ν λ©λͺ¨λ¦¬μ μμ±λ `Question` κ³Ό `List<Answer>` λ μλ‘ κ³ μ ν μΈμ€ν΄μ€λ₯Ό μ°Έμ‘°νκ² λλ€.

> π `QuestionDao` μ `AnswerDao` λ λͺ¨λ μν κ°μ κ°μ§μ§ μκΈ°μ λ©ν°μ€λ λ νκ²½μμλ λ¬Έμ κ° λμ§ μλλ€.

π‘ 8. μμΈλ³΄κΈ° νλ©΄μμ λ΅λ³νκΈ° κΈ°λ₯μ μ μμ μΌλ‘ λμνλ€. λ¨, λ΅λ³ μΆκ° μ μ μ²΄ λ΅λ³μ μκ° μ¦κ°νμ§ μλλ€. λ΅λ³μ μΆκ°νλ μμ μ μ§λ¬Έ(`QUESTION` νμ΄λΈ)μ λ΅λ³ μ(`countOfAnswer`)λ 1 μ¦κ°μν¨λ€.
- λ΅λ³μ μΆκ°νλ μμ μ λ΅λ³ μλ₯Ό μ¦κ°μμΌμΌ νλ―λ‘, `AddAnswerController` μμ `answerDao.insert()` μ΄ν λ‘μ§μ΄ μΆκ°λμ΄μΌ νλ€.
- λ¨Όμ , `questionDao` μ `questionId` λ₯Ό μΈμλ‘ λ°μ μ§λ¬Έμ λν λ΅λ³ μλ₯Ό μ¦κ°μν€λ DB λ‘μ§μ κ΅¬ννλ€.

```java
public void updateCountOfAnswer(long questionId) {
  JdbcTemplate jdbcTemplate = new JdbcTemplate();
  String sql = "UPDATE QUESTIONS SET countOfAnswer=countOfAnswer+1 "
          + "WHERE questionId = ?";

  jdbcTemplate.update(sql, questionId);
}
```

- κ΅¬νν λ‘μ§μ `AddAnswerController` μμ μ¬μ©νλλ‘ νλ€.

```java
public class AddAnswerController extends AbstractController {
    ...

    private QuestionDao questionDao = new QuestionDao();

    @Override
    public ModelAndView execute(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        ...

        Answer savedAnswer = answerDao.insert(answer);
        questionDao.updateCountOfAnswer(savedAnswer.getQuestionId());
        return jsonView().addObject("answer", savedAnswer);
    }
}
```

π‘ 9. ν΄λΉ μλΉμ€λ λͺ¨λ°μΌμμλ μλΉμ€ν  κ³νμ΄λΌ APIλ₯Ό μΆκ°ν΄μΌ νλ€. `/api/qna/list` URLλ‘ μ κ·Ό μ μ§λ¬Έ λͺ©λ‘μ JSON λ°μ΄ν°λ‘ μ‘°νν  μ μλλ‘ κ΅¬ννλ€.
- λ¨Όμ , ν΄λΉ μμ²­ URLμ μ²λ¦¬ν  μ»¨νΈλ‘€λ¬λ₯Ό μμ±νκ³ , `RequestMapping` μμ URLκ³Ό ν¨κ» λ§€ννλ€.
- μμ±ν μ»¨νΈλ‘€λ¬λ `QuestionDao` μ `findAll()` κ²°κ³Όλ₯Ό `jsonView()` λ‘ λ°ννλ€.

```java
public class ApiListQuestionController extends AbstractController {
    private QuestionDao questionDao = new QuestionDao();

    @Override
    public ModelAndView execute(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        return jsonView().addObject("question", questionDao.findAll());
    }
}
```

π‘ 10. μμΈλ³΄κΈ° νλ©΄μ λ΅λ³ λͺ©λ‘μμ λ΅λ³μ μ­μ ν΄μΌ νλ€. λ΅λ³ μ­μ  λν μλ‘κ³ μΉ¨ μμ΄ κ΅¬νμ΄ κ°λ₯νλλ‘ AJAXλ‘ κ΅¬ννλ€.
- `show.jsp` μμ λ΅λ³ μ­μ  λ²νΌ ν΄λ¦­ μ μμ²­μ λ³΄λΌ URLμ λͺμνκ³ , μ΄λ₯Ό μ²λ¦¬ν  μ»¨νΈλ‘€λ¬λ₯Ό μμ±νλ€. (λ§€νλ μν)
- `webapp/js/scripts.js` μμ μ­μ  λ²νΌ ν΄λ¦­ μ λ°μμν¬ μ΄λ²€νΈλ₯Ό λͺμνκ³ , AJAXλ₯Ό μ¬μ©νμ¬ μλ²μκ² μμ²­μ λ³΄λΈλ€.

```java
$(".qna-comment").on("click", ".form-delete", deleteAnswer);

function deleteAnswer(e) {
  e.preventDefault();

  var deleteBtn = $(this);
  var queryString = deleteBtn.closest("form").serialize();

  $.ajax({
    type: 'post',
    url: "/api/qna/deleteAnswer",
    data: queryString,
    dataType: 'json',
    error: function (xhr, status) {
      alert("error");
    },
    success: function (json, status) {
      var result = json.result;
      if (result.status) {
        deleteBtn.closest('article').remove();
      }
    }
  });
}
```

- μ»¨νΈλ‘€λ¬μμλ μμ²­μΌλ‘λΆν° `answerId` λ₯Ό κΊΌλ΄ DBμμ ν΄λΉ λ΅λ³μ μ­μ νκ³ , κ²°κ³Όλ₯Ό λ°ννλ€.
- λ°νλ°μ κ²°κ³Όμ λ°λΌ, `error` or `success` λΆλΆμ μ€ννμ¬ λ΅λ³μ μ­μ νλ€.

```java
public class DeleteAnswerController extends AbstractController {
    private AnswerDao answerDao = new AnswerDao();

    @Override
    public ModelAndView execute(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        Long answerId = Long.parseLong(req.getParameter("answerId"));
        try {
            answerDao.delete(answerId);
            return jsonView().addObject("result", Result.ok());
        } catch (DataAccessException e) {
            return jsonView().addObject("result", Result.fail(e.getMessage()));
        }
    }
}
```

> π λ΅λ³ μ­μ  μ, `countOfAnswers` λ₯Ό -1 ν΄μ£Όλ λ‘μ§μ΄ νμν  κ² κ°λ€.

π‘ 11. μ§λ¬Έ μμ μ΄ κ°λ₯ν΄μΌ νλ€. μ΄λ κΈμ΄μ΄μ λ‘κ·ΈμΈ μ¬μ©μκ° κ°μ κ²½μ°μλ§ κ°λ₯ν΄μΌ νλ€.
- μ§λ¬Έ μμ  λ²νΌ ν΄λ¦­ μ, μμ²­μ λ³΄λΌ URLμ λͺμνκ³ , μ§λ¬Έ μμ  μμμΌλ‘ λκ²¨μ€ μ»¨νΈλ‘€λ¬ μμ± ν `RequestMapping` μμ λ§€ννλ€.
    - μ΄λ, ν΄λΉ μ§λ¬Έμ μ λ³΄λ₯Ό GETλ°©μμΌλ‘ URLμ λͺμνμ¬ μ λ¬νλ€. (`question.questionId`)
- μ»¨νΈλ‘€λ¬μμλ κΈμ΄μ΄μ λ‘κ·ΈμΈ μ¬μ©μκ° λμΌνμ§ κ²μ¬νλ€.
    - μ΄λ, `Question` μ `isSameUser()` λ©μλλ₯Ό κ΅¬νν΄ μ¬μ©νλλ‘ νλ€.
    - ν΄λΉ λ©μλμλ sessionμΌλ‘λΆν° μ»μ λ‘κ·ΈμΈ μ¬μ©μ μ λ³΄λ₯Ό μΈμλ‘ λκ²¨μ£Όκ³ , μ΄λ₯Ό λ°μ λ©μλλ `writer id`λ₯Ό κ°μ§κ³ , μ¬μ©μκ° λμΌνμ§ κ²μ¬νλ€.
- λμΌν κ²½μ°μλ§ μ§λ¬Έ μμ  μ»¨νΈλ‘€λ¬λ‘ λκ²¨μ€λ€.

```java
public class UpdateFormQuestionController extends AbstractController {
    private QuestionDao questionDao = new QuestionDao();

    @Override
    public ModelAndView execute(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        if (!UserSessionUtils.isLogined(req.getSession())) {
            return jspView("redirect:/users/loginForm");
        }

        Long questionId = Long.parseLong(req.getParameter("questionId"));
        Question question = questionDao.findById(questionId);

        if (!question.isSameUser(UserSessionUtils.getUserFromSession(req.getSession()))) {
            throw new IllegalStateException("λ³ΈμΈμ΄ μμ±ν κΈλ§ μμ ν  μ μμ΅λλ€.");
        }

        return jspView("/qna/update.jsp").addObject("question", question);
    }
}
```

- `/qna/update.jsp` λ μ§λ¬Έμ μ λͺ©κ³Ό λ΄μ©μ μμ ν  μ μλ `form` μ΄ μκ³ , `/qna/update` λ‘ POST μμ²­μ λ³΄λΈλ€.
- μ΄λ₯Ό μ²λ¦¬ν  μ»¨νΈλ‘€λ¬ μμ± λ° λ§€νμ μννλ€.
- μ»¨νΈλ‘€λ¬μμλ `questionId` , `title` , `contents` λ₯Ό λ°μ μ§λ¬Έ DBμ UPDATEνλ€.
    - λ¨Όμ , `Question` μ `update()` λ©μλλ‘ κΈ°μ‘΄ μ§λ¬Έ κ°μ²΄μ μ λ³΄λ₯Ό λ³κ²½νκ³ , `QuestionDao` μ `update()` λ©μλλ‘ `QUESTION` DBμ μλ ν΄λΉ μ§λ¬Έμ UPDATEνλ€.

```java
public class UpdateQuestionController extends AbstractController {
    private QuestionDao questionDao = new QuestionDao();

    @Override
    public ModelAndView execute(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        if (!UserSessionUtils.isLogined(req.getSession())) {
            return jspView("redirect:/users/loginForm");
        }

        long questionId = Long.parseLong(req.getParameter("questionId"));
        Question question = questionDao.findById(questionId);
        if (!question.isSameUser(UserSessionUtils.getUserFromSession(req.getSession()))) {
            throw new IllegalStateException("λ€λ₯Έ μ¬μ©μκ° μ΄ κΈμ μμ ν  μ μμ΅λλ€.");
        }

        Question newQuestion = new Question(question.getWriter(), req.getParameter("title"),
                req.getParameter("contents"));
        question.update(newQuestion);
        questionDao.update(question);
        return jspView("redirect:/");
    }
}
```

> "μλ²μΈ‘μμ μμ ν μ νλ¦¬μΌμ΄μμ΄ λλλ‘ λ¨Όμ  κ΅¬νν ν ν΄λΌμ΄μΈνΈλ μΆνμ κ³ λ €ν΄λ κ΄μ°?λ€. κ·Έλ§νΌ μλ²μΈ‘μμ λ³΄μμ λν μ²λ¦¬λ₯Ό μ€μνκ² ν΄μΌ νλ€.β

π‘ 12. μ»¨νΈλ‘€λ¬μμ μ κ·Όνλ `QuestionDao` μ `AnswerDao` μ κ°μ΄ DAOμμ DB μ κ·Ό λ‘μ§μ κ΅¬νν  λ μ¬μ©νλ `JdbcTemplate` μ μΈμ€ν΄μ€λ₯Ό μ¬λ¬ κ° μμ±ν  νμμλ€. μΈμ€ν΄μ€λ₯Ό νλλ§ μμ±νλλ‘ κ΅¬ννλ€.
- μμμ μΈκΈνλ―, `Dao` , `Controller` , `JdbcTemplate` μ λ§€λ² μΈμ€ν΄μ€λ₯Ό μμ±νμ§ μμλ λλ€. **μν κ°μ κ°μ§μ§ μμΌλ©΄μ λ©μλλ§ κ°μ§λ ν΄λμ€**μ΄κΈ° λλ¬Έμ΄λ€.
- λ°λΌμ μΈμ€ν΄μ€ νλλ§ μμ±ν΄ μ¬μ¬μ©νλλ‘ ν΄μΌνλλ°, μ΄λ¬ν λμμΈ ν¨ν΄μ **μ±κΈν€(Singleton) ν¨ν΄**μ΄λΌκ³  νλ€.
    - κ΅¬νμ μν΄μλ λ¨Όμ  ν΄λμ€μ κΈ°λ³Έ μμ±μλ₯Ό `private` μ κ·Ό μ μ΄μλ‘ κ΅¬ννμ¬ ν΄λμ€ μΈλΆμμ μΈμ€ν΄μ€λ₯Ό μμ±ν  μ μλλ‘ νλ€. (`getInstance()` μ κ°μ `static` λ©μλλ₯Ό ν΅ν΄μλ§ μμ±μ΄ κ°λ₯νλλ‘ νλ€.)

```java
public class JdbcTemplate {
    private static JdbcTemplate jdbcTemplate;
    
    private JdbcTemplate() {}
    
    public static JdbcTemplate getInstance() {
        if (jdbcTemplate == null) {
            jdbcTemplate = new JdbcTemplate();
        }
        return jdbcTemplate;
    }
    ...

}
```

- μμ κ°μ΄ μ±κΈν€ ν¨ν΄μ κ΅¬ννλ κ²½μ°, λ©ν°μ€λ λ νκ²½μμ λμμ `getInstance()` λ©μλλ₯Ό νΈμΆν΄ μΈμ€ν΄μ€κ° νλ μ΄μ μμ±λλ λ¬Έμ κ° λ°μν  μ μλ€.
    - λ¬Έμ κ° μμ§λ§, λ€μμ λ€λ£° **μμ‘΄μ± μ£Όμ(DI)**μ μ€λͺνκΈ° μν΄ μμ κ°μ΄ κ΅¬ννλ€.

> **μ±κΈν€ ν¨ν΄μ μ₯λ¨μ **
> μ₯μ 
> - κ³ μ λ λ©λͺ¨λ¦¬ μμ­μ μ»μΌλ©΄μ, μΈμ€ν΄μ€λ₯Ό ν λ²λ§ μμ±νκΈ°μ λ©λͺ¨λ¦¬ λ­λΉ λ°©μ§
> - μΈμ€ν΄μ€κ° μ μ­μΌλ‘ μμ±λμ΄, λ€λ₯Έ ν΄λμ€μμμ μ κ·Ό λ° κ³΅μ κ° μ©μ΄
> - μ΄λ―Έ μμ±λμ΄ μλ μΈμ€ν΄μ€λ₯Ό μ¬μ©νμ¬ κ°μ²΄ λ‘λ© μκ°μ κ°μ
> 
> λ¨μ 
> - μ±κΈν€ μΈμ€ν΄μ€κ° λλ¬΄ λ§μ μμμ μ²λ¦¬νκ±°λ, λ§μ λ°μ΄ν°λ₯Ό κ³΅μ νλ κ²½μ° λ€λ₯Έ ν΄λμ€μ μΈμ€ν΄μ€λ€ κ°μ κ²°ν©λκ° λλ¬΄ λμμ Έ βκ°λ°© νμ μμΉβμ μλ°°
> - μ΄λ‘ μΈν΄, μμ  λ° μ μ§λ³΄μμ λΉμ©μ΄ μ¦κ°νκ³ , λ©ν°μ€λ λ νκ²½μμ λκΈ°ν μ²λ¦¬κ° μλ€λ©΄ μΈμ€ν΄μ€κ° λ μ΄μ μμ±λ  μ μλ κ°λ₯μ±μ΄ μ‘΄μ¬

```java
JdbcTemplate jdbcTemplate = JdbcTemplate.getInstance();
```

- `JdbcTemplate` μ μ¬μ©νλ `Dao` μμ μμ κ°μ΄ `JdbcTemplate.getInstance()` λ©μλλ‘ μΈμ€ν΄μ€λ₯Ό λ°μ μ¬μ©νλ€.

π‘ 13. μ§λ¬Έ μ­μ  κΈ°λ₯μ κ΅¬ννλ€. μ­μ κ° κ°λ₯ν κ²½μ°λ λ΅λ³μ΄ μλ κ²½μ°, μ§λ¬Έμμ λ΅λ³μκ° λͺ¨λ κ°μ κ²½μ°μ΄λ€. μ΄ κΈ°λ₯μ μΉκ³Ό λͺ¨λ°μΌ μ±μμ λͺ¨λ μ§μνλ €κ³  νλ€. μΉ λΈλΌμ°μ λ₯Ό ν΅ν΄ μ κ·Όνμ λλ `JspView` λ₯Ό νμ©ν΄ λͺ©λ‘ νμ΄μ§(`"redirect:/"`)λ‘ μ΄λνκ³ , λͺ¨λ°μΌ μ±μ μ§μνκΈ° μν΄ `JsonView` λ₯Ό νμ©ν΄ μλ΅ κ²°κ³Όλ₯Ό JSONμΌλ‘ μ μ‘νλ €κ³  νλ€. μ΄λ₯Ό μ§μνλ €λ©΄ λ κ°μ μ»¨νΈλ‘€λ¬κ° νμνλ€. κ° μ»¨νΈλ‘€λ¬λ₯Ό κ΅¬ννκ³ , μ€λ³΅μ μ κ±°νλ€.
- λ¨Όμ , μ­μ  λ²νΌ ν΄λ¦­ μ μμ²­μ λ³΄λΌ URLμ λͺμνκ³ , μ΄λ₯Ό μ²λ¦¬ν  μ»¨νΈλ‘€λ¬ μμ± λ° λ§€νμ μννλ€.
    - λν, `question.questionId` λ₯Ό μμ²­κ³Ό ν¨κ» μ λ¬νμ¬ μ΄λ₯Ό μ¬μ©ν  μ μλλ‘ νλ€.

```java
public class DeleteQuestionController extends AbstractController {
  private QnaService qnaService = QnaService.getInstance();

  @Override
  public ModelAndView execute(HttpServletRequest req, HttpServletResponse resp) throws Exception {
    if (!UserSessionUtils.isLogined(req.getSession())) {
      return jspView("redirect:/users/loginForm");
    }

    long questionId = Long.parseLong(req.getParameter("questionId"));
    try {
      qnaService.deleteQuestion(questionId, UserSessionUtils.getUserFromSession(req.getSession()));
      return jspView("redirect:/");
    } catch (CannotDeleteException e) {
      return jspView("show.jsp").addObject("question", qnaService.findById(questionId))
              .addObject("answers", qnaService.findAllByQuestionId(questionId))
              .addObject("errorMessage", e.getMessage());
    }
  }
}
```

```java
public class QnaService {
  private static QnaService qnaService;

  private QuestionDao questionDao = new QuestionDao();
  private AnswerDao answerDao = new AnswerDao();

	private QnaService() {}

  public static QnaService getInstance() {
    if (qnaService == null) {
      qnaService = new QnaService();
    }
    return qnaService;
  }

  public Question findById(long questionId) {
    return questionDao.findById(questionId);
  }

  public List<Answer> findAllByQuestionId(long questionId) {
    return answerDao.findAllByQuestionId(questionId);
  }

  public void deleteQuestion(long questionId, User user) throws CannotDeleteException {
    Question question = questionDao.findById(questionId);
    if (question == null) {
      throw new CannotDeleteException("μ‘΄μ¬νμ§ μλ μ§λ¬Έμλλ€.");
    }

    if (!question.isSameUser(user)) {
      throw new CannotDeleteException("λ€λ₯Έ μ¬μ©μκ° μ΄ κΈμ μ­μ ν  μ μμ΅λλ€.");
    }

    List<Answer> answers = answerDao.findAllByQuestionId(questionId);
    if (answers.isEmpty()) {
      questionDao.delete(questionId);
      return;
    }

    boolean canDelete = true;
    for (Answer answer : answers) {
      String writer = question.getWriter();
      if (!writer.equals(answer.getWriter())) {
          canDelete = false;
          break;
      }
    }

    if (!canDelete) {
      throw new CannotDeleteException("λ€λ₯Έ μ¬μ©μκ° μΆκ°ν λκΈμ΄ μ‘΄μ¬ν΄ μ­μ ν  μ μμ΅λλ€.");
    }

    questionDao.delete(questionId);
  }
}
```

- `QnaService` μλΉμ€λ₯Ό μμ±νμ¬ μ§λ¬Έκ³Ό λ΅λ³μ λν μ²λ¦¬λ₯Ό μννλ€.
- μ¬κΈ°μλ, `deleteQuestion` μμ
    - μΈμλ‘ λ°μ `questionId` μ ν΄λΉνλ μ§λ¬Έμ μ°Ύκ³ ,
    - ν΄λΉ μ§λ¬Έμ μμ±μμ μΈμλ‘ λ°μ `user` κ° λμΌνμ§ κ²μ¬ν ν
    - ν΄λΉ μ§λ¬Έμ λ΅λ³μ΄ μλμ§ μλμ§λ₯Ό νλ¨νμ¬ μλ κ²½μ°μλ§ μ§λ¬Έμ μ­μ νκ³  λ°νλλ€.
    - κ·Έλ μ§ μμ κ²½μ°, μ§λ¬Έμ λν λ΅λ³ μμ±μκ° μ¬μ©μ λ³ΈμΈμΈμ§ μλμ§ νλ¨νμ¬ `canDelete` κ°μ΄ `false` κ° λλ©΄, μΆκ°ν `CannotDeleteException` μμΈλ₯Ό λμ§λλ‘ νλ€.
- μΆκ°μ μΌλ‘, λͺ¨λ°μΌ μ§μμ μν΄ `ApiDeleteQuestionController` λ₯Ό μμ±νκ³ , λ§€ννλ€.
    - μ΄λ `jspView()` κ° μλ `jsonView()` λ₯Ό λ°ννλλ‘ νλ€.

π‘ 14. 13λ² λ¬Έμ λ₯Ό κ΅¬νν  λ λ¨μ νμ€νΈκ° κ°λ₯νλλ‘ νλ€. Daoλ₯Ό μ¬μ©νλ λͺ¨λ  μ»¨νΈλ‘€λ¬ ν΄λμ€λ DBκ° μ€μΉλμ΄ μμ΄μΌ νλ©°, νμ΄λΈκΉμ§ μμ±λμ΄ μλ μνμμλ§ νμ€νΈκ° κ°λ₯νλ€. DBκ° μ‘΄μ¬νμ§ μλ μνμμλ μ λ‘μ§μ λ¨μ νμ€νΈνλ €κ³  νλ€.
- 13λ² λ¬Έμ λ μ§λ¬Έ μ­μ  κΈ°λ₯μ μΉκ³Ό λͺ¨λ°μΌ λͺ¨λμ μ κ³΅νκΈ° μν΄ 2κ°μ `Controller` κ΅¬ν μ λ°μνλ μ€λ³΅μ μ΄λ»κ² μ κ±°ν  κ²μΈκ°μ λν λ¬Έμ μ΄λ€.
    - μ΄ μκ΅¬μ¬ν­μ λ§μ‘±νλλ‘ `DeleteQuestionController` , `ApiDeleteQuestionController` ν΄λμ€λ₯Ό κ΅¬ννλ€.
    - μλ κ΅¬νλλ‘λΌλ©΄, `questionId` λ₯Ό κ°μ Έμ μ§λ¬Έμ λ¨Όμ  μ‘°ννκ³ , μμ±μμ μμ νλ €λ μ¬μ©μκ° λμΌνμ§ κ²μ¬νκ³ , ν΄λΉ μ§λ¬Έμ λν λ΅λ³μ μ‘°νν λ€, λ΅λ³μ μμ±μκΉμ§ λ€ νμΈν΄μΌνλ κ³Όμ  λͺ¨λ λμΌνκ³ , μλ΅ν  λ·°(`JspView` , `JsonView`)λ§ λ€λ₯΄λ€.
    - μ΄μ λν μ€λ³΅μ μ κ±°ν΄μΌ νλ€.

λ°©λ²μ 2κ°μ§κ° μλ€.

- μ΄ λ ν΄λμ€μ λν λΆλͺ¨ ν΄λμ€λ₯Ό μΆκ°ν΄ μ€λ³΅ λ‘μ§μ λΆλͺ¨ ν΄λμ€λ‘ μ΄λν ν μμμ ν΅ν΄ μ€λ³΅μ μ κ±°νλ€.
- μ»¨νΈλ‘€λ¬κ° DAOμ μμ‘΄κ΄κ³λ₯Ό ν΅ν΄ DB μ κ·Ό λ‘μ§μ μ κ±°νλ― μλ‘μ΄ ν΄λμ€λ₯Ό μΆκ°ν΄ λ‘μ§ μ²λ¦¬λ₯Ό μμνμ¬ μ€λ³΅μ μ κ±°νλ€. β **"μ‘°ν©"(composition)**
    - μμμ κ²½μ° **λΆλͺ¨ ν΄λμ€μ λ³κ²½μ΄ λ°μνλ©΄ μμ ν΄λμ€μλ μν₯**μ΄ μκΈ°κΈ° λλ¬Έμ μμλ³΄λ€λ μ‘°ν©μ ν΅ν΄ μ€λ³΅μ μ κ±°ν  κ²μ μΆμ²νλ€.

μλ° μ§μμμλ μ΄λ¬ν μ€λ³΅ μ κ±°μ μ»¨νΈλ‘€λ¬ μ­ν  λΆλ¦¬ λ±μ λͺ©μ μΌλ‘ **Service**(λλ Manager)λΌλ ν΄λμ€λ₯Ό μΆκ°ν΄ λ΄λΉνλλ‘ κ΅¬ννλ€. λ°λΌμ μ λ¬Έμ μμλ `QnaService` λΌλ ν΄λμ€λ₯Ό μΆκ°νλ€.

```java
public class QnaService {
  private static QnaService qnaService;

  private QuestionDao questionDao = new QuestionDao();
  private AnswerDao answerDao = new AnswerDao();

  private QnaService() {}

  public static QnaService getInstance() {
    if (qnaService == null) {
      qnaService = new QnaService();
    }
    return qnaService;
  }

  public Question findById(long questionId) {
    return questionDao.findById(questionId);
  }

  public List<Answer> findAllByQuestionId(long questionId) {
    return answerDao.findAllByQuestionId(questionId);
  }

  public void deleteQuestion(long questionId, User user) throws CannotDeleteException {
    Question question = questionDao.findById(questionId);
    if (question == null) {
      throw new CannotDeleteException("μ‘΄μ¬νμ§ μλ μ§λ¬Έμλλ€.");
    }

    if (!question.isSameUser(user)) {
      throw new CannotDeleteException("λ€λ₯Έ μ¬μ©μκ° μ΄ κΈμ μ­μ ν  μ μμ΅λλ€.");
    }

    List<Answer> answers = answerDao.findAllByQuestionId(questionId);
    if (answers.isEmpty()) {
      questionDao.delete(questionId);
      return;
    }

    boolean canDelete = true;
    for (Answer answer : answers) {
      String writer = question.getWriter();
      if (!writer.equals(answer.getWriter())) {
          canDelete = false;
          break;
      }
    }

    if (!canDelete) {
      throw new CannotDeleteException("λ€λ₯Έ μ¬μ©μκ° μΆκ°ν λκΈμ΄ μ‘΄μ¬ν΄ μ­μ ν  μ μμ΅λλ€.");
    }

    questionDao.delete(questionId);
  }
}
```

- μ΄ ν΄λμ€ λν μν κ°μ κ°μ§μ§ μμ **μ±κΈν€ ν¨ν΄**μΌλ‘ κ΅¬ννλ€.
- `deleteQuestion()` κ° `DeleteQuestionController` μ `ApiDeleteQuestionController` ν΄λμ€μμμ μ€λ³΅ μ½λλ₯Ό κ΅¬νν λ‘μ§μ΄λ€.
    - μ΄λ κ² μ»¨νΈλ‘€λ¬μμμ λ³΅μ‘νκ³  μ€λ³΅λ λ‘μ§μ μλΉμ€λ‘ μμνκΈ°μ, μ»¨νΈλ‘€λ¬μμλ μ μμ μΌλ‘ μ­μ κ° λλ κ²½μ°μ μλ¬κ° λ°μνλ κ²½μ°μ λ°λ₯Έ μ²λ¦¬λ§ κ΅¬νν  μ μκ² λλ€.
    - μ΄λ, `CannotDeleteException` μ΄λΌλ μ»΄νμΌνμ Exceptionμ `throw` νλ€.
        - μ΄μ κ°μ΄ μ¬μ©μμκ² μμΈμ²λ¦¬λ₯Ό ν΅ν΄ μλ¬ λ©μΈμ§λ₯Ό μ λ¬νκ±°λ, λ€λ₯Έ μμμ νλλ‘ μ λν  νμκ° μλ κ²½μ° λ°νμ Exceptionλ³΄λ€ μ»΄νμΌνμ Exceptionμ΄ μ ν©νλ€.

> **κ³μΈ΅ν μν€νμ²**
> μ»¨νΈλ‘€λ¬, μλΉμ€, DAO κ΅¬μ‘°λ‘ μΉ μ νλ¦¬μΌμ΄μμ κ°λ°νλ κ²
> <img src="https://user-images.githubusercontent.com/33208303/158941304-9bc2d1cb-4178-41fa-b0e6-911bedbb6ccd.png" width="50%">
> 
> μ§λ¬Έ μ­μ λ₯Ό λ΄λΉνλ ν΄λμ€λ€μ **κ³μΈ΅ν μν€νμ²**λ‘ λνλ΄λ©΄ λ€μκ³Ό κ°λ€.
> 
> <img src="https://user-images.githubusercontent.com/33208303/158941301-a52cd31f-7951-4e7a-91c1-37b5a2b198b1.png" width="50%">
> 
> μ΅κ·Όμλ DB μ κ·Ό λ‘μ§μ λ΄λΉνλ ν΄λμ€ μ΄λ¦μ DAO λμ  Repositoryλ‘ κ΅¬ννλ κ²½μ°κ° λ§μμ§κ³  μλ€. Domain κ°μ²΄μ ν΄λΉνλ ν΄λμ€λ Question & Answerμ΄λ€.

π‘ 15. `RequestMapping` μ½λλ₯Ό λ³΄λ©΄, μ»¨νΈλ‘€λ¬ μΆκ°λ§λ€ μμ²­ URLκ³Ό μ»¨νΈλ‘€λ¬λ₯Ό μΆκ°ν΄μΌ νλ λΆνΈν¨μ΄ μλ€. μλΈλ¦Ώκ³Ό κ°μ΄ annotationμ νμ©ν΄ μ€μ μ μΆκ°νκ³  μλ²κ° μμν  λ μλμΌλ‘ λ§€νμ΄ λλλ‘ κ°μ νλ€.
- `@Controller` μ€μ μ ν΅ν μλ‘μ΄ MVCλ λ€μ μ₯μμ κ΅¬ννλ€.