# π­Β μμ‘΄κ΄κ³ μ£Όμ `DI` λ₯Ό ν΅ν νμ€νΈνκΈ° μ¬μ΄ μ½λ λ§λ€κΈ°

## βΒ μ `DI` κ° νμνκ°?

- κ°μ²΄μκ² **μμ‘΄κ΄κ³**
    - κ°μ²΄ νΌμ λͺ¨λ  μΌμ μ²λ¦¬νκΈ° νλ€κΈ° λλ¬Έμ λ΄κ° ν΄μΌ ν  μμμ λ€λ₯Έ κ°μ²΄μκ² μμνλ©΄μ λ°μ
    - κ°μ²΄ μμ μ΄ κ°μ§κ³  μλ μ±μκ³Ό μ­ν μ λ€λ₯Έ κ°μ²΄μκ² μμνλ μκ° λ°μ
- DI λ κ°μ²΄ κ°μ μμ‘΄κ΄κ³λ₯Ό μ΄λ»κ² ν΄κ²°νλλμ λ°λ₯Έ μλ‘μ΄ μ κ·Ό λ°©μ

### Example

```java
import java.util.Calendar;

public class DateMessageProvider {
    public String getDateMessage() {
				Calendar now = Calendar.getInstance();
				int hour = now.get(Calendar.HOUR_OF_DAY);
				
				if(hour < 12)
						return "μ€μ ";
				return "μ€ν";
		}
}
```

```java
public class DateMessageProviderTest {
		@Test
		public void μ€μ () throws Exception {
				DateMessageProvider provider = new DateMessageProvider();
				assertEquals("μ€μ ", provider.getDateMessage());
		}

		@Test
		public void μ€ν() throws Exception {
				DateMessageProvider provider = new DateMessageProvider();
				assertEquals("μ€ν", provider.getDateMessage());
		}
}
```

- νμ€νΈ λ©μλμ€ λ μ€μ νλλ **λ°λμ μ€ν¨**
- νμ€νΈκ° λͺ¨λ μ±κ³΅ν  μμλ μ΄μ λ `DateMessageProvider` κ° `Calendar` μ μμ‘΄κ΄κ³λ₯Ό κ°μ§λλ° νμ€νΈλ₯Ό μν΄ `Calendar`μ μκ°μ λ³κ²½ν  μ μλ λ°©λ²μ΄ μλ€
- μ΄μ κ°μ μμ‘΄κ΄κ³λ₯Ό κ°νκ² κ²°ν©(tightly coupling)λμ΄ μλ€κ³  νλ€
- μ¦, `Calnder` μΈμ€ν΄μ€μ λν μμ±μ `getDateMessage` κ° κ²°μ νλ κ²μ΄ μλλΌ `DateMessageProvider` μΈλΆμμ `Calender` μΈμ€ν΄μ€λ₯Ό μμ±ν ν μ λ¬νλ κ΅¬μ‘°λ‘ λ°κΏμΌ νλ€

### getDateMessage() λ©μλ μΈμλ‘ μ λ¬νλ λ°©λ²μΌλ‘ λ¦¬ν©ν λ§

```java
import java.util.Calendar;

public class DateMessageProvider {
    public String getDateMessage(Calendar now) {
				int hour = now.get(Calendar.HOUR_OF_DAY);
				
				if(hour < 12)
						return "μ€μ ";
				return "μ€ν";
		}
}
```

```java
public class DateMessageProviderTest {
		@Test
		public void μ€μ () throws Exception {
				DateMessageProvider provider = new DateMessageProvider();
				assertEquals("μ€μ ", provider.getDateMessage(createCurrentDate(11)));
		}

		@Test
		public void μ€ν() throws Exception {
				DateMessageProvider provider = new DateMessageProvider();
				assertEquals("μ€ν", provider.getDateMessage(createCurrentDate(13)));
		}

		private Calendar createCurrentDate(int hourOfDay) {
				Calendar now = Calendar.getInstatnce();
				now.set(Calendar.HOUR_OF_DAY, hourOfDay);
				return now;
		}
}
```

- μ΄μ²λΌ κ°μ²΄ κ°μ μμ‘΄κ΄κ³μ λν κ²°μ κΆμ `μμ‘΄κ΄κ³λ₯Ό κ°μ§λ κ°μ²΄` κ° κ°μ§λ κ²μ΄ μλλΌ `μΈλΆμ λκ΅°κ°`κ° λ΄λΉνλλ‘ λ§‘κ²¨ λ²λ¦ΌμΌλ‘μ¨ μ’ λ μ μ°ν κ΅¬μ‘°λ‘ κ°λ°νλ κ²μ `DI` λΌ νλ€
- μΌλ°μ μΌλ‘ μ μ°ν κ΅¬μ‘°μ μ νλ¦¬μΌμ΄μμ λ³νλ₯Ό μ΅μννλ©΄μ νμ₯νκΈ°λ μ½κ³ , νμ€νΈνκΈ°λ μ½λ€λκ²μ μλ―Έ

## π€¬Β `DI` λ₯Ό μ μ©νλ©΄μ μμ΄λ λΆνΈν¨

```java
public class QnaService {
    private static QnaService qnaService;

    private QuestionDao questionDao = QuestionDao.getInstance();
    private AnswerDao answerDao = AnswerDao.getInstance();

    private QnaService() {
    }

    public static QnaService getInstance() {
        if (qnaService == null) {
            qnaService = new QnaService();
        }
        return qnaService;
    }

		[...]
}
```

β `DI` κ΅¬μ‘°λ‘ λ³κ²½

```java
public class QnaService {
    private static QnaService qnaService;

    private QuestionDao questionDao;
    private AnswerDao answerDao;

    private QnaService(QuestionDao questionDao, AnswerDao answerDao) {
        this.questionDao = questionDao;
        this.answerDao = answerDao;
    }

    public static QnaService getInstance(QuestionDao questionDao, AnswerDao answerDao) {
        if (qnaService == null) {
            qnaService = new QnaService(questionDao, answerDao);
        }
        return qnaService;
    }

		[...]
}
```

β `DeleteQuestionController`, `ApiDeleteQuestionController` μμ μ»΄νμΌ μλ¬κ° λ°μν΄ μμ 

```java
public class DeleteQuestionController extends AbstractController {
    private QnaService qnaService = QnaService.getInstance(QuestionDao.getInstance(), AnswerDao.getInstance());
		[...]
}

public class ApiDeleteQuestionController extends AbstractController {
    private QnaService qnaService = QnaService.getInstance(QuestionDao.getInstance(), AnswerDao.getInstance());
		[...]
}
```

- `QuestionDao` μ `AnswerDao` κ° λ°μ΄ν°λ² μ΄μ€μ μμ‘΄κ΄κ³
    
    β μμ‘΄κ΄κ³λ₯Ό κ°μ§μ§ μλλ‘ λ³κ²½ν  μ μμ΄μΌ λ°μ΄ν°λ² μ΄μ€μ μμ‘΄νμ§ μλ νμ€νΈκ° κ°λ₯
    

- DIλ₯Ό μ μ©ν΄ μ μ°ν¨μ μ»μ μ μμμ§ λͺ¨λ₯΄κ² μ§λ§ μΆκ° μμν  λΆλΆμ΄ λλ¬΄ λ§λ€λ λλ
    
    β *DI λ μ°λ κΈ°μΈκ°?*
    

- DI μ μ μ°ν¨μ κ°μ΄μΌλ‘ λλΌλ €λ©΄ κ°μ μλΉμ€λ₯Ό μΌμ  κΈ°κ° μ μ§λ³΄μ νλ©΄μ μ¬μ©μμ μκ΅¬μ¬ν­μ΄ λ°λκ³ , μλ‘μ΄ κΈ°λ₯μ μΆκ°νλ κ²½νμ ν΄λ΄μΌ ν¨

- νμ€νΈνκΈ° μ¬μ΄μ½λλ₯Ό λ§λ€κΈ° β DI λ₯Ό μ μ©νμ λ κ°λ₯

## βΊοΈΒ λΆλ§ ν΄μνκΈ°

1. μ±κΈν€ ν¨ν΄μ μ¬μ©ν¨μΌλ‘ μΈν΄ νμ€νΈμ μ΄λ €μμ΄ μλ€
2. νμ€νΈλ₯Ό μν΄ λ§€λ² `Mock` κ°μ²΄λ₯Ό λ§λλ κ²μ λ§μ λΉμ©μ΄ λ€κ³  κ·μ°?μ μμμ΄λ€

### μ±κΈν€ ν¨ν΄μ μ κ±°ν DI

- μ±κΈν€ ν¨ν΄μ λμμΈ ν¨ν΄ μ€ μ΄ν΄νκΈ°λ κ°μ₯ μ½κΈ° λλ¬Έμ λλ¦¬ μ¬μ©λ¨
- νμ§λ§ μ±κΈν€ ν¨ν΄μ μ¬μ©ν  κ²½μ° κ·Έμ λ°λ₯Έ λ¨μ λ λ§μ
    - ν΄λΉ ν΄λμ€μ κ°ν μμ‘΄κ΄κ³λ₯Ό κ°μ§κΈ° λλ¬Έμ νμ€νΈνκΈ°κ° μ΄λ €μ
    - `private` μΌλ‘ κ΅¬ννκΈ° λλ¬Έμ μμν  μ μλ€λ λ¨μ 
    - κ°μ²΄ μ§ν₯ μ€κ³ μμΉμ λ°λΌ κ°λ°νλ κ²μ μ ν΄νλ μμΈ
    
π‘ μ±κΈν€ ν¨ν΄μ μ¬μ©νμ§ μμΌλ©΄μ μΈμ€ν΄μ€ νλλ§ μ μ§ν  μ μλ λ€λ₯Έ ν΄κ²°μ±!  


μ»¨νΈλ‘€λ¬λ μ±κΈν€ ν¨ν΄μ μ μ©νμ§ μκ³ λ κ°μ ν¨κ³Ό 

 β μλΈλ¦Ώ μ»¨νμ΄λκ° `DispatcherServlet` μ μ΄κΈ°ν νλ μμ μ μ»¨νΈλ‘€λ¬ μΈμ€ν΄μ€λ₯Ό μμ±ν ν μ¬μ¬μ© κ°λ₯νλλ‘ κ΅¬ννκΈ° λλ¬Έ

### QnaService μ QuestionDao μ AnswerDaoλ₯Ό μμ λ°©λ²μΌλ‘ κ΅¬ννκ³  νμ€νΈ

- μ±κΈ ν€ ν¨ν΄μ μν μ½λλ₯Ό λͺ¨λ μ κ±°νκ³  μμ±μλ₯Ό `public` μΌλ‘ λ³κ²½

```java
public class QnaService {
    private QuestionDao questionDao;
    private AnswerDao answerDao;

    public QnaService(QuestionDao questionDao, AnswerDao answerDao) {
        this.questionDao = questionDao;
        this.answerDao = answerDao;
    }
```

- `QnaService`μ μμ‘΄κ΄κ³μ μλ `DeleteQuestionController` μ `ApiDeleteQuestionController`λ₯Ό μμ 

```java
public class DeleteQuestionController extends AbstractController {
    private QnaService qnaService;

    public DeleteQuestionController(QnaService qnaService) {
        this.qnaService = qnaService;
    }
}

public class ApiDeleteQuestionController extends AbstractController {
    private QnaService qnaService;

    public ApiDeleteQuestionController(QnaService qnaService) {
        this.qnaService = qnaService;
    }
}
```

- μ λ κ°μ μ»¨νΈλ‘€λ¬λ₯Ό μμ±ν  λ QnaServiceλ₯Ό DI λ‘ μ λ¬νλλ‘ μμ 

```java
public class LegacyHandlerMapping implements HandlerMapping {
    private Map<String, Controller> mappings = new HashMap<>();
		
		public void initMapping() {
        QnaService qnaService = new QnaService(JdbcQuestionDao.getInstance(), JdbcAnswerDao.getInstance());
        mappings.put("/qna/delete", new DeleteQuestionController(qnaService));
        mappings.put("/api/qna/deleteQuestion", new ApiDeleteQuestionController(qnaService));
    }
}
```

- JdbcQuestionDao, JdbcAnswerDao λ μμ§λ μ±κΈν€ ν¨ν΄μ μ¬μ©νλ―λ‘ μ μ©νμ§ μλλ‘ μμ νκ³  λ€λ₯Έ μ»¨νΈλ‘€λ¬λ€μλ μ»΄νμΌ μλ¬κ° λ°μνλ λΆλΆμ λͺ¨λ μ°Ύμ DIλ‘ μμ‘΄κ΄κ³λ₯Ό κ°μ§λλ‘ μμ 

```java
public class LegacyHandlerMapping implements HandlerMapping {
    private static final Logger logger = LoggerFactory.getLogger(DispatcherServlet.class);
    private Map<String, Controller> mappings = new HashMap<>();

    public void initMapping() {
        QuestionDao questionDao = new JdbcQuestionDao();
        AnswerDao answerDao = new JdbcAnswerDao();
        QnaService qnaService = new QnaService(questionDao, answerDao);

        mappings.put("/", new HomeController(questionDao));
        mappings.put("/qna/show", new ShowQuestionController(questionDao, answerDao));
        mappings.put("/qna/form", new CreateFormQuestionController());
        mappings.put("/qna/create", new CreateQuestionController(questionDao));
        mappings.put("/qna/updateForm", new UpdateFormQuestionController(questionDao));
        mappings.put("/qna/update", new UpdateQuestionController(questionDao));
        mappings.put("/qna/delete", new DeleteQuestionController(qnaService));
        mappings.put("/api/qna/deleteQuestion", new ApiDeleteQuestionController(qnaService));
        mappings.put("/api/qna/list", new ApiListQuestionController(questionDao));
        mappings.put("/api/qna/addAnswer", new AddAnswerController(questionDao, answerDao));
        mappings.put("/api/qna/deleteAnswer", new DeleteAnswerController(answerDao));

        logger.info("Initialized Request Mapping!");
    }
}
```

- MVC νλ μμν¬λ₯Ό μ μ©ν μ¬μ©μ κ΄λ¦¬κΈ°λ₯μ DI κ΅¬μ‘°λ‘ λ³κ²½
    
    β Ah! μλ° λ¦¬νλ μμ νμ©ν΄ μΈμ€ν΄μ€λ₯Ό μλμΌλ‘ μμ±νλ λ°©μμ΄λ€ λ³΄λ μΈμ€ν΄μ€λ₯Ό μμ±ν  λμ μμ‘΄κ΄κ³μ μλ μΈμ€ν΄μ€λ₯Ό μ λ¬ν  λ°©λ²μ΄ μλ€!
    
    β λ€μ μ !
    

### Mockitoλ₯Ό νμ©ν νμ€νΈ

- νμ€νΈ μ½λλ₯Ό κ΅¬ννλ κ²μ ν° λΆλ΄μΌλ‘ μκ°νλ κ°λ°μκ° λ§μλ° `Mock(κ°μ§)` ν΄λμ€κΉμ§ κ΅¬νν΄μΌ νλ€λ©΄ ν° λΆλ΄ β κ²°κ³Όμ μΌλ‘ νμ€νΈλ₯Ό νμ§ μκ²λ¨
- Mock ν΄λμ€λ₯Ό κ΅¬ννμ§ μμλ νμ€νΈκ° κ°λ₯νλλ‘ μ§μνλ Mock νμ€νΈ νλ μ μν¬κ° μλ€?!?!
- jMock, EasyMock, Mockito

- λ©μ΄λΈ μ€μ νμΌ pom.xml μ Mockito μ λν μμ‘΄κ΄κ³λ₯Ό μΆκ°

```xml
<dependency>
			<groupId>org.mockito</groupId>
			<artifactId>mockito-core</artifactId>
			<version>1.10.19</version>
			<scope>test</scope>
		</dependency>
```

- Mockitoλ₯Ό νμ©ν΄ QnaServiceμ deleteQuestion() λ©μλλ₯Ό νμ€νΈ

```java
@RunWith(MockitoJUnitRunner.class)
public class QnaServiceTest {
    @Mock
    private QuestionDao questionDao;
    @Mock
    private AnswerDao answerDao;

    private QnaService qnaService;

    @Before
    public void setup() {
        qnaService = new QnaService(questionDao, answerDao);
    }

    @Test(expected = CannotDeleteException.class)
    public void deleteQuestion_μλ_μ§λ¬Έ() throws Exception {
        when(questionDao.findById(1L)).thenReturn(null);

        qnaService.deleteQuestion(1L, newUser("userId"));
    }

[...]

}
```

- Mokitoλ `@Mock` μ΄λΈνμ΄μμΌλ‘ μ€μ ν ν΄λμ€μ λ©μλλ₯Ό νΈμΆνμ λ λ°νκ°μ μ§μ ν  μ μμ
- `@Mock` μΌλ‘ μ€μ ν ν΄λμ€μ λ©μλκ° νΈμΆλλμ§μ μ¬λΆλ₯Ό verify() λ©μλλ₯Ό ν΅ν΄ κ²μ¦νλ μμ λν κ°λ₯

### DI λ³΄λ€ μ°μ νλ κ°μ²΄μ§ν₯ κ°λ°

- νμ¬ μκ°μ λ°λΌ μ€μ /μ€νλ₯Ό μΆλ ₯νλ μ λ§ κ°λ¨ν λ‘μ§μ νμ€νΈνκΈ° μν μ½λ

```java
public class DateMessageProviderTest {
		@Test
		public void μ€μ () throws Exception {
				DateMessageProvider provider = new DateMessageProvider();
				assertEquals("μ€μ ", provider.getDateMessage(createCurrentDate(11)));
		}

		@Test
		public void μ€ν() throws Exception {
				DateMessageProvider provider = new DateMessageProvider();
				assertEquals("μ€ν", provider.getDateMessage(createCurrentDate(13)));
		}

		private Calendar createCurrentDate(int hourOfDay) {
				Calendar now = Calendar.getInstatnce();
				now.set(Calendar.HOUR_OF_DAY, hourOfDay);
				return now;
		}
}
```

<aside>
π‘ μ’ λ κ°μ²΄ μ§ν₯μ μΈ κ°λ°

</aside>

- `Hour` ν΄λμ€λ₯Ό μΆκ°

```java
public class Hour {
		private int hour;
			
		public Hour(int hour) {
				this.hour = hour;
		}

		public String getMessage() {
				if (hour < 12) {
						return "μ€μ ";
				}
				return "μ€ν";
		}
		
		@Override
		public boolean equals(Object obj) {
				if (this == obj)
						return true;
				if(obj == null)
						return false;
				if(getClass() != obj.getClass())
						return false;
				Hour other = (Hour)obj;
				if(hour != other.hour)
						return false;
				return true;
		}
}				
```

- μμ λν νμ€νΈ

```java
public class DateMessageProviderTest {
		@Test
		public void μ€μ () throws Exception {
				Hour hour = new Hour(11)
				assertEquals("μ€μ ", hour.getMessage());
		}

		@Test
		public void μ€ν() throws Exception {
				Hour hour = new Hour(16)
				assertEquals("μ€ν", hour.getMessage());
		}
}
```

<aside>
π‘ λ‘μ§ μ²λ¦¬λ₯Ό λ΄λΉνλ μλ‘μ΄ κ°μ²΄λ₯Ό μΆμΆν¨μΌλ‘μ¨ μ­ν μ λΆλ¦¬ν  μ μμΌλ©°, νμ€νΈ λν λ μ½κ²ν  μ μλ€

</aside>

- βκ³μΈ΅ν μν€νμ² κ΄μ κ³Ό κ°μ²΄μ§ν₯ μ€κ³ κ΄μ μμ ν΅μ¬μ μΈ λΉμ¦λμ€ λ‘μ§μ κ΅¬νν΄μΌ νλ μ­ν μ λκ° λ΄λΉν΄μΌ ν κΉ?β
    - `Service`μμ μ²λ¦¬ν΄μΌνλ? β μ΄λ μλΉμ€ λ μ΄μ΄μ μ­ν μ λ§μ§ μλ€
    - ν΅μ¬μ μΈ λΉμ¦λμ€ λ‘μ§ κ΅¬νμ λλ©μΈ κ°μ²΄κ° λ΄λΉνλ κ²μ΄ λ§λ€
    - μλΉμ€ λ μ΄μ΄μ ν΅μ¬μ μΈ μ­ν μ λλ©μΈ κ°μ²΄λ€μ΄ λΉμ§λμ€ λ‘μ§μ κ΅¬νν  μ μλλ‘ λλ©μΈ κ°μ²΄λ₯Ό μ‘°ν©νκ±°λ, λ‘μ§μ²λ¦¬λ₯Ό μλ£νμ λμ μνκ°μ DAOλ₯Ό νμ©ν΄ λ°μ΄ν°λ² μ΄μ€μ μκ΅¬ μ μ₯νλ λ±μ μ­ν μ λ΄λΉν΄μΌ νλ€

- μ μ°¨μ§ν₯μ μΌλ‘ κ°λ°νλ©΄ μλΉμ€ λ μ΄μ΄μ λ³΅μ‘λλ μ μ  λ μ¦κ°νλ©΄μ μ μ§λ³΄μμ νμ€νΈνκΈ° νλ  μν©μ΄ λ°μνλ€
- ν΅μ¬ κ°μ²΄λΌ ν  μ μλ `λλ©μΈ κ°μ²΄`λ μ¬μ©μκ° μλ ₯ν λ°μ΄ν°λ₯Ό DAO μ μ λ¬νκ±°λ λ°μ΄ν°λ² μ΄μ€ λ°μ΄ν°λ₯Ό λ·°μ μ λ¬νλ μ­ν  λ°μ νμ§ μκ² λλ€
- μ§κΈλΆν°λΌλ `λλ©μΈ κ°μ²΄`μ λ λ§μ μΌμ μμΌλ³΄μ!

- `Question` μ μμ μ΄ λͺ¨λ  λ‘μ§μ μ²λ¦¬νμ§ μκ³  μΈμλ‘ μ λ¬λ User, Answer μ νλ ₯ν΄ μ­μ  κ°λ₯ μ¬λΆλ₯Ό νλ¨νλ€

- `User`, `Answer` μ κ΅¬νμ½λ

```java
public class User {
		private String userId;

		[...]

		public boolean isSameUser(String newUserId) {
				return userId.equals(newUserId);
		}
}
```

```java
public class Answer {
		private String writer;
		
		[...]

		public boolean canDelete(User user) {
				return user.isSameUser(this.writer);
		}
}
```

- deleteQuestion() λ©μλμ λ‘μ§μ΄ Question, Answer, User λ‘ λΆμ°λ κ²°κ³Ό μ½λμ λ³΅μ‘λλ λ§μ΄ λ?μμ‘λ€

- deleteQuestion() λ©μλλ₯Ό μμ 

```java
public void deleteQuestion(long questionId, User user) throws CannotDeleteException {
        Question question = questionDao.findById(questionId);
        if (question == null) {
            throw new CannotDeleteException("μ‘΄μ¬νμ§ μλ μ§λ¬Έμλλ€.");
        }

        List<Answer> answers = answerDao.findAllByQuestionId(questionId);
        if (question.canDelete(user, answers)) {
            questionDao.delete(questionId);
        }
    }
```

- κ°μ²΄ μ§ν₯μΌλ‘ κ°λ°νμ λ μ»μ μ μλ μ₯μ  μ€μ νλλ νμ€νΈ νκΈ° μ½λ€λ κ²μ΄λ€

- Question μ canDelete() λ©μλμ λν νμ€νΈ μ½λ

```java
public class QuestionTest {
    public static Question newQuestion(String writer) {
        return new Question(1L, writer, "title", "contents", new Date(), 0);
    }

    public static Question newQuestion(long questionId, String writer) {
        return new Question(questionId, writer, "title", "contents", new Date(), 0);
    }

    @Test(expected = CannotDeleteException.class)
    public void canDelete_κΈμ΄μ΄_λ€λ₯΄λ€() throws Exception {
        User user = newUser("javajigi");
        Question question = newQuestion("sanjigi");
        question.canDelete(user, new ArrayList<Answer>());
    }

    @Test
    public void canDelete_κΈμ΄μ΄_κ°μ_λ΅λ³_μμ() throws Exception {
        User user = newUser("javajigi");
        Question question = newQuestion("javajigi");
        assertTrue(question.canDelete(user, new ArrayList<Answer>()));
    }

    @Test
    public void canDelete_κ°μ_μ¬μ©μ_λ΅λ³() throws Exception {
        String userId = "javajigi";
        User user = newUser(userId);
        Question question = newQuestion(userId);
        List<Answer> answers = Arrays.asList(newAnswer(userId));
        assertTrue(question.canDelete(user, answers));
    }

    @Test(expected = CannotDeleteException.class)
    public void canDelete_λ€λ₯Έ_μ¬μ©μ_λ΅λ³() throws Exception {
        String userId = "javajigi";
        List<Answer> answers = Arrays.asList(newAnswer(userId), newAnswer("sanjigi"));
        newQuestion(userId).canDelete(newUser(userId), answers);
    }
}
```

- μμ‘΄κ΄κ³κ° μκΈ° λλ¬Έμ κ΅³μ΄ Mockitoμ κ°μ Mock νλ μμν¬λ₯Ό μ¬μ©ν  νμλ μμΌλ©° νμ€νΈ κ΅¬νλν Mockitoλ₯Ό μ¬μ©ν  λ λ³΄λ€ κ°λ¨νλ€
- ν΅μ¬λ‘μ§μ λλ©μΈ κ°μ²΄μ κ΅¬νν¨μΌλ‘μ¨ λ‘μ§ κ΅¬νμ λ³΅μ‘λλ₯Ό λ?μΆκ³ , νμ€νΈνκΈ° μ¬μ΄ μ½λλ‘ κ΅¬νν  μμλ€
- νμ€νΈκ° μ΅κ΄ν λμ΄μμ§ μλ€λ©΄ λ€λ₯Έ λΆλΆμ νμ§μλλΌλ λλ©μΈ κ°μ²΄μ λν νμ€νΈ λ§μ΄λΌλ μ§νν΄μΌ νλ€!
- κ°μ₯ μ€μν λ‘μ§μ ν¬ν¨νκ³  μκΈ° λλ¬Έμ μ΄λΆλΆμ λν λ‘μ§λ§μ΄λΌλ νμ€νΈνλ€λ©΄ μΆ©λΆν ν¨κ³Όλ₯Ό λ³Ό μ μλ€
- λλ©μΈ κ°μ²΄λ₯Ό νμ€νΈνκΈ° μ¬μ΄ μ½λλ₯Ό λ§λ€λ €κ³  κ³ λ―Όνλ€ λ³΄λ©΄ μμ°μ€λ½κ² μ’ λ κ°μ²΄μ§ν₯μ μΈ μ½λλ₯Ό κ΅¬νν  μ μκ² λλ€

- μλ‘ κ΅¬νν QnaServiceTest

```java
@RunWith(MockitoJUnitRunner.class)
public class QnaServiceTest {
    @Mock
    private QuestionDao questionDao;
    @Mock
    private AnswerDao answerDao;

    private QnaService qnaService;

    @Before
    public void setup() {
        qnaService = new QnaService(questionDao, answerDao);
    }

    @Test(expected = CannotDeleteException.class)
    public void deleteQuestion_μλ_μ§λ¬Έ() throws Exception {
        when(questionDao.findById(1L)).thenReturn(null);

        qnaService.deleteQuestion(1L, newUser("userId"));
    }

    @Test
    public void deleteQuestion_μ­μ ν μ_μμ() throws Exception {
        User user = newUser("userId");
        Question question = new Question(1L, user.getUserId(), "title", "contents", new Date(), 0) {
            public boolean canDelete(User user, List<Answer> answers) throws CannotDeleteException {
                return true;
            };
        };
        when(questionDao.findById(1L)).thenReturn(question);

        qnaService.deleteQuestion(1L, newUser("userId"));
        verify(questionDao).delete(question.getQuestionId());
    }

    @Test(expected = CannotDeleteException.class)
    public void deleteQuestion_μ­μ ν μ_μμ() throws Exception {
        User user = newUser("userId");
        Question question = new Question(1L, user.getUserId(), "title", "contents", new Date(), 0) {
            public boolean canDelete(User user, List<Answer> answers) throws CannotDeleteException {
                throw new CannotDeleteException("μ­μ ν  μ μμ");
            };
        };
        when(questionDao.findById(1L)).thenReturn(question);

        qnaService.deleteQuestion(1L, newUser("userId"));
    }
}
```

- κ΄κ³ν λ°μ΄ν° λ² μ΄μ€λ₯Ό μ¬μ©νλ©΄μ μ’λ κ°μ²΄μ§ν₯μ μΈ κ°λ°μ΄ κ°λ₯νλλ‘ νλ €λ©΄ ORM νλ μμν¬λ₯Ό μ¬μ©ν  νμκ° μλ€
- DI λ₯Ό μ μ©νλ κ²λ μ€μνμ§λ§ λ¨Όμ  κ°μ²΄μ§ν₯μ μΈ κ°λ°μ νλλ‘ μ°μ΅ν΄μΌ νλ€

## π§ͺΒ DI νλ μμν¬ μ€μ΅

### μκ΅¬μ¬ν­

- κ° ν΄λμ€μ λν μΈμ€ν΄μ€ μμ± λ° μμ‘΄κ΄κ³ μ€μ μ μ΄λΈνμ΄μμΌλ‘ μλν
    - μ΄λ―Έ μΆκ°λμ΄μλ @Controller, μλΉμ€λ @Service, DAO λ @Repository
    - 3κ°μ μ€μ μΌλ‘ μμ±λ κ° μΈμ€ν΄μ€ κ°μ μμ‘΄κ΄κ³λ @Injection μ΄λΈνμ΄μμ μ¬μ©. 
    
π‘ `λΉ`μ΄λΌλ μ©μ΄κ° λ±μ₯νλ©΄ @Controller, @Service, @Repository μ€μ μ μν΄ DI νλ μμν¬κ° μλμΌλ‘ μμ±ν μΈμ€ν΄μ€λ‘ μ μ. 


- μ¬κ·λ‘ κ΅¬νν BeanFactory μ½λ

```java
public class BeanFactory {
    private static final Logger logger = LoggerFactory.getLogger(BeanFactory.class);

    private Set<Class<?>> preInstanticateBeans;

    private Map<Class<?>, Object> beans = Maps.newHashMap();

    public BeanFactory(Set<Class<?>> preInstanticateBeans) {
        this.preInstanticateBeans = preInstanticateBeans;
    }

    @SuppressWarnings("unchecked")
    public <T> T getBean(Class<T> requiredType) {
        return (T) beans.get(requiredType);
    }

    public void initialize() {
        for (Class<?> clazz : preInstanticateBeans) {
            if (beans.get(clazz) == null) {
                logger.debug("instantiated Class : {}", clazz);
                instantiateClass(clazz);
            }
        }
    }

    private Object instantiateClass(Class<?> clazz) {
        Object bean = beans.get(clazz);
        if (bean != null) {
            return bean;
        }

        Constructor<?> injectedConstructor = BeanFactoryUtils.getInjectedConstructor(clazz);
        if (injectedConstructor == null) {
            bean = BeanUtils.instantiate(clazz);
            beans.put(clazz, bean);
            return bean;
        }

        logger.debug("Constructor : {}", injectedConstructor);
        bean = instantiateConstructor(injectedConstructor);
        beans.put(clazz, bean);
        return bean;
    }

    private Object instantiateConstructor(Constructor<?> constructor) {
        Class<?>[] pTypes = constructor.getParameterTypes();
        List<Object> args = Lists.newArrayList();
        for (Class<?> clazz : pTypes) {
            Class<?> concreteClazz = BeanFactoryUtils.findConcreteClass(clazz, preInstanticateBeans);
            if (!preInstanticateBeans.contains(concreteClazz)) {
                throw new IllegalStateException(clazz + "λ Beanμ΄ μλλ€.");
            }

            Object bean = beans.get(concreteClazz);
            if (bean == null) {
                bean = instantiateClass(concreteClazz);
            }
            args.add(bean);
        }
        return BeanUtils.instantiateClass(constructor, args.toArray());
    }
}
```

- @Controller, @Service, @Repositoryλ‘ μ€μ λμ΄ μλ λΉμ μ°Ύλ BeanScannerλ₯Ό κ΅¬ν

```java
public class BeanScanner {
    private Reflections reflections;

    public BeanScanner(Object... basePackage) {
        reflections = new Reflections(basePackage);
    }

    @SuppressWarnings("unchecked")
    public Set<Class<?>> scan() {
        return getTypesAnnotatedWith(Controller.class, Service.class, Repository.class);
    }

    @SuppressWarnings("unchecked")
    private Set<Class<?>> getTypesAnnotatedWith(Class<? extends Annotation>... annotations) {
        Set<Class<?>> preInstantiatedBeans = Sets.newHashSet();
        for (Class<? extends Annotation> annotation : annotations) {
            preInstantiatedBeans.addAll(reflections.getTypesAnnotatedWith(annotation));
        }
        return preInstantiatedBeans;
    }
}
```

- BeanFactory μ @Controller λ‘ μ€μ ν λΉμ μ‘°νν  μ μλ getControllers() λ©μλλ₯Ό μΆκ°

```java
public class BeanFactory {
    private static final Logger logger = LoggerFactory.getLogger(BeanFactory.class);

    private Set<Class<?>> preInstanticateBeans;

    private Map<Class<?>, Object> beans = Maps.newHashMap();

    [...]

		public Map<Class<?>, Object> getControllers() {
        Map<Class<?>, Object> controllers = Maps.newHashMap();
        for (Class<?> clazz : preInstanticateBeans) {
            if (clazz.isAnnotationPresent(Controller.class)) {
                controllers.put(clazz, beans.get(clazz));
            }
        }
        return controllers;
    }
}
```

- AnnotationHandlerMapping μμμ λ¦¬ν©ν λ§
