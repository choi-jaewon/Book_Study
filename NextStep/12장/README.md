# **πΏ νλμ setter λ©μλμ @Inject κΈ°λ₯ μΆκ°**
## **β· μκ΅¬μ¬ν­**

`@Inject` λ₯Ό νμ©ν΄ DIν¨μΌλ‘μ¨ λΉ κ°μ μμ‘΄κ΄κ³λ₯Ό μ½κ² μ°κ²°ν  μ μλ€. νμ§λ§ νμ¬λ μμ±μλ₯Ό ν΅ν΄μλ§ DIκ° κ°λ₯νλ€. λ°λΌμ μμ±μ μ΄μΈμ **νλ, `setter` λ©μλ**λ₯Ό ν΅ν΄μλ DIν  μ μλλ‘ κΈ°λ₯μ μΆκ°νλ€.

## **π 1λ¨κ³ ννΈ - ν΄λμ€ μ€κ³**

μμ±μ, νλ, `setter` λ©μλ, μ΄ 3κ°μ§μ κ²½μ°μ μκ° μλ€. μ§κΈκΉμ§ μ¬λ¬ κ²½μ°μ μκ° μλ κ²½μ° **μΆμν κ³Όμ μ ν΅ν΄ μΈν°νμ΄μ€λ₯Ό μΆκ°**ν ν κ° κ²½μ°μ λν κ΅¬νμ μννλ€.

`BeanFactory` μ μ­ν μ λΉμ μΆκ°νκ³ , μ‘°ννλ μ­ν λ§ λ¨κΈ°κ³  μμ±μλ₯Ό νμ©ν΄ DIλ₯Ό νκ³  μΈμ€ν΄μ€ μμ±μ `ConstructorInjector` λΌλ μ΄λ¦μΌλ‘ λΆλ¦¬νλ€.

- `Injector` λΌλ μΈν°νμ΄μ€λ₯Ό μΆκ°νλ€. μ΄λ μμ±μ, νλ, `setter` λ©μλ DIμ λν μΆμ λ©μλλ‘ `inject()` λ₯Ό κ°μ§λ€.
- μμ±μ, νλ, `setter` λ©μλμ λν DIλ₯Ό λ΄λΉνλ ν΄λμ€λ₯Ό κ΅¬ννλ€. μ΄λ `Injector` μΈν°νμ΄μ€λ₯Ό κ΅¬ννλ€.
    - 3κ°μ κ΅¬νμ²΄λ₯Ό κ΅¬ννλ κ³Όμ μμ λ°μνλ μ€λ³΅ μ½λλ€μ `Injector` μΈν°νμ΄μ€μ κ΅¬νμ²΄ μ¬μ΄μ `AbstractInjector` μ κ°μ μΆμ ν΄λμ€λ‘ μ κ±°νλ€.

## **πͺ 2λ¨κ³ ννΈ**

μμ±μ κ΅¬νμ²΄(`ConstructorInjector`)λ `@Inject` κ° μ€μ λμ΄ μλ μμ±μλ₯Ό κ°μ§λ ν΄λμ€λ₯Ό μ°Ύλλ€.

- μμ±μμ μΈμμ ν΄λΉνλ λΉμ΄ `BeanFactory` μ λ±λ‘λμ΄ μλμ§ νμΈν΄λ³΄κ³  λ±λ‘λμ΄ μμ§ μλ€λ©΄ λΉ μΈμ€ν΄μ€λ₯Ό μμ±νμ¬ DIνλ€.

νλ κ΅¬νμ²΄(`FieldInjector`)μ `setter` λ©μλ κ΅¬νμ²΄(`SetterInjector`) λν κ°μ λ°©μμΌλ‘ κ΅¬ννλ€.

- νλ κ΅¬νμ²΄μ κ²½μ° ν΄λμ€μ `@Injector`κ° μ€μ λμ΄ μλ λͺ¨λ  νλλ₯Ό μ°Ύλλ€.
- νλ νμ(ν΄λμ€)μ ν΄λΉνλ λΉμ΄ λ±λ‘λμ΄ μλμ§ νμΈν΄λ³΄κ³  λ±λ‘λμ΄ μμ§ μλ€λ©΄ λΉ μΈμ€ν΄μ€λ₯Ό μμ±νμ¬ DIνλ€.

## **ποΈββοΈ μ€λ³΅ μ κ±°λ₯Ό μν ννΈ**

`@Injector` κ° μ€μ λμ΄ μλ μμΉκ° λ€λ₯΄λ€λ μ μ μ μΈνλ©΄, 3κ°μ κ΅¬νμ²΄μ λ‘μ§ μ²λ¦¬ κ³Όμ μ λμΌνλ€.

- μ΄μ κ°μ΄ λ‘μ§μ΄ κ°μ κ²½μ° **ννλ¦Ώ λ©μλ ν¨ν΄**μ μ μ©ν  μ μλ€.
- λΆλͺ¨ ν΄λμ€μΈ `AbstractInjector` λ λ‘μ§ κ΅¬νμ λ΄λΉνκ³ , 3κ°μ νμ ν΄λμ€(κ΅¬νμ²΄)λ κ° ν΄λμ€λ§λ€ λ€λ₯Έ λΆλΆλ§μ κ΅¬ννλ€.

---

# **ποΈ νλμ setter λ©μλ @Inject κ΅¬ν**

`ConstructorInjector` λ μ΄μ μ κ΅¬νν `BeanFactory` μ `instantiateClass()` , `instantiateConstructor()` λ©μλμ κ°λ€.

- λ€λ₯Έ μ μ λΉμ μ μ₯νκΈ° μν `Map` μ μ§μ  κ°μ§μ§ μκ³  `BeanFactory` λ₯Ό ν΅ν΄ μ κ·Όνλλ‘ νλ€λ κ²μ΄λ€.
- `ConstructorInjector` μ `instantiateClass()` λ μμΌλ‘ μΆκ°ν  `FieldInjector` , `SetterInjector` λ νμνλ―λ‘, μ΄λ€μ μ€λ³΅ μ κ±°λ₯Ό μν ν΄λμ€μΈ `AbstractInjector` μμ κ΅¬ννλλ‘ νλ€.

```java
public abstract class AbstractInjector implements Injector {
    private BeanFactory beanFactory;

    public AbstractInjector(BeanFactory beanFactory) {
        this.beanFactory = beanFactory;
    }

    @Override
    public void inject(Class<?> clazz) {
        instantiateClass(clazz);
        Set<?> injectedBeans = getInjectedBeans(clazz);
        for (Object injectedBean : injectedBeans) {
            Class<?> beanClass = getBeanClass(injectedBean);
            inject(injectedBean, instantiateClass(beanClass), beanFactory);
        }
    }

    abstract Set<?> getInjectedBeans(Class<?> clazz);

    abstract Class<?> getBeanClass(Object injectedBean);

    abstract void inject(Object injectedBean, Object bean, BeanFactory beanFactory);

    private Object instantiateClass(Class<?> clazz) {
        Class<?> concreteClass = findBeanClass(clazz, beanFactory.getPreInstanticateBeans());
        Object bean = beanFactory.getBean(concreteClass);
        if (bean != null) {
            return bean;
        }

        Constructor<?> injectedConstructor = BeanFactoryUtils.getInjectedConstructor(concreteClass);

        if (injectedConstructor == null) {
            bean = BeanUtils.instantiate(concreteClass);
            beanFactory.registerBean(concreteClass, bean);
            return bean;
        }

        bean = instantiateConstructor(injectedConstructor);
        beanFactory.registerBean(concreteClass, bean);
        return bean;
    }

    private Object instantiateConstructor(Constructor<?> constructor) {
        Class<?>[] pTypes = constructor.getParameterTypes();
        List<Object> args = Lists.newArrayList();

        for (Class<?> clazz : pTypes) {
            Class<?> concreteClazz = BeanFactoryUtils.findConcreteClass(clazz, beanFactory.getPreInstanticateBeans());
            Object bean = beanFactory.getBean(concreteClazz);
            if (bean == null) {
                bean = instantiateClass(concreteClazz);
            }
            args.add(bean);
        }
        return BeanUtils.instantiateClass(constructor, args.toArray());
    }

    private Class<?> findBeanClass(Class<?> clazz, Set<Class<?>> preInstanticateBeans) {
        Class<?> concreteClazz = BeanFactoryUtils.findConcreteClass(clazz, preInstanticateBeans);
        if (!preInstanticateBeans.contains(concreteClazz)) {
            throw new IllegalStateException(clazz + "λ Beanμ΄ μλλ€.");
        }
        return concreteClazz;
    }
}
```

- `BeanFactory` μ `private` μμλ€μ λν μ κ·Όμ μν΄ `BeanFactory` μ μ μΈν `getBean()` , `getPreInstanticateBeans()` , `registerBean()` λ©μλλ₯Ό νμ©νλ€.

```java
public class ConstructorInjector extends AbstractInjector {
    public ConstructorInjector(BeanFactory beanFactory) {
        super(beanFactory);
    }

    @Override
    Set<?> getInjectedBeans(Class<?> clazz) {
        return Sets.newHashSet();
    }

    @Override
    Class<?> getBeanClass(Object injectedBean) {
        return null;
    }

    @Override
    void inject(Object injectedBean, Object bean, BeanFactory beanFactory) {
    }
}
```

- `ConstructorInjector` λ κ΅¬ν λ‘μ§μ΄ λͺ¨λ `AbstractInjector` λ‘ μ΄λνμΌλ―λ‘ κ°λ¨νλ€.

```java
public class FieldInjector extends AbstractInjector {
    private static final Logger log = LoggerFactory.getLogger(FieldInjector.class);

    public FieldInjector(BeanFactory beanFactory) {
        super(beanFactory);
    }

    @Override
    Set<?> getInjectedBeans(Class<?> clazz) {
        return BeanFactoryUtils.getInjectedFields(clazz);
    }

    @Override
    Class<?> getBeanClass(Object injectedBean) {
        Field field = (Field)injectedBean;
        return field.getType();
    }

    @Override
    void inject(Object injectedBean, Object bean, BeanFactory beanFactory) {
        Field field = (Field) injectedBean;
        try {
            field.setAccessible(true);
            field.set(beanFactory.getBean(field.getDeclaringClass()), bean);
        } catch (IllegalAccessException | IllegalArgumentException e) {
            log.error(e.getMessage());
        }
    }
}
```

```java
public class SetterInjector extends AbstractInjector {
    private static final Logger log = LoggerFactory.getLogger(SetterInjector.class);

    public SetterInjector(BeanFactory beanFactory) {
        super(beanFactory);
    }

    @Override
    Set<?> getInjectedBeans(Class<?> clazz) {
        return BeanFactoryUtils.getInjectedMethods(clazz);
    }

    @Override
    Class<?> getBeanClass(Object injectedBean) {
        Method method = (Method) injectedBean;
        Class<?>[] paramTypes = method.getParameterTypes();
        if (paramTypes.length != 1) {
            throw new IllegalStateException("DIν  λ©μλ μΈμλ νλμ¬μΌ ν©λλ€.");
        }
        return paramTypes[0];
    }

    @Override
    void inject(Object injectedBean, Object bean, BeanFactory beanFactory) {
        Method method = (Method) injectedBean;
        try {
            method.invoke(beanFactory.getBean(method.getDeclaringClass()), bean);
        } catch (IllegalAccessException | IllegalArgumentException | InvocationTargetException e) {
            log.error(e.getMessage());
        }
    }
}
```

ννλ¦Ώ λ©μλ ν¨ν΄μ μ μ©ν΄ λ‘μ§ μ€λ³΅μ μ κ±°νμ¬ νμ ν΄λμ€μ κ΅¬νμ΄ λ§€μ° κ°λ¨ν΄μ‘λ€. μ΄μ  `BeanFactory` κ° λ°©κΈκΉμ§ κ΅¬νν 3κ°μ `Injector` λ₯Ό μ¬μ©νλλ‘ λ¦¬ν©ν λ§νλ€.

```java
public class BeanFactory {
    private static final Logger logger = LoggerFactory.getLogger(BeanFactory.class);

    private Set<Class<?>> preInstanticateBeans;

    private Map<Class<?>, Object> beans = Maps.newHashMap();

    private List<Injector> injectors;

    public BeanFactory(Set<Class<?>> preInstanticateBeans) {
        this.preInstanticateBeans = preInstanticateBeans;

        injectors = Arrays.asList(
                new FieldInjector(this),
                new SetterInjector(this),
                new ConstructorInjector(this)
        );
    }

    public void initialize() {
        for (Class<?> clazz : preInstanticateBeans) {
            if (beans.get(clazz) == null) {
                logger.debug("instantiated Class : {}", clazz);
                inject(clazz);
            }
        }
    }

    private void inject(Class<?> clazz) {
        for (Injector injector : injectors) {
            injector.inject(clazz);
        }
    }
    ...

}
```

- `BeanFactory` μμ±μ νΈμΆ μ, κ΅¬νν `Injector` λ€μ μ΄κΈ°νν΄μ£Όκ³ , `initialize()` λ©μλ νΈμΆλ‘, μ΄κΈ°νν `Injector` λ€μ λν DIλ₯Ό μννλ€.

---

# **ποΈββοΈ @Inject κ°μ **

`BeanFactory` μ `AbstractInjector` μ½λλ₯Ό λ³΄λ©΄, μ€λ³΅μ μ κ±°νκΈ°λ νμ§λ§ μμ€ μ½λμ μ΄ν΄λκ° λ¨μ΄μ§λ€. λν, νμ¬ λͺ¨λ  λΉκ³Ό κ΄λ ¨ν μ λ³΄λ `BeanFactory` κ° κ΄λ¦¬νλλ°, λΉ μΈμ€ν΄μ€ μμ±κ³Ό μ£Όμμ `Injector` κ΅¬ν ν΄λμ€κ° λ΄λΉνλ κ΅¬μ‘°μ¬μ `BeanFactory` μκ² μΌμ μν€λ κ²μ΄ μλ **λΉ μ λ³΄ μ‘°ν**κ° κ³μ λ°μνλ€.

- μ¦, `BeanFactory` κ°μ²΄μ νμ©λκ° λ¨μ΄μ§λ κ΅¬μ‘°μ΄λ€.
- νμ©λλ₯Ό λμ΄κΈ° μν΄ λΉ μΈμ€ν΄μ€ μμ±κ³Ό μ£Όμ μμμ `BeanFactory` κ°, νμ¬ λΉ ν΄λμ€μ μν μ λ³΄λ₯Ό λ³λμ ν΄λμ€λ‘ μΆμν(`BeanDefinition`)ν΄ κ΄λ¦¬νκ²λ νλ€.
- μ€μ λ annotationμ μλ ν΄λμ€λ₯Ό μ‘°ννλ μ­ν μ νλ `BeanScanner` ν΄λμ€μ μ΄λ¦λ `ClasspathBeanDefinitionScanner` λ‘ renameνλ€.
    - μ΄λ annotationμ΄ μ€μ λ ν΄λμ€λ₯Ό μ‘°νν ν `BeanDefinition` μ μμ±ν΄ `BeanFactory` μ μ λ¬νλ€. μ΄λ, `ClasspathBeanDefinitionScanner` μ `BeanFactory` κ° κ°ν μμ‘΄κ΄κ³λ₯Ό κ°μ§μ§ μλλ‘ μ€κ³νλ€. 
    (`BeanDefinition` μ `BeanFactory` μ μ λ¬νλ λΆλΆμμλ§ λ°μνλλ‘ κ΅¬ν, `BeanDefinition` μ μ μ₯νλ μΈν°νμ΄μ€(`BeanDefinitionRegistry`)λ₯Ό μΆκ°νμ¬ μμ‘΄κ΄κ³λ₯Ό λμ¨νκ²!)
    - μ¦, `BeanFactory` λ **`BeanDefinition` μ μ μ₯νλ μ μ₯μ μ­ν **κ³Ό **`BeanDefinition` μ νμ©ν΄ λΉ μΈμ€ν΄μ€ μμ±, μμ‘΄κ΄κ³ μ£Όμμ λ΄λΉνλ μ­ν **λ‘ λλλ€.

```java
public interface BeanDefinitionRegistry {
    void registerBeanDefinition(Class<?> clazz, BeanDefinition beanDefinition);
}
```

```java
public class ClasspathBeanDefinitionScanner {
    private final BeanDefinitionRegistry beanDefinitionRegistry;

    public ClasspathBeanDefinitionScanner(BeanDefinitionRegistry beanDefinitionRegistry) {
        this.beanDefinitionRegistry = beanDefinitionRegistry;
    }

    @SuppressWarnings("unchecked")
    public void doScan(Object... basePackages) {
        Reflections reflections = new Reflections(basePackages);
        Set<Class<?>> beanClasses = getTypesAnnotatedWith(reflections, Controller.class, Service.class, Repository.class);
        for (Class<?> clazz : beanClasses) {
            beanDefinitionRegistry.registerBeanDefinition(clazz, new BeanDefinition(clazz));
        }
    }

    @SuppressWarnings("unchecked")
    private Set<Class<?>> getTypesAnnotatedWith(Reflections reflections, Class<? extends Annotation>... annotations) {
        Set<Class<?>> preInstantiatedBeans = Sets.newHashSet();
        for (Class<? extends Annotation> annotation : annotations) {
            preInstantiatedBeans.addAll(reflections.getTypesAnnotatedWith(annotation));
        }
        return preInstantiatedBeans;
    }
}
```

- μ΄λ‘μ¨, κ° κ°μ²΄μ μ­ν μ λΆλ¦¬νλ€.
    - `ClasspathBeanDefinitionScanner` λ ν΄λμ€ν¨μ€μμ λΉμ μ‘°ν(`getTypesAnnotatedWith()`)νλ μ­ν μ λ΄λΉνκ³ ,
    - μ‘°νν λΉ μ λ³΄λ₯Ό `BeanDefinition` μ μμ±ν΄ `beanDefinitionRegistry` μ μ λ¬(`beanDefinitionRegistry.registerBeanDefinition(clazz, new BeanDefinition(clazz));`)
    - `BeanDefinitionRegistry` κ΅¬νμ²΄κ° `BeanDefinition` μ μ μ₯μ μ­ν μ λ΄λΉνλ€.

`BeanFactory` λ λΉ μΈμ€ν΄μ€λ₯Ό μμ±νκ³ , μμ‘΄κ΄κ³ μ£Όμμ μν΄μλ `BeanDefinition` μ λ³΄κ° νμνλ€.

- μ΄λ `BeanFactory` κ° `BeanDefinitionRegistry` κ΅¬νμ²΄λ‘ `BeanDefinition` μ λ³΄λ₯Ό κ΄λ¦¬νλλ‘ κ΅¬ννλ€.

```java
public class BeanFactory implements BeanDefinitionRegistry {
    private static final Logger logger = LoggerFactory.getLogger(BeanFactory.class);
    ...

    private Map<Class<?>, BeanDefinition> beanDefinitions = Maps.newHashMap();
    ...

    public void initialize() {
        for (Class<?> clazz : getBeanClasses()) {
            getBean(clazz);
        }
    }

    public Set<Class<?>> getBeanClasses() {
        return beanDefinitions.keySet();
    }

    @Override
    public void registerBeanDefinition(Class<?> clazz, BeanDefinition beanDefinition) {
        logger.debug("register bean : {]", clazz);
        beanDefinitions.put(clazz, beanDefinition);
    }
}
```

`BeanFactory` μ `ClasspathBeanDefinitionScanner` μ μμ‘΄κ΄κ³μ λν μ°κ²°μ μ΄ λ ν΄λμ€λ₯Ό νμ©νλ κ³³μμ λ΄λΉνλ€. μ§κΈκΉμ§λ `AnnotationHandlerMapping` μ΄ μ΄λ₯Ό λ΄λΉνλ€.

```java
public class AnnotationHandlerMapping implements HandlerMapping {
    private static final Logger logger = LoggerFactory.getLogger(AnnotationHandlerMapping.class);

    private Object[] basePackage;

    private Map<HandlerKey, HandlerExecution> handlerExecutions = Maps.newHashMap();

    public AnnotationHandlerMapping(Object... basePackage) {
        this.basePackage = basePackage;
    }

    public void initialize() {
        BeanFactory beanFactory = new BeanFactory();
        ClasspathBeanDefinitionScanner scanner = new ClasspathBeanDefinitionScanner(beanFactory);
        scanner.doScan(basePackage);
        beanFactory.initialize();
        ...

    }
    ...

}
```

`BeanFactory` μ `ClasspathBeanDefinitionScanner` μ μμ‘΄κ΄κ³ λν DIλ₯Ό νμ©ν΄ κ΅¬ννμ¬ μ μ°μ±μ νλ³΄νλ€.

- `ClasspathBeanDefinitionScanner` λ λ¨μν ν΄λμ€ ν¨μ€μμ λΉμ μ‘°νν ν `BeanDefinition` μ μμ±ν΄ `BeanDefinitionRegistry` μ `registerBeanDefinition()` λ©μλλ‘ μ λ¬νλ μ­ν μ λ΄λΉνλ€.
- νμ§λ§, μ΄ κ²½μ° **κ°μ²΄ κ°μ DIλ₯Ό λ΄λΉν  μ½λκ° νμ**νλ€λ λ¨μ μ΄ μλ€.
    - κ°μ²΄ κ°μ μμ‘΄κ΄κ³λ₯Ό μ°κ²°νμ§ μμ μ±λ‘ APIλ₯Ό μ κ³΅νλ©΄, μ΄λ₯Ό μ¬μ©νλ κ°λ°μλ μΌμΌμ΄ DIλ₯Ό ν΄μ€μΌ νλ€.
- μ΄λ¬ν λ¨μ  λ³΄μμ μν΄ **κ°μ²΄ κ°μ DIλ₯Ό λ΄λΉνλ μλ‘μ΄ κ°μ²΄λ₯Ό μΆκ°**νλ€. (`ApplicationContext`)

```java
public class ApplicationContext {
    private BeanFactory beanFactory;
    
    public ApplicationContext(Object... basePackages) {
        beanFactory = new BeanFactory();
        ClasspathBeanDefinitionScanner scanner = new ClasspathBeanDefinitionScanner(beanFactory);
        scanner.doScan(basePackages);
        beanFactory.initialize();
    }
    
    public <T> T getBean(Class<T> clazz) {
        return beanFactory.getBean(clazz);
    }
    
    public Set<Class<?>> getBeanClasses() {
        return beanFactory.getBeanClasses();
    }
}
```

- `BeanFactory` μ΄κΈ°ν κ³Όμ μ `ApplicationContext` λ‘ μ?κΈ°κ³ , `BeanFactory` λ‘ μ§μ  μ κ·Όνλ APIλ₯Ό `ApplicationContext` λ₯Ό ν΅ν΄ μ κ·Όνλλ‘ λ¦¬ν©ν λ§νλ€.
    - μ΄λ, `BeanFactory` κ° λ΄λΉνλ `getControllers()` λ©μλμ μ­ν μ΄ μ ν©νμ§ μμ μ΄λ₯Ό `AnnotationHandlerMapping` μΌλ‘ μ΄λνκ³  κ΅¬νμ μν΄ νμν κΈ°λ₯λ§ `ApplicationContext` λ₯Ό ν΅ν΄ μ κ·Όνλλ‘ κ΅¬ννλ€.

```java
public class AnnotationHandlerMapping implements HandlerMapping {
    ...

    public void initialize() {
        ApplicationContext ac = new ApplicationContext(basePackage);
        Map<Class<?>, Object> controllers = getControllers(ac);
        ...

    }

    private Map<Class<?>, Object> getControllers(ApplicationContext ac) {
        Map<Class<?>, Object> controllers = Maps.newHashMap();
        for (Class<?> clazz : ac.getBeanClasses()) {
            Annotation annotation = clazz.getAnnotation(Controller.class);
            if (annotation != null) {
                controllers.put(clazz, ac.getBean(clazz));
            }
        }
        return controllers;
    }
}
```

- κ°μ²΄ κ°μ μμ‘΄κ΄κ³ μ°κ²°μ λ΄λΉνλλ‘ κ΅¬ννμ¬ λ μ΄μ `BeanFactory` λ₯Ό μ¬μ©νλ κ°λ°μκ° μμ‘΄κ΄κ³λλ¬Έμ λ¨Έλ¦¬ μννμ§ μμλ λλ€.

λΉ ν΄λμ€ μ λ³΄λ₯Ό λ΄κ³  μλ `BeanDefinition` μ μμ±μλ‘ μ λ¬λλ ν΄λμ€μμ `@Inject` κ° μ΄λ»κ² μ€μ λμ΄ μλμ§μ λ°λΌ `InjectType` μ κ²°μ νλ€. (`enum` μ¬μ©)

- μ΄λ, `InjectType` μμλ νλμ `setter` λ©μλλ₯Ό νμ©ν `@Inject` λ λμΌνκ² μ·¨κΈνλλ°, μ΄λ λ΄λΆ κ΅¬νμ μ°¨μ΄κ° μκΈ° λλ¬Έμ΄λ€.

λΉ ν΄λμ€ μ λ³΄λ₯Ό νμ©ν΄ λΉ μΈμ€ν΄μ€λ₯Ό μμ±νκ³  μμ‘΄κ΄κ³ μ£Όμμ λ΄λΉνλ `BeanFactory` λ λΉ ν΄λμ€μ μμ‘΄κ΄κ³μ κ΄λ ¨ν μ λ³΄ μ²λ¦¬λ₯Ό λͺ¨λ `BeanDefinition` μκ² μμνμ¬ λ΄λΉν  μ±μμ΄ μ€μλ€.

---

# **π€ΌββοΈ μ€μ  μΆκ°λ₯Ό ν΅ν μ μ°μ± νλ³΄**

μ§κΈκΉμ§ κ΅¬νν μ½λλ₯Ό λΉνμ μΈ μκ°μΌλ‘ λΆμνμλ, μλμ κ°μ κ°μ μ¬ν­λ€μ΄ λ³΄μΈλ€.

- `DispatcherServlet` μ `init()` λ©μλλ₯Ό λ³΄λ©΄ `"next"` λΌλ ν¨ν€μ§ μ΄λ¦μ΄ νλ μ½λ©λμ΄ μλ€. κΈ°λ³Έ ν¨ν€μ§λͺμ μΈλΆμμ μ λ¬ν  μ μλλ‘ κ΅¬ννλ€.
- `JdbcTemplate` μ΄ μμ§κΉμ§ μ±κΈν€ ν¨ν΄μΌλ‘ κ΅¬νλμ΄ μλ€. μ΄λ₯Ό DI νλ μμν¬μ λΉμΌλ‘ λ±λ‘ν΄ κ΄λ¦¬ν¨μΌλ‘μ¨, μ±κΈν€ ν¨ν΄μ μ κ±°νλ€.
    - μ΄λ, κ° λ μ΄μ΄μ λͺνν μΌμΉνμ§ μλ λΉμ μ§μνλ annotationμ μΆκ°ν  νμκ° μλ€.
- DB Connectionμ μμ±νλ λΆλΆλ `static` μΌλ‘ κ΅¬νλμ΄ μλ€. λν DB μ€μ  μ λ³΄λ νλ μ½λ©μΌλ‘ κ΄λ¦¬λμ΄ νΉμ  DBμ μ’μλλ κ΅¬μ‘°μ΄λ€. μ΄λ¬ν κ΅¬μ‘°λ₯Ό λ²μ΄λκ³ , **Connection Poolingμ** μ§μνκΈ° μν΄ `Connection` λμ  `javax.sql.DataSource` μΈν°νμ΄μ€μ μμ‘΄κ΄κ³λ₯Ό κ°μ§λλ‘ κ°μ νλ€.

μΈλ²μ§Έ μ΄μλ `DataSource` κ΅¬νμ²΄λ‘ Apache Commonsμμ μ κ³΅νλ DBCP λΌμ΄λΈλ¬λ¦¬λ₯Ό νμ©ν΄ DB μ€μ ν ν λΉμΌλ‘ λ±λ‘νλ©΄ ν΄κ²°μ΄ κ°λ₯νλ€.

- μ§κΈκΉμ§λ λͺ¨λ μ§μ  κ΅¬νν ν΄λμ€λ₯Ό λΉμΌλ‘ λ±λ‘νκΈ°μ annotation μ€μ μ΄ κ°λ₯νλ€.
- νμ§λ§ DBCP κ΅¬νμ²΄μ κ²½μ° μΈλΆ λΌμ΄λΈλ¬λ¦¬μ΄κΈ°λλ¬Έμ μμ μ΄ λΆκ°λ₯νλ€. μΈλΆ λΌμ΄λΈλ¬λ¦¬ μ¬μ©μ μν΄ κ΅¬νμ²΄λ₯Ό μμν΄ λΉ μ€μ μ νλ©΄ ν΄κ²°μ΄ κ°λ₯νμ§λ§, λ§€λ² μ΄λ¬ν κ³Όμ μ κ±°μΉλ κ²μ ν¨μ¨μ μ΄μ§ λͺ»νλ€.
- λ°λΌμ, μ²«λ²μ§Έμ λλ²μ§Έ μ΄μλΆν° κ°μ νλλ‘ νλ€.

## **π€Ό ServletContainerInitializerλ₯Ό νμ©ν΄ web.xml μμ΄ μΉ κ°λ°νκΈ°**

- **μ²«λ²μ§Έ μ΄μ**

: μλΈλ¦Ώμ μΈμλ₯Ό μ λ¬ν¨μΌλ‘μ¨ ν΄κ²°μ΄ κ°λ₯νλ€.

μλΈλ¦Ώ μ»¨νμ΄λμ κ²½μ°, `web.xml` μ€μ μ ν΅ν΄ μλΈλ¦Ώμ μΈμλ₯Ό μ λ¬ν  μ μλ€. νμ§λ§ μλΈλ¦Ώ 3.0 μ΄ν λ²μ μμλ `web.xml` μ μ¬μ©νμ§ μκ³ λ μ€μ μ΄ κ°λ₯νλ°, μ΄λ `ServletContainerInitializer` μΈν°νμ΄μ€λ₯Ό κ΅¬νν¨μΌλ‘μ¨ κ°λ₯νλ€.

```java
public interface WebApplicationInitializer {
    void onStartup(ServletContext servletContext) throws ServletException;
}
```

μ μΈν°νμ΄μ€μ λν κ΅¬νμ²΄λ₯Ό κ΅¬ννλ€. (`MyServletContainerInitializer`)

```java
@HandlesTypes(WebApplicationInitializer.class)
public class MyServletContainerInitializer implements ServletContainerInitializer {
    @Override
    public void onStartup(Set<Class<?>> webAppInitializerClasses, ServletContext servletContext) throws ServletException {
        List<WebApplicationInitializer> initializers = new LinkedList<>();

        if (webAppInitializerClasses != null) {
            for (Class<?> waiClass : webAppInitializerClasses) {
                try {
                    initializers.add((WebApplicationInitializer) waiClass.newInstance());
                } catch (Throwable ex) {
                    throw new ServletException("WebApplicationInitializer classλ₯Ό μΈμ€ν΄μ€ννλλ° μ€ν¨νμ΅λλ€.", ex);
                }
            }
        }
        
        if (initializers.isEmpty()) {
            servletContext.log("classpathμμ κ°μ§λ Spring WebApplicationInitializer typesκ° μμ΅λλ€.");
            return;
        }
        
        for(WebApplicationInitializer initializer : initializers) {
            initializer.onStartup(servletContext);
        }
    }
}
```

- μλΈλ¦Ώ μ»¨νμ΄λλ ν΄λμ€ν¨μ€μ μ‘΄μ¬νλ ν΄λμ€ μ€ `WebApplicationInitializer` μΈν°νμ΄μ€λ₯Ό κ΅¬ννλ λͺ¨λ  κ΅¬νμ²΄λ₯Ό μ°Ύμ `MyServletContainerInitializer` μ `onStartup()` λ©μλ μΈμλ‘ μ λ¬νλ€.
- ν΄λΉ λ©μλλ μλΈλ¦Ώ μ»¨νμ΄λμ μ λ¬ν `WebApplicationInitializer` μ `onStartup()` λ©μλλ₯Ό μ€ννλ€.

> `WebApplicationInitializer` μΈν°νμ΄μ€λ₯Ό κ΅¬νν¨μΌλ‘μ¨ μλΈλ¦Ώ μ»¨νμ΄λμ μ΄κΈ°ν κ³Όμ μ νμ₯ν  μ μκ² λμλ€.
> 

`MyServletContainerInitializer` μ `onStartup()` λ©μλ μ€νμ μν΄μλ μλΈλ¦Ώ μ»¨νμ΄λκ° μ΄ ν΄λμ€λ₯Ό μΈμνλ κ²μ΄ λ¨Όμ μ΄λ€. μ΄λ₯Ό μν μ€μ μ ν΄λμ€ν¨μ€μ `META-INF/services` μ `javax.servlet.ServletContainerInitializer` νμΌμ μμ±ν΄, `core.web.MyServletContainerInitializer` μ μΆκ°νλ€.

μ΄μ  `web.xml` μμ΄λ κ°λ°μ΄ κ°λ₯ν μνκ° λμμΌλ, νλμ½λ©μΌλ‘ κ΅¬νλμ΄ μλ ν¨ν€μ§ μ΄λ¦μ `WebApplicationInitializer` μ λν κ΅¬νμ²΄λ₯Ό λ§λ€μ΄ κ΅¬ν κ°λ₯νλ€.

```java
public class MyWebApplicationInitializer implements WebApplicationInitializer {
    private static final Logger log = LoggerFactory.getLogger(MyWebApplicationInitializer.class);

    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        ServletRegistration.Dynamic dispatcher = servletContext.addServlet("dispatcher", new DispatcherServlet("next"));
        dispatcher.setLoadOnStartup(1);
        dispatcher.addMapping("/");

        log.info("Start MyWebApplication Initializer");
    }
}
```

- `MyWebApplicationInitializer` μμ `DispatcherServlet` μ μ§μ  μμ±ν΄ λ±λ‘(`LoadOnStartup` , `Mapping` μμ± λν λ±λ‘) `DispatcherServlet` μ `@WebServlet` annotation μ€μ μ λμ΄μ νμμλ€.

```java
public class DispatcherServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    private static final Logger logger = LoggerFactory.getLogger(DispatcherServlet.class);

    private List<HandlerMapping> mappings = Lists.newArrayList();
    private List<HandlerAdapter> handlerAdapters = Lists.newArrayList();
    private Object[] basePackages;
    
    public DispatcherServlet(Object... basePackages) {
        this.basePackages = basePackages;
    }
    
    @Override
    public void init() throws ServletException {
        AnnotationHandlerMapping ahm = new AnnotationHandlerMapping(basePackages);
        ahm.initialize();
        mappings.add(ahm);
        handlerAdapters.add(new HandlerExecutionHandlerAdapter());
    }
    ...

}
```

- `init()` λ©μλμμ `AnnotationHandlerMapping` κ³Ό μ§μ μ μΈ μμ‘΄κ΄κ³λ₯Ό κ°μ§λ κ²μ νμΈν  μ μλ€. μ΄λν DIλ₯Ό ν΅ν΄ μμ‘΄κ΄κ³λ₯Ό λμ¨νκ² κ°μ§λλ‘ λ³κ²½ν΄ νμ₯ κ°λ₯νλλ‘ κ°μ ν  μ μλ€.
    - μμ±μμ `HandlerMapping` μ μΈμλ‘ μ λ¬ν  μ μλλ‘ λ¦¬ν©ν λ§νλ€.

```java
public class DispatcherServlet extends HttpServlet {
    ...

    private HandlerMapping hm;

    public DispatcherServlet(HandlerMapping hm) {
        this.hm = hm;
    }

    @Override
    public void init() throws ServletException {
        mappings.add(hm);
        handlerAdapters.add(new HandlerExecutionHandlerAdapter());
    }
    ...

}
```

- `DispatcherServlet` μ΄ μ΄λ `HandlerMapping` κ΅¬νμ²΄μ μμ‘΄κ΄κ³λ₯Ό κ°μ§ κ²μΈμ§λ `DispatcherServlet` λ₯Ό μμ±νλ `MyApplicationInitializer` κ° μλμ κ°μ΄ κ²°μ νλ€.
    
    ```java
    public class MyWebApplicationInitializer implements WebApplicationInitializer {
        private static final Logger log = LoggerFactory.getLogger(MyWebApplicationInitializer.class);
    
        @Override
        public void onStartup(ServletContext servletContext) throws ServletException {
            AnnotationHandlerMapping ahm = new AnnotationHandlerMapping("next");
            ahm.initialize();
            ServletRegistration.Dynamic dispatcher = servletContext.addServlet("dispatcher", new DispatcherServlet(ahm));
            ...
    
        }
    }
    ```
    

## **π€ΌββοΈ @Component annotation μ§μ**

**λλ²μ§Έ μ΄μ**

: `ClasspathBeanDefinition` ν΄λμ€μ `@Component` annotation μ€μ λ§ μΆκ°νλ©΄ ν΄κ²°μ΄ κ°λ₯νλ€.

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Component {
    String value() default "";
}
```

μ΄μ  μΆκ°ν annotationμ `ClasspathBeanDefinitionScanner` μ `doScan()` λ©μλλ‘ μ°Ύμ μ μλλ‘ μΆκ°νλ€.

```java
public class ClasspathBeanDefinitionScanner {
    public void doScan(Object... basePackages) {
        Reflections reflections = new Reflections(basePackages);
        Set<Class<?>> beanClasses = getTypesAnnotatedWith(reflections, Controller.class, Service.class, Repository.class, Component.class);
        for (Class<?> clazz : beanClasses) {
            beanDefinitionRegistry.registerBeanDefinition(clazz, new BeanDefinition(clazz));
        }
    }
}
```

μ΄μ  μΆκ°ν annotationμ νμ©ν΄ `JdbcTemplate` μ λΉμΌλ‘ λ±λ‘ κ°λ₯νλλ‘ μ€μ  ν κ° DAOμμ `JdbcTemplate` μ DIλ‘ μ¬μ©νλλ‘ λ¦¬ν©ν λ§νλ€.

```java
@Component
public class JdbcTemplate {
    ...

}

@Repository
public class JdbcQuestionDao implements QuestionDao {
    private JdbcTemplate jdbcTemplate;
    
    @Inject
    public JdbcQuestionDao(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }
    ...

}

@Repository
public class JdbcAnswerDao implements AnswerDao {
    private JdbcTemplate jdbcTemplate;

    @Inject
    public JdbcAnswerDao(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }
    ...

}
```

μλ²λ₯Ό μμνλ©΄ `JdbcTemplate` μ΄ `"next"` ν¨ν€μ§κ° μλ `"core"` ν¨ν€μ§ μλμ μμ΄ μΈμνμ§ λͺ»ν΄ μλ¬κ° λ°μνλ€.

- `MyWebApplicationInitializer` μμ μ΄λ₯Ό μΈμνλλ‘ `AnnotationHandlerMapping` μ `"core"` λ₯Ό μΆκ°ν΄ μ λ¬νλ€.

```java
public class MyWebApplicationInitializer implements WebApplicationInitializer {
    private static final Logger log = LoggerFactory.getLogger(MyWebApplicationInitializer.class);

    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        AnnotationHandlerMapping ahm = new AnnotationHandlerMapping("core", "next");
        ...

    }
}
```

---

# 5. μΈλΆ λΌμ΄λΈλ¬λ¦¬ ν΄λμ€λ₯Ό λΉμΌλ‘ λ±λ‘νκΈ°

## 5-1. μκ΅¬μ¬ν­

- μ€μ  νμΌμ λΉ μΈμ€ν΄μ€λ₯Ό μμ±νλ λ©μλλ₯Ό κ΅¬νν΄ λκ³  μ΄λΈνμ΄μμΌλ‘ μ€μ 
- DI νλ μμν¬λ μ΄ μ€μ  νμΌμ μ½μ΄ **BeanFactory**μ λΉμΌλ‘ μ μ₯νμ¬ **ClasspathBeanDefinitionScanner**λ₯Ό ν΅ν΄ λ±λ‘ν λΉκ³Ό κ°μ μ μ₯μμμ κ΄λ¦¬
- μ€μ  νμΌμ μλ° ν΄λμ€λ‘ νκ³  λͺ¨λ  μ€μ μ μ΄λΈνμ΄μμ ν΅ν΄ κ΄λ¦¬
- λ³λμ μ€μ  νμΌμ λ§λ€λ©΄μ **ClasspathBeanDefinitionScanner**μμ μ¬μ©ν  κΈ°λ³Έ ν¨ν€μ§ μ€μ λ μ€μ  νμΌμμ ν  μ μλλ‘ κ΄λ¦¬

```java
@Configuration
@ComponentScan({ "next", "core" })
public class MyConfiguration {
    @Bean
    public DataSource dataSource() {
        BasicDataSource ds = new BasicDataSource();
        ds.setDriverClassName("org.h2.Driver");
        ds.setUrl("jdbc:h2:~/jwp-basic;AUTO_SERVER=TRUE");
        ds.setUsername("sa");
        ds.setPassword("");
        return ds;
    }

    @Bean
    public JdbcTemplate jdbcTemplate(DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
}
```

- `@Configuration`μΌλ‘ μλ° ν΄λμ€κ° μ€μ  νμΌμ΄λΌκ³  νμ
- `@Bean` μ΄λΈνμ΄μμΌλ‘ κ° λ©μλμμ μμ±νλ μΈμ€ν΄μ€κ° **BeanFactory**μ λΉμΌλ‘ λ±λ‘νλΌκ³  μ€μ 
- **ClasspathBeanDefinitionScanner**μμ μ¬μ©ν  κΈ°λ³Έ ν¨ν€μ§μ λν μ€μ μ νλ μ½λ©νλλ° μ€μ  νμΌμμ `@ComponentScan`μΌλ‘ μ€μ ν  μ μλλ‘ νλ κ²μ΄ λͺ©ν
- `@Configuration` μ€μ  νμΌμ ν΅ν΄ λ±λ‘ν λΉκ³Ό **ClasspathBeanDefinitionScanner**λ₯Ό ν΅ν΄ λ±λ‘ν λΉ κ°μλ DIκ° κ°λ₯νλλ‘ νλ κ²λ λͺ©ν

## 5-2. 1λ¨κ³ ννΈ

μ μκ΅¬μ¬ν­μ νμ€νΈν  μ μλ λ¨μ νμ€νΈλ₯Ό λ¨Όμ  κ΅¬ν

**AnnotatedBeanDefinitionReaderTest**λΌλ νμ€νΈ ν΄λμ€λ₯Ό λ§λ€κ³  μμ μ€μ  νμΌμ μ°Έκ³ ν΄ νμ€νΈμ© μ€μ  νμΌμ λ§λ λ€.

```java
@Target({ ElementType.TYPE })
@Retention(RetentionPolicy.RUNTIME)
public @interface ComponentScan {
    String[] value() default {};
    String[] basePackages() default {};
}
```

- `@ComponentScan`μ λ°°μ΄μ κ°μΌλ‘ μ λ¬ν  μ μλλ‘ μ½λ μΆκ°

- λ€μμΌλ‘ `@Configuration` μ€μ  νμΌμ μ½μ΄ **BeanFactory**μ **BeanDefinition**μ λ±λ‘νλ μ­ν μ νλ μλ‘μ΄ **AnnotatedBeanDefinitionReader** ν΄λμ€λ₯Ό μΆκ°ν΄μΌ ν¨.
    - λ©μλ λ°ν ν΄λμ€μ ν΄λΉνλ **BeanDefinition**μ μμ±ν ν, **BeanFactory**μ λ±λ‘
    - **BeanDefinition**μ μμνλ **AnnotatedBeanDefinition**μ μΆκ°ν ν λ©μλ μ λ³΄κΉμ§ κ°μ΄ μ λ¬
    - **BeanFactory**μ `getBean()` λ©μλλ μΈμλ‘ μ λ¬λλ λΉ ν΄λμ€μ ν΄λΉνλ **BeanDefinition**μ΄ **AnnotatedBeanDefinition** μΈμ€ν΄μ€μΌ κ²½μ° κΈ°μ‘΄κ³Ό λ€λ₯Έ λ°©μμΌλ‘ λΉ μΈμ€ν΄μ€λ₯Ό μμ±

**ApplicationContext**κ° **ClasspathBeanDefinitionScanner**μμ μ¬μ©ν  κΈ°λ³Έ ν¨ν€μ§ μ λ³΄λ₯Ό λ°λ κ²μ΄ μλλΌ `@Configuration`μ μ€μ λμ΄ μλ μ€μ  νμΌμ μΈμλ‘ λ°λλ‘ μμ ν ν `@ComponentScan` κ°μ κ΅¬ν΄ **ClasspathBeanDefinitionScanner**λ‘ μ λ¬νκ³  **AnnotatedBeanDefinitionReader**λ₯Ό ν΅ν΄ μ€μ  νμΌμ λ±λ‘λ `@Bean`μ **BeanFactory**μ λ±λ‘νλλ‘ κ΅¬νν΄μΌ ν¨

## 5-3. 2λ¨κ³ ννΈ

### 5-3-1. λ¨μ νμ€νΈ μ½λ λ§λ€κΈ°

```java
@Configuration
public class ExampleConfig {
    @Bean
    public DataSource dataSource() {
        BasicDataSource ds = new BasicDataSource();
        ds.setDriverClassName("org.h2.Driver");
        ds.setUrl("jdbc:h2:~/jwp-basic;AUTO_SERVER=TRUE");
        ds.setUsername("sa");
        ds.setPassword("");
        return ds;
    }
}
```

- src/test/javaμ νμ€νΈμ© μ€μ  νμΌ μΆκ°

```java
public class AnnotatedBeanDefinitionReaderTest {
    @Test
    public void register_simple() {
        BeanFactory beanFactory = new BeanFactory();
        AnnotatedBeanDefinitionReader abdr = new AnnotatedBeanDefinitionReader(beanFactory);
        abdr.register(ExampleConfig.class);
        beanFactory.initialize();

        assertNotNull(beanFactory.getBean(DataSource.class));
    }
}
```

- μ μ€μ  νμΌμ μ½μ΄ **BeanFactory**μ λ±λ‘ν ν `getBean()`μ ν΅ν΄ λΉμ μ°Ύμμ λ **null**μ΄ μλμ§ μ¬λΆλ₯Ό νλ¨νλ νμ€νΈ μΆκ°

### 5-3-2. BeanFactoryμ BeanDefinition μΆκ°

```java
public static Set<Method> getBeanMethods(Class<?> clazz, Class<? extends Annotation> annotation) {
        return getAllMethods(clazz, withAnnotation(annotation));
}
```

- `@Configuration` μ€μ  νμΌμμ `@Bean`μΌλ‘ μ€μ λμ΄ μλ λ©μλλ₯Ό λ¨Όμ  μ°Ύμ

```java
public class AnnotatedBeanDefinition extends BeanDefinition {
    private Method method;

    public AnnotatedBeanDefinition(Class<?> clazz, Method method) {
        super(clazz);
        this.method = method;
    }

    public Method getMethod() {
        return method;
    }
}
```

- λΉμ μμ±νλ €λ©΄ `@Bean`μ΄ μ€μ λμ΄ μλ λ©μλ μ λ³΄κ° νμ
- κΈ°μ‘΄μ **BeanDefinition**μ μμνλ **AnnotatedBeanDefinition**μ μΆκ°

### 5-3-3. λΉ μΈμ€ν΄μ€ μμ± λ° μμ‘΄κ΄κ³ μ£Όμ

**BeanFactory**μ **BeanDefinition**μ μΆκ°νμΌλ λΉ μΈμ€ν΄μ€ μμ±κ³Ό μμ‘΄κ΄κ³λ₯Ό μ£Όμν΄μΌ ν¨.

**BeanFactory**μ `getBean()` λ©μλμμ **AnnotatedBeanDefinition**μ κ³ λ €ν΄ κ°λ°

```java
public <T> T getBean(Class<T> clazz) {
        Object bean = beans.get(clazz);
        if (bean != null) {
            return (T) bean;
        }

        BeanDefinition beanDefinition = beanDefinitions.get(clazz);
        if (beanDefinition != null && beanDefinition instanceof AnnotatedBeanDefinition) {
            bean = createAnnotatedBean(beanDefinition);
						beans.put(clazz, bean);
		        return (T)bean;
        }
[...]
```

- μμ κ°μ΄ κ΅¬νν ν μμμ μΆκ°ν νμ€νΈκ° μ±κ³΅νλμ§ νμΈ
- μ€μ  νμΌμ λν νμ€νΈκ° λͺ¨λ μ±κ³΅νλ©΄ **ClasspathBeanDefinitionScanner**μμ ν΅ν© νμ€νΈλ₯Ό μ§ν β **ClasspathBeanDefinitionScanner**μμ μΆκ°ν λΉκ³Ό **AnnotatedBeanDefinitionReader**λ₯Ό ν΅ν΄ μΆκ°ν λΉ μ¬μ΄μ DIκ° κ°λ₯νμ§ νμ€νΈν  μ μλ λΉμ μΆκ°ν ν νμ€νΈλ₯Ό μ§ν

### 5-3-4. ApplicationContextκ° μ€μ  νμΌμ μ¬μ©νλλ‘ λ¦¬ν©ν λ§

```java
private Object[] findBasePackages(Class<?>[] annotatedClasses) {
        List<Object> basePackages = Lists.newArrayList();
        for (Class<?> annotatedClass : annotatedClasses) {
            ComponentScan componentScan = annotatedClass.getAnnotation(ComponentScan.class);
            if (componentScan == null) {
                continue;
            }
            for (String basePackage : componentScan.value()) {
                log.info("Component Scan basePackage : {}", basePackage);
            }
            basePackages.addAll(Arrays.asList(componentScan.value()));
        }
        return basePackages.toArray();
    }
```

- **ApplicationContext**μμ `@Configuration` μ€μ  νμΌμ μμ±μ μΈμλ‘ μ λ¬λ°μ μ μλλ‘ μμ νκ³  **ClasspathBeanDefinitionScanner**μμ μ¬μ©ν  κΈ°λ³Έ ν¨ν€μ§ μ λ³΄λ₯Ό `@ComponentScan`μμ μ°Ύμ
- **ClasspathBeanDefinitionScanner**μ **AnnotatedBeanDefinitionReader**λ₯Ό κ°μ΄ μ¬μ©νλλ‘ ν΅ν©

### 5-3-5. AnnotationHandlerMappingμ ApplicationContextλ₯Ό DI

**AnnotationHandlerMapping**μμ **ApplicationContext**λ₯Ό μ§μ  μμ±νκ³  μλλ° **ApplicationContext**λ **DI**νλλ‘ μμ 

```java
public class MyWebApplicationInitializer implements WebApplicationInitializer {
    private static final Logger log = LoggerFactory.getLogger(MyWebApplicationInitializer.class);

    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        ApplicationContext ac = new AnnotationConfigApplicationContext(MyConfiguration.class);
        AnnotationHandlerMapping ahm = new AnnotationHandlerMapping(ac);
        ahm.initialize();
        [...]
    }
}
```

- **ApplicationContext**λΌλ μΈν°νμ΄μ€λ₯Ό μΆμΆνκ³ 
- κΈ°μ‘΄ **ApplicationContext** ν΄λμ€λ₯Ό **AnnotationConfigApplicationContext**λ‘ μ΄λ¦ λ³κ²½

---

# 6. μ΄κΈ°ν κΈ°λ₯ μΆκ°

νμμλ μ½λλ₯Ό μ λ¦¬νλ μμμ νκΈ°μν΄ **ConnectionManager** ν΄λμ€λ₯Ό μ κ±°νλ©΄ μ»΄νμΌ μλ¬κ° λ°μνλ€.

**BeanFactory**μμ κ΄λ¦¬νλ **DataSource**λ₯Ό μ¬μ©νλλ‘ λ³κ²½νλ €λ©΄ **ContextLoaderListener**λ λΉμΌλ‘ λ±λ‘ν΄μΌ νλλ° λΉ λ±λ‘μ νλ©΄μ μ΄κΈ°ν μμμ΄ νμνλ€.

β **BeanFactory**μμ λΉ μΈμ€ν΄μ€λ₯Ό μμ±νκ³  **DI**κ° λλ ν μ΄κΈ°ν μμμ μ§ννλλ‘ κΈ°λ₯ μΆκ°

```java
@Target({ ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
public @interface PostConstruct {
}
```

- μ΄κΈ°νκ° νμν λ©μλμ μ€μ ν  μ μλλ‘ `@PostConstruct` μ΄λΈνμ΄μ μΆκ°

```java
@Component
public class DBInitializer {
    private static final Logger log = LoggerFactory.getLogger(DBInitializer.class);

    @Inject
    private DataSource dataSource;

    @PostConstruct
    public void initialize() {
        ResourceDatabasePopulator populator = new ResourceDatabasePopulator();
        populator.addScript(new ClassPathResource("jwp.sql"));
        DatabasePopulatorUtils.execute(populator, dataSource);

        log.info("Completed Load ServletContext!");
    }
}
```

- λ°μ΄ν°λ² μ΄μ€ μ΄κΈ°νλ₯Ό λ΄λΉνλ **ContextLoaderListener**λ₯Ό **DBInitializer**λ‘ μ΄λ¦ λ³κ²½
- `@PostConstruct`λ₯Ό μ€μ ν λ©μλκ° μ‘΄μ¬νλ©΄ λ©μλλ₯Ό μ€νν΄ μ΄κΈ°νν  μ μλλ‘ μ§μ

```java
public class BeanFactory implements BeanDefinitionRegistry {
    public <T> T getBean(Class<T> clazz) {
				[...]
        beanDefinition = beanDefinitions.get(concreteClazz.get());
        bean = inject(beanDefinition);
        beans.put(concreteClazz.get(), bean);
        initialize(bean, concreteClazz.get());
        return (T) bean;
    }

    private void initialize(Object bean, Class<?> beanClass) {
        Set<Method> initializeMethods = BeanFactoryUtils.getBeanMethods(beanClass, PostConstruct.class);
        if (initializeMethods.isEmpty()) {
            return;
        }
        for (Method initializeMethod : initializeMethods) {
            log.debug("@PostConstruct Initialize Method : {}", initializeMethod);
            BeanFactoryUtils.invokeMethod(initializeMethod, bean,
                    populateArguments(initializeMethod.getParameterTypes()));
        }
    }
}
```

- **BeanFactory**μ `getBean()`μμ `@PostConstruct` μ€μ  λ©μλλ₯Ό μ°Ύμ μ€ν

---

# 7. μΈν°νμ΄μ€, DI, DI μ»¨νμ΄λ

μ§κΈκΉμ§ MVC νλ μμν¬μ DI νλ μμν¬λ₯Ό κ΅¬ννλ κ³Όμ μ μ μ°μ±κ³Ό νμ₯μ±μ΄ νμν κ²½μ° μΈν°νμ΄μ€λ₯Ό μΆκ°νκ³  μ΄ μΈν°νμ΄μ€λ₯Ό κ΅¬ννλ κ΅¬νμ²΄λ₯Ό μΆκ°νλ λ°©μμΌλ‘ νμ₯νλ€.

μ€νλ§ νλ μμν¬κ° λ²μ μ μκ·Έλ μ΄λνλ©΄μ νμ λ²μ μ μ§μν  μ μλ μ΄μ  λν μ μ€κ³ν μΈν°νμ΄μ€μ μλ€.

```java
public interface BeanDefinitionReader {
    void loadBeanDefinitions(Class<?>... annotatedClasses);
}
```

- BeanDefinitionμ μμ±νκΈ° μν΄ μ΄λΈνμ΄μ μ€μ λ§μ μ§μνλ κ²μ΄ μλλΌ XML, Property μ€μ μ μ§μνλλ‘ νμ₯ν  μ μλ€.

```java
public interface BeanDefinitionRegistry {
    void registerBeanDefinition(Class<?> clazz, BeanDefinition beanDefinition);
}
```

- BeanDefinitionReaderμ λ³νμ μν₯μ λ°μ§ μμΌλ©΄μ λλ¦½μ μΌλ‘ νμ₯νλ κ²μ΄ κ°λ₯

```java
public interface BeanFactory {
    Set<Class<?>> getBeanClasses();
    <T> T getBean(Class<T> clazz);
    void clear();
}
```

```java
public interface BeanDefinition {
    Constructor<?> getInjectConstructor();
    Set<Field> getInjectFields();
    Class<?> getBeanClass();
    InjectType getResolvedInjectMode();
}
```

- BeanDefinition, BeanFactory λν μΈν°νμ΄μ€ κΈ°λ°μΌλ‘ νμ₯ κ°λ₯

νμ₯ νμν λΆλΆμ μΈν°νμ΄μ€λ‘ κ΅¬ννκ³  κ° μΈν°νμ΄μ€ μ¬μ΄μ μ°κ²°μ κ΅¬ν ν΄λμ€κ° μλ μΈν°νμ΄μ€λ₯Ό ν΅ν΄μλ§ μμ‘΄κ΄κ³λ₯Ό κ°μ§λλ‘ μ°κ²°ν¨μΌλ‘μ¨ μ μ°ν κ΅¬μ‘°λ₯Ό κ°μ§ μ μμ.

μλ‘ λ€λ₯΄κ² λ³ννλ κ΅¬ν ν΄λμ€ μ¬μ΄μ μ°κ²°μ μ΄ ν΄λμ€μ μ°κ²°μ λ΄λΉνλ factory ν΄λμ€λ₯Ό ν΅ν΄ λ΄λΉνλλ‘ κ΅¬νν¨μΌλ‘μ¨ κ°λ°μμ νΈμμ± νλ³΄ κ°λ₯

```java
public interface ApplicationContext {
    <T> T getBean(Class<T> clazz);
    Set<Class<?>> getBeanClasses();
}
```

- λνμ μΈ ν©ν λ¦¬ ν΄λμ€μΈ ApplicationContext λν κ΅¬ν ν΄λμ€μ μ°κ²° μ‘°ν©μ λ°λΌ λ€μν μ‘°ν©μ΄ κ°λ₯νκΈ° λλ¬Έμ μΈν°νμ΄μ€λ₯Ό μΆκ°ν ν νμ₯ κ°λ₯νλλ‘ κ΅¬ν κ°λ₯

```java
public class AnnotationConfigApplicationContext implements ApplicationContext {
    private static final Logger log = LoggerFactory.getLogger(AnnotationConfigApplicationContext.class);

    private DefaultBeanFactory beanFactory;

    public AnnotationConfigApplicationContext(Class<?>... annotatedClasses) {
        Object[] basePackages = findBasePackages(annotatedClasses);
        beanFactory = new DefaultBeanFactory();
        BeanDefinitionReader abdr = new AnnotatedBeanDefinitionReader(beanFactory);
        abdr.loadBeanDefinitions(annotatedClasses);

        if (basePackages.length > 0) {
            ClasspathBeanDefinitionScanner scanner = new ClasspathBeanDefinitionScanner(beanFactory);
            scanner.doScan(basePackages);
        }
        beanFactory.preInstantiateSinglonetons();
    }

    private Object[] findBasePackages(Class<?>[] annotatedClasses) {
        [...]
    }

    @Override
    public <T> T getBean(Class<T> clazz) {
        return beanFactory.getBean(clazz);
    }

    @Override
    public Set<Class<?>> getBeanClasses() {
        return beanFactory.getBeanClasses();
    }
}
```

- ApplicationContext μΈν°νμ΄μ€μ λν κ΅¬ν ν΄λμ€λ μ΄λΈνμ΄μ κΈ°λ°μΌλ‘ DI μ»¨νμ΄λλ₯Ό μμ±ν  κ²½μ°, AnnotationConfigApplicationContextλΌλ μ΄λ¦μΌλ‘ νμ₯ κ°λ₯
- AnnotationConfigApplicationContextλ κ° μΈν°νμ΄μ€μ λν κ΅¬νμ²΄λ₯Ό μ‘°ν©νλ μ­ν λ§ λ΄λΉνκ³ , μ΄λΈνμ΄μ μ€μ  κΈ°λ°μ DI μ»¨νμ΄λμ λν μ€μ§μ μΈ μμμ BeanFactoryμ λͺ¨λ μμνλ€.

---

# 8. μΉ μλ² λμμ ν΅ν μλΉμ€ μ΄μ

μλΉμ€μ μ μνλ μ¬μ©μκ° μλ κ²½μ°μλ ν°μΊ£ μλ²λ₯Ό μ’λ£νκ³  μμ€μ½λλ₯Ό λ°°ν¬νλ λ°©μμΌλ‘ μ΄μν΄λ λμ§λ§ μ¬μ©μκ° λ§μμ§ κ²½μ° μνμΉ, nignxμ κ°μ μΉμλ²λ₯Ό λμν΄ ν°μΊ£ μλ²μ ν΅ν© μ΄μν΄μΌ νλ€.

## 8-1. μκ΅¬μ¬ν­

μΉ μλ²λ₯Ό μΆκ°ν΄ ν°μΊ£ μλ²μ μ°κ²°ν ν μμ€μ½λ λ°°ν¬ μ€ μ κ² νμ΄μ§λ₯Ό λ³΄μ¬μ£Όλλ‘ κ°μ 

## 8-2. 1λ¨κ³ ννΈ

- nginxλ₯Ό μ€μΉνκ³  ν°μΊ£ μλ²μ μ°κ²°
- nginx μ€μ  νμΌλ‘ μ κ²μ© νμ΄μ§λ₯Ό μν μ€μ κ³Ό ν°μΊ£ μλ²μμ μ°κ²°μ λ΄λΉνλ μ€μ  2κ° κ΅¬ν
- μ κ³Όμ μ μλμΌλ‘ μ§νν΄ μ±κ³΅νλ©΄ λ€μ λ¨κ³λ μ μ€ν¬λ¦½νΈλ₯Ό ν΅ν΄ μλν

## 8-3. 2λ¨κ³ ννΈ

### 8-3-1. nginx μ€μΉ

- nginx μ€μΉ
- ps -ef | grep nginx λͺλ Ήμ ν΅ν΄ nginxκ° μ€ν μνμΈμ§ νμΈ
- curl http://localhost λͺλ Ήμ μ€νν΄ nginx νμ λ©μΈμ§λ₯Ό ν¬ν¨νλ HTML μλ΅νλμ§ νμΈ
- μΉ λΈλΌμ°μ μμ http://{μλ² IP μ£Όμ} λ‘ μ κ·Ό (μ κ·Όλμ§ μμΌλ©΄ 80 ν¬νΈμ λν λ°©νλ²½ μ€μ μ ν΄μ )

### 8-3-2. nginxμ ν°μΊ£ μλ² μ°κ²°

- /etc/nginx/sites-available λλ ν λ¦¬μ jwp-basic.confμ κ°μ μ΄λ¦μΌλ‘ νμΌ μμ± ν, λ΄μ© μ€μ 
- /etc/nginx/sites-enabled λλ ν λ¦¬μ default μ¬λ³Όλ¦­ λ§ν¬ μ­μ 
- /etc/nginx/sites-enabled λλ ν λ¦¬μ μμμ μμ±ν μ€μ  νμΌμ μ¬λ³Όλ¦­ λ§ν¬λ‘ μ€μ 
- sudo nginx -s reload λͺλ Ήμ μ€νν΄ nginx μ¬μμ
- μΉ λΈλΌμ°μ μμ http://{μλ² IP μ£Όμ} λ‘ μ κ·Ό

### 8-3-3. μ κ² νμ΄μ§ μ€λΉ λ° μλμΌλ‘ μ κ² νμ΄μ§ λ³κ²½

- μ κ² νμ΄μ§λ‘ μ¬μ©ν  HTML, CSS, μ΄λ―Έμ§ μ€λΉ
- /etc/nginx/sites-available λλ ν λ¦¬μ default μ€μ  νμΌμ jwp-basic-pm.confμ κ°μ μ΄λ¦μΌλ‘ λ³΅μ¬
- λ³΅μ¬ν μ€μ  νμΌμ root μ€μ μ μ κ² νμ΄μ§λ₯Ό μν λλ ν λ¦¬λ‘ λ³κ²½
- /etc/nginx/sites-enabled λλ ν λ¦¬μ μ¬λ³Όλ¦­ λ§ν¬ μ€μ μ μ­μ νκ³  μμμ μΆκ°ν μ€μ  νμΌμ μ¬λ³Όλ¦­ λ§ν¬ μ€μ 
- sudo nginx -s reload λͺλ Ήμ μ€νν΄ nginx μ¬μμ
- μΉ λΈλΌμ°μ μμ http://{μλ² IP μ£Όμ} λ‘ μ κ·Ό

### 8-3-4. μ μ€ν¬λ¦½νΈλ₯Ό ν΅ν λ°°ν¬ μλν

- deploy.sh μ€μ  νμΌμ λ€μ λ΄μ© μΆκ°
- /etc/nginx/sites-enabled λλ ν λ¦¬μ μ¬λ³Όλ¦­ λ§ν¬ μ€μ μ μ­μ νκ³  /etc/nginx/sites-available λλ ν λ¦¬μ μ κ²μ© μ€μ  νμΌλ‘ μ¬λ³Όλ¦­ λ§ν¬ λ€μ μ€μ 
- sudo nginx -s reload λͺλ Ήμ μ€νν΄ nginx μ¬μμ
- λ°°ν¬ μλ£νκ³  ν°μΊ£ μλ²κ° μ μμΈμ§ νμΈν ν, nignx μ€μ μ μ κ² νμ΄μ§μμ μλΉμ€ μ°κ²°λ‘ λ³κ²½νκΈ° μν΄ μ μ€ν¬λ¦½νΈ μμ±
- deploy-finish.shλΌλ μ΄λ¦μ μλ‘μ΄ μ μ€ν¬λ¦½νΈ μΆκ°
- /etc/nginx/sites-enabled λλ ν λ¦¬μ μ¬λ³Όλ¦­ λ§ν¬ μ€μ μ μ­μ νκ³  /etc/nginx/sites-available λλ ν λ¦¬μ μλΉμ€μ© μ€μ  νμΌλ‘ μ¬λ³Όλ¦­ λ§ν¬ λ€μ μ€μ 
- sudo nginx -s reload λͺλ Ήμ μ€νν΄ nginx μ¬μμ