## **커넥션풀과 데이터소스 이해**

![image](https://user-images.githubusercontent.com/79301439/207020350-acfbd276-a8ce-4149-bdb1-5611fe32e9da.png)

![image](https://user-images.githubusercontent.com/79301439/207020428-b9c34617-817f-4e1a-8226-85a761c52f0f.png)

```java
import com.zaxxer.hikari.HikariDataSource;

@Test
void dataSourceConnectionPool() throws SQLException, InterruptedException {
    //커넥션 풀링
    HikariDataSource dataSource = new HikariDataSource();
    dataSource.setJdbcUrl(URL);
    dataSource.setUsername(USERNAME);
    dataSource.setPassword(PASSWORD);
    dataSource.setMaximumPoolSize(10);
    dataSource.setPoolName("MyPool");

    useDataSource(dataSource);
    Thread.sleep(1000);
}
```

![image](https://user-images.githubusercontent.com/79301439/207020692-7ffd24d2-419c-4858-966b-f3553ac26ffb.png)

![image](https://user-images.githubusercontent.com/79301439/207020982-c08a5889-1e20-4312-a305-70b2e683cb02.png)

![image](https://user-images.githubusercontent.com/79301439/207021039-8125ac8b-5eb1-405e-91eb-befe108b4970.png)

![image](https://user-images.githubusercontent.com/79301439/207021182-1f5e85c2-a9c8-4d81-b22f-3c2551c3a81f.png)
