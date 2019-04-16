##### list 参数

> [stackoverflow.com](https://stackoverflow.com/questions/3428742/how-to-use-annotations-with-ibatis-mybatis-for-an-in-query/29076097)
- Solution 1: Use @SelectProvider
- Solution 2: Extend LanguageDriver which will always compile sql to DynamicSqlSource. However, you still have to write \" everywhere.
- Solution 3: Extend LanguageDriver which can convert your own grammar to mybatis one.
- Solution 4: Write your own LanguageDriver which compile SQL with some template renderer, just like mybatis-velocity project does. In this way, you can even integrate groovy.

- Solution3 :
```
public class MybatisExtendedLanguageDriver extends XMLLanguageDriver 
                                           implements LanguageDriver {
    private final Pattern inPattern = Pattern.compile("\\(#\\{(\\w+)\\}\\)");
    public SqlSource createSqlSource(Configuration configuration, String script, Class<?> parameterType) {
        Matcher matcher = inPattern.matcher(script);
        if (matcher.find()) {
            script = matcher.replaceAll("(<foreach collection=\"$1\" item=\"__item\" separator=\",\" >#{__item}</foreach>)");
        }
        script = "<script>" + script + "</script>";
        return super.createSqlSource(configuration, script, parameterType);
    }
}
```
- usage
```
@Lang(MybatisExtendedLanguageDriver.class)
@Select("SELECT " + COLUMNS + " FROM sometable where id IN (#{ids})")
List<SomeItem> loadByIds(@Param("ids") List<Integer> ids);
```
