## **JDBC 이해**

![image](https://user-images.githubusercontent.com/79301439/206371412-97e4fc3c-1e81-41f6-94e3-0eabc59cc4c0.png)

```java
public Member findById(String memberId) throws SQLException {
    String sql = "select * from member where member_id = ?";

    Connection con = null;
    PreparedStatement pstmt = null;
    ResultSet rs = null;

    try {
        con = getConnection();
        pstmt = con.prepareStatement(sql);
        pstmt.setString(1, memberId);

        rs = pstmt.executeQuery();
        if (rs.next()) {
            Member member = new Member();
            member.setMemberId(rs.getString("member_id"));
            member.setMoney(rs.getInt("money"));
            return member;
        } else {
            throw new NoSuchElementException("member not found memberId=" + memberId);
        }

    } catch (SQLException e) {
        log.error("db error", e);
        throw e;
    } finally {
        close(con, pstmt, rs);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/206371746-2088275d-cf51-4be7-b058-d96d0049d748.png)

![image](https://user-images.githubusercontent.com/79301439/206371820-ca351e37-4fa5-4aa2-813f-996d353c5f5c.png)

![image](https://user-images.githubusercontent.com/79301439/206371986-d5c3c717-fc53-4644-8e50-1d1fbaf5275f.png)

![image](https://user-images.githubusercontent.com/79301439/206372065-c0131cb2-a87d-4aa1-ade2-737b57d32be3.png)

```java
package hello.jdbc.repository;

import hello.jdbc.domain.Member;
import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;

import java.sql.SQLException;

import static org.assertj.core.api.Assertions.*;

@Slf4j
class MemberRepositoryV0Test {

    MemberRepositoryV0 repository = new MemberRepositoryV0();

    @Test
    void crud() throws SQLException {
        //save
        Member member = new Member("memberV3", 10000);
        repository.save(member);

        //findById
        Member findMember = repository.findById(member.getMemberId());
        log.info("findMember={}", findMember);
        log.info("member == findMember {}", member == findMember);
        log.info("member equals findMember {}", member.equals(findMember));
        assertThat(findMember).isEqualTo(member);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/206372220-a8242f5a-0a2d-4b05-b0bf-9f907b5b2bdb.png)
