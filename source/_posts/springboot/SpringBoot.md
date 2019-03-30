---
title: Spring Boot启动过程源码阅读
date: 2019-03-03 12:04:26
categories: 源码
tags: [Spring Boot,源码]
---

### 入口

1. 代码部分

   ```java
   @SpringBootApplication
   public class WebApplication {
       public static void main(String[] args) {
           SpringApplication.run(WebApplication.class, args);
       }
   }
   ```

   

2. 注解`@SpringBootApplication`

   ```java
   @Target(ElementType.TYPE)
   @Retention(RetentionPolicy.RUNTIME)
   @Documented
   @Inherited
   @SpringBootConfiguration
   @EnableAutoConfiguration
   @ComponentScan(excludeFilters = {
   		@Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
   		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
   public @interface SpringBootApplication {
   
   	@AliasFor(annotation = EnableAutoConfiguration.class)
   	Class<?>[] exclude() default {};
   
   	@AliasFor(annotation = EnableAutoConfiguration.class)
   	String[] excludeName() default {};
   
   	@AliasFor(annotation = ComponentScan.class, attribute = "basePackages")
   	String[] scanBasePackages() default {};
   
   	@AliasFor(annotation = ComponentScan.class, attribute = "basePackageClasses")
   	Class<?>[] scanBasePackageClasses() default {};
   
   }
   ```

   元注解说明

   - `@Target`注解类型
   - `@Retention`注解生命周期，包括source，class，runtime
   - `@Documented`注解被javadoc收录
   - `@Inherited` 表示被`@SpringBootApplication`修饰的类，其子类可以继承`@SpringBootApplication`注解

   框架定义的注解

   - `@SpringBootConfiguration`同`@Configuration`注解，对应xml中的beans标签
   - `@EnableAutoConfiguration`启用SpringBoot自动配置上下文，自动扫描加载子目录下的bean
   - `@ComponentScan`



### 入口实现

1. 构造SpringApplication代码

   ```java
   public static ConfigurableApplicationContext run(Class<?> primarySource,
                                                    String... args) {
       return run(new Class<?>[] { primarySource }, args);
   }
   public static ConfigurableApplicationContext run(Class<?>[] primarySources,
                                                    String[] args) {
       return new SpringApplication(primarySources).run(args);
   }
   
   //构造方法，构造SpringApplication
   @SuppressWarnings({ "unchecked", "rawtypes" })
   public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
       //设置资源加载类，null
       this.resourceLoader = resourceLoader;
       Assert.notNull(primarySources, "PrimarySources must not be null");
       //初始化主要资源集
       this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
       //判断使用的是Servlet模式，WebFlux模式或者NONE
       this.webApplicationType = WebApplicationType.deduceFromClasspath();
       //设置需要初始化组件
       setInitializers((Collection) getSpringFactoriesInstances(
           ApplicationContextInitializer.class));
       //设置监听器
       setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
       //推断主入口类
       this.mainApplicationClass = deduceMainApplicationClass();
   }
   ```

   

2. 启动SpringApplication

   ```java
   public ConfigurableApplicationContext run(String... args) {
       //启动计时器
       StopWatch stopWatch = new StopWatch();
       stopWatch.start();
       ConfigurableApplicationContext context = null;
       Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList<>();
       //设置java.awt.headless参数（无界面）
       configureHeadlessProperty();
       //加载并启动监听器（初始官方默认就EventPublishListener）
       SpringApplicationRunListeners listeners = getRunListeners(args);
       listeners.starting();
       try {
           //加载启动参数
           ApplicationArguments applicationArguments = new DefaultApplicationArguments(
               args);
           //准备环境
           ConfigurableEnvironment environment = prepareEnvironment(listeners,
   applicationArguments);
           //
           configureIgnoreBeanInfo(environment);
           //打印banner图标
           Banner printedBanner = printBanner(environment);
           context = createApplicationContext();
           exceptionReporters = getSpringFactoriesInstances(
               SpringBootExceptionReporter.class,
               new Class[] { ConfigurableApplicationContext.class }, context);
           //准备上下文：
           prepareContext(context, environment, listeners, applicationArguments,
                          printedBanner);
           //刷新上下文
           refreshContext(context);
           //
           afterRefresh(context, applicationArguments);
           //计时结束
           stopWatch.stop();
           if (this.logStartupInfo) {
               new StartupInfoLogger(this.mainApplicationClass)
                   .logStarted(getApplicationLog(), stopWatch);
           }
           listeners.started(context);
           callRunners(context, applicationArguments);
       }
       catch (Throwable ex) {
           handleRunFailure(context, ex, exceptionReporters, listeners);
           throw new IllegalStateException(ex);
       }
   
       try {
           listeners.running(context);
       }
       catch (Throwable ex) {
           handleRunFailure(context, ex, exceptionReporters, null);
           throw new IllegalStateException(ex);
       }
       return context;
   }
   ```

   

3. 准备上下文

   ```java
   private void prepareContext(ConfigurableApplicationContext context,
   			ConfigurableEnvironment environment, SpringApplicationRunListeners listeners,
   			ApplicationArguments applicationArguments, Banner printedBanner) {
       //设置环境参数：激活哪个profiles，加载application.properties下配置的Property
       context.setEnvironment(environment);
       postProcessApplicationContext(context);
       applyInitializers(context);
       listeners.contextPrepared(context);
       if (this.logStartupInfo) {
           logStartupInfo(context.getParent() == null);
           logStartupProfileInfo(context);
       }
       // Add boot specific singleton beans
       ConfigurableListableBeanFactory beanFactory = context.getBeanFactory();
       beanFactory.registerSingleton("springApplicationArguments", applicationArguments);
       if (printedBanner != null) {
           beanFactory.registerSingleton("springBootBanner", printedBanner);
       }
       if (beanFactory instanceof DefaultListableBeanFactory) {
           ((DefaultListableBeanFactory) beanFactory)
           .setAllowBeanDefinitionOverriding(this.allowBeanDefinitionOverriding);
       }
       // Load the sources
       Set<Object> sources = getAllSources();
       Assert.notEmpty(sources, "Sources must not be empty");
       load(context, sources.toArray(new Object[0]));
       listeners.contextLoaded(context);
   }
   ```

   





