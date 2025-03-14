# SQL Injection 분석 보고서

## 개요
SQL 인젝션(SQL Injection)은 웹 애플리케이션의 보안 취약점을 악용하여 데이터베이스에 악의적인 SQL 쿼리를 삽입하는 공격 기법입니다. 이 보고서에서는 `1 UNION SELECT md5()`를 이용한 MySQL DB에 대한 SQL 인젝션 공격의 원리, 공격 과정, 그리고 방어 방법에 대해 설명합니다.

## SQL Injection 원리
SQL 인젝션은 사용자가 입력할 수 있는 필드에 악의적인 SQL 코드를 삽입하여 데이터베이스에서 비정상적인 동작을 유도합니다. 이를 통해 공격자는 데이터베이스의 데이터를 유출하거나, 데이터를 변경하는 등의 행위를 할 수 있습니다.

## 공격 과정
`1 UNION SELECT md5()`를 이용한 SQL 인젝션 공격은 다음과 같은 단계로 수행됩니다:

1. **취약점 탐지**:
   - 웹 애플리케이션의 입력 필드에 SQL 인젝션이 가능한지 확인합니다.
   - 예를 들어, 다음과 같은 쿼리를 삽입하여 응답을 확인합니다:
     ```sql
     ' OR 1=1 --
     ```

2. **악의적인 쿼리 삽입**:
   - SQL 인젝션이 가능한 입력 필드에 악의적인 쿼리를 삽입합니다.
   - 예를 들어, 다음과 같은 쿼리를 삽입하여 데이터베이스의 해시값을 얻어냅니다:
     ```sql
     1 UNION SELECT md5('password')
     ```

3. **쿼리 실행 및 결과 확인**:
   - 삽입된 쿼리가 실행되면, 데이터베이스에서 반환된 결과를 확인합니다.
   - 예를 들어, 'password' 문자열의 MD5 해시값이 반환됩니다.

## 방어 방법
SQL 인젝션 공격을 방어하기 위해 다음과 같은 방법을 사용할 수 있습니다:

1. **입력 값 검증**:
   - 사용자가 입력한 값을 철저히 검증하고, 예상치 못한 값은 거부합니다.

2. **준비된 문(Prepared Statements) 사용**:
   - SQL 쿼리를 작성할 때 준비된 문을 사용하여 입력 값을 안전하게 처리합니다.
   - 예를 들어, PHP에서 준비된 문을 사용하는 방법은 다음과 같습니다:
     ```php
     $stmt = $conn->prepare("SELECT * FROM users WHERE username = ?");
     $stmt->bind_param("s", $username);
     $stmt->execute();
     ```

3. **ORM 사용**:
   - 객체 관계 매핑(Object-Relational Mapping, ORM) 프레임워크를 사용하여 SQL 쿼리를 자동으로 생성하고 관리합니다.

## 결론
SQL 인젝션은 매우 위험한 공격 기법으로, 이를 방어하기 위해 적절한 입력 값 검증과 준비된 문 사용 등 다양한 보안 조치가 필요합니다. 이를 통해 웹 애플리케이션의 보안을 강화할 수 있습니다.

## 참고 자료
- [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [MySQL Documentation](https://dev.mysql.com/doc/)

---

