# thymeleaf

>thymeleaf和layuimini报错org.thymeleaf.exceptions.TemplateProcessingException: Could not parse as expres

```apl
因为[[]]一般用于在script中使用thymeleaf表达式，所以这里会有冲突。
解决办法：
法一：在两个方括号中间加一个空格
法二：在< script > 标签中添加了 th:inline=“none” 属性，禁用内联表达式
```

