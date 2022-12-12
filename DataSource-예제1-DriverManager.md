## **커넥션풀과 데이터소스 이해**

![image](https://user-images.githubusercontent.com/79301439/207010551-1758421b-d4be-4511-8fa0-ddc5df9c69e0.png)

```java
package hello.jdbc.connection;

import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

import static hello.jdbc.connection.ConnectionConst.*;

@Slf4j
public class ConnectionTest {

    @Test
    void driverManager() throws SQLException {
        Connection con1 = DriverManager.getConnection(URL, USERNAME, PASSWORD);
        Connection con2 = DriverManager.getConnection(URL, USERNAME, PASSWORD);
        log.info("connection={}, class={}", con1, con1.getClass());
        log.info("connection={}, class={}", con2, con2.getClass());
    }
    
}
```

![image](https://user-images.githubusercontent.com/79301439/207010944-2bb22e72-11dd-46dc-a200-247965439619.png)

![image](https://user-images.githubusercontent.com/79301439/207011025-d7de2efd-eb89-4fd8-bc1e-509c121d808d.png)

```java
package hello.jdbc.connection;

import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

import static hello.jdbc.connection.ConnectionConst.*;

@Slf4j
public class ConnectionTest {

    @Test
    void driverManager() throws SQLException {
        Connection con1 = DriverManager.getConnection(URL, USERNAME, PASSWORD);
        Connection con2 = DriverManager.getConnection(URL, USERNAME, PASSWORD);
        log.info("connection={}, class={}", con1, con1.getClass());
        log.info("connection={}, class={}", con2, con2.getClass());
    }

    @Test
    void dataSourceDriverManager() throws SQLException {
        //DriverManagerDataSource - 항상 새로운 커넥션을 획득
        DataSource dataSource = new DriverManagerDataSource(URL, USERNAME, PASSWORD);
        useDataSource(dataSource);
    }

    private void useDataSource(DataSource dataSource) throws SQLException {
        Connection con1 = dataSource.getConnection();
        Connection con2 = dataSource.getConnection();
        log.info("connection={}, class={}", con1, con1.getClass());
        log.info("connection={}, class={}", con2, con2.getClass());
    }

}
```

![image](https://user-images.githubusercontent.com/79301439/207011177-184017a3-7864-4120-9ce7-b9729b82799a.png)

![image](https://user-images.githubusercontent.com/79301439/207011255-d6c1cd99-27d0-48e1-8556-2f74c3f0ab55.png)

![image](https://user-images.githubusercontent.com/79301439/207011416-51d182fd-e503-433b-b03a-6fab2e11737b.png)

![image](https://user-images.githubusercontent.com/79301439/207011491-89763eae-67f2-462f-9f0d-e0a7c7251a5d.png)
