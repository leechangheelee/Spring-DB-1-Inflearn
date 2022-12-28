## **스프링과 문제 해결 - 예외 처리, 반복**

![image](https://user-images.githubusercontent.com/79301439/209795264-351d6d73-fd3c-40bb-92a8-2fa1b194faec.png)

![image](https://user-images.githubusercontent.com/79301439/209795295-a11df079-a254-465e-a4c9-51d90d0a95bb.png)

```java
package hello.jdbc.repository;

import hello.jdbc.domain.Member;
import lombok.extern.slf4j.Slf4j;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;

import javax.sql.DataSource;

/**
 * JdbcTemplate 사용
 */
@Slf4j
public class MemberRepositoryV5 implements MemberRepository {

    private final JdbcTemplate template;

    public MemberRepositoryV5(DataSource dataSource) {
        this.template = new JdbcTemplate(dataSource);
    }

    @Override
    public Member save(Member member) {
        String sql = "insert into member(member_id, money) values (?, ?)";
        template.update(sql, member.getMemberId(), member.getMoney());
        return member;
    }

    @Override
    public Member findById(String memberId) {
        String sql = "select * from member where member_id = ?";
        return template.queryForObject(sql, memberRowMapper(), memberId);
    }

    private RowMapper<Member> memberRowMapper() {
        return (rs, rowNum) -> {
            Member member = new Member();
            member.setMemberId(rs.getString("member_id"));
            member.setMoney(rs.getInt("money"));
            return member;
        };
    }

    @Override
    public void update(String memberId, int money) {
        String sql = "update member set money=? where member_id=?";
        template.update(sql, money, memberId);
    }

    @Override
    public void delete(String memberId) {
        String sql = "delete from member where member_id=?";
        template.update(sql, memberId);
    }

}
```

![image](https://user-images.githubusercontent.com/79301439/209795485-0f5367a4-52c4-43f8-b81b-7fc8c6f76a2e.png)

![image](https://user-images.githubusercontent.com/79301439/209795531-accae409-9de0-4ada-94e1-98088ccc22f9.png)
