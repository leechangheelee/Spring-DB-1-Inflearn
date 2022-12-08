## **JDBC 이해**

![image](https://user-images.githubusercontent.com/79301439/206337539-f70d0638-f1ac-4c00-86cb-fc7b939c6921.png)

![image](https://user-images.githubusercontent.com/79301439/206337688-1bad38d6-2acc-4dae-b00e-7ef68c2fcaef.png)

```java
package hello.jdbc.domain;

import lombok.Data;

@Data
public class Member {

    private String memberId;
    private  int money;

    public Member() {
    }

    public Member(String memberId, int money) {
        this.memberId = memberId;
        this.money = money;
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/206337906-89602534-f0e6-45b5-a4d5-cf7246cfdcf8.png)

```java
package hello.jdbc.repository;

import hello.jdbc.connection.DBConnectionUtil;
import hello.jdbc.domain.Member;
import lombok.extern.slf4j.Slf4j;

import java.sql.*;

/**
 * JDBC - DriverManager 사용
 */
@Slf4j
public class MemberRepositoryV0 {

    public Member save(Member member) throws SQLException {
        String sql = "insert into member(member_id, money) values (?, ?)";

        Connection con = null;
        PreparedStatement pstmt = null;

        try {
            con = getConnection();
            pstmt = con.prepareStatement(sql);
            pstmt.setString(1, member.getMemberId());
            pstmt.setInt(2, member.getMoney());
            pstmt.executeUpdate();
            return member;
        } catch (SQLException e) {
            log.error("db error", e);
            throw e;
        } finally {
            close(con, pstmt, null);
        }
    }

    private void close(Connection con, Statement stmt, ResultSet rs) {

        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                log.info("error", e);
            }
        }

        if (stmt != null) {
            try {
                stmt.close(); //SQLException, 여기서 Exception 터져도 아래 구문에서 con 닫을 수 있음
            } catch (SQLException e) {
                log.info("error", e);
            }
        }

        if (con != null) {
            try {
                con.close();
            } catch (SQLException e) {
                log.info("error", e);
            }
        }

    }

    private Connection getConnection() {
        return DBConnectionUtil.getConnection();
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/206338021-58595617-6176-40b3-b26a-32c0f731c5d3.png)

![image](https://user-images.githubusercontent.com/79301439/206338103-3e4276d1-8e43-4020-8832-db95fec52825.png)

![image](https://user-images.githubusercontent.com/79301439/206338135-cac5e8b5-9487-4172-8517-5e762b3ff820.png)

```java
package hello.jdbc.repository;

import hello.jdbc.domain.Member;
import org.junit.jupiter.api.Test;

import java.sql.SQLException;

import static org.junit.jupiter.api.Assertions.*;

class MemberRepositoryV0Test {

    MemberRepositoryV0 repository = new MemberRepositoryV0();

    @Test
    void crud() throws SQLException {
        Member member = new Member("memberV0", 10000);
        repository.save(member);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/206338221-52f90afd-3b08-43ba-bd18-76d64cae2bb9.png)
