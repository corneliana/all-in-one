
## StringBuilder
```java
package src;  
  
public class Main {  
    public static void main(String[] args) {  
        String[] fields = { "name", "position", "salary" };  
        String table = "employee";  
        String insert = buildInsertSql(table, fields);  
        System.out.println(insert);  
        String s = "INSERT INTO employee (name, position, salary) VALUES (?, ?, ?)";  
        System.out.println(s.equals(insert) ? "测试成功" : "测试失败");  
    }  
  
    static String buildInsertSql(String table, String[] fields) {  
        // TODO:  
        StringBuilder sb = new StringBuilder();  
        StringBuilder sql = sb.append("INSERT INTO ").append(table).append(" (");  
        StringBuilder values = new StringBuilder(") VALUES (");  
  
        for (int i = 0; i < fields.length; i++) {  
            sql.append(fields[i]);  
            values.append("?");  
  
            if (i < fields.length - 1) {  
                sql.append(", ");  
                values.append(", ");  
            }  
        }  
        return sql.append(values).append(')').toString();  
    }  
}
```

## StringJoiner
```java
package src;
  
import java.util.StringJoiner;  
  
public class Main {  
    public static void main(String[] args) {  
        String[] fields = { "name", "position", "salary" };  
        String table = "employee";  
        String select = buildSelectSql(table, fields);  
        System.out.println(select);  
        System.out.println("SELECT name, position, salary FROM employee".equals(select) ? "测试成功" : "测试失败");  
    }  
  
    static String buildSelectSql(String table, String[] fields) {  
        // TODO:  
        StringJoiner joiner = new StringJoiner(", ", "SELECT ", " FROM " + table);  
        for (String field : fields) {  
            joiner.add(field);  
        }  
        return joiner.toString();  
    }  
}
```

## Wrapper Class
