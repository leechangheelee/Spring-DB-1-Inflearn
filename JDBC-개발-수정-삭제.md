## **JDBC 이해**

![image](https://user-images.githubusercontent.com/79301439/206396507-1652aad1-b7fc-4941-93ff-cfc18204a7c4.png)

```java
public void update(String memberId, int money) throws SQLException {
    String sql = "update member set money=? where member_id=?";

    Connection con = null;
    PreparedStatement pstmt = null;

    try {
        con = getConnection();
        pstmt = con.prepareStatement(sql);
        pstmt.setInt(1, money);
        pstmt.setString(2, memberId);
        int resultSize = pstmt.executeUpdate();
        log.info("resultSize={}", resultSize);
    } catch (SQLException e) {
        log.error("db error", e);
        throw e;
    } finally {
        close(con, pstmt, null);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/206396677-039b3149-f00c-48ac-b120-ae84d8369c10.png)

```java
@Test
void crud() throws SQLException {
    //save
    Member member = new Member("memberV100", 10000);
    repository.save(member);

    //findById
    Member findMember = repository.findById(member.getMemberId());
    log.info("findMember={}", findMember);
    assertThat(findMember).isEqualTo(member);

    //update: money: 10000 → 20000
    repository.update(member.getMemberId(), 20000);
    Member updatedMember = repository.findById(member.getMemberId());
    assertThat(updatedMember.getMoney()).isEqualTo(20000);

    //delete
    repository.delete(member.getMemberId());
    assertThatThrownBy(() -> repository.findById(member.getMemberId()))
            .isInstanceOf(NoSuchElementException.class);
}
```

![image](https://user-images.githubusercontent.com/79301439/206396822-418ff00f-3c52-4e8c-a334-569c25b0e5f0.png)

![image](https://user-images.githubusercontent.com/79301439/206396902-67bc2c89-72aa-4fa3-a263-5f287a5bb85c.png)

```java
public void delete(String memberId) throws SQLException {
    String sql = "delete from member where member_id=?";

    Connection con = null;
    PreparedStatement pstmt = null;

    try {
        con = getConnection();
        pstmt = con.prepareStatement(sql);
        pstmt.setString(1, memberId);
        pstmt.executeUpdate();
    } catch (SQLException e) {
        log.error("db error", e);
        throw e;
    } finally {
        close(con, pstmt, null);
    }

}
```

![image](https://user-images.githubusercontent.com/79301439/206397053-a429ab67-6ac9-4fee-b17b-51c03a025fb8.png)

```java
@Test
void crud() throws SQLException {
    //save
    Member member = new Member("memberV100", 10000);
    repository.save(member);

    //findById
    Member findMember = repository.findById(member.getMemberId());
    log.info("findMember={}", findMember);
    assertThat(findMember).isEqualTo(member);

    //update: money: 10000 → 20000
    repository.update(member.getMemberId(), 20000);
    Member updatedMember = repository.findById(member.getMemberId());
    assertThat(updatedMember.getMoney()).isEqualTo(20000);

    //delete
    repository.delete(member.getMemberId());
    assertThatThrownBy(() -> repository.findById(member.getMemberId()))
            .isInstanceOf(NoSuchElementException.class);
}
```

![image](https://user-images.githubusercontent.com/79301439/206397163-056322a6-5222-4724-91ae-c19ce8dce08c.png)
