# 4. 효율적 쿼리
## 서브 쿼리와 조인
**서브 쿼리**
- 다른 SQL문이 포함된 SQL문
**JOIN**
- 2개의 테이블을 하나로 합치는 것
**여러 테이블에 질의하는 등 데이터베이스에 복잡한 요청할 때 유용**
### 여러 테이블에 질의하기
- 실제 데이터베이스를 다룰 때는 **여러 테이블을 대상으로 작업하는 것이 일반적**
### 서브 쿼리
- 내부에 다른 SQL문이 포함된 SQL문
	- `SELECT`문 안에 `SELECT`문이 포함된 서브쿼리
	- `DELETE`문 안에 `SELECT`문이 포함된 서브쿼리
### 조인
- 여러 테이블을 하나로 합치는 것
	![조인](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F26242F48569460C734)
#### INNER JOIN
- 가장 일반적인 조인
```SQL
SELECT <열 목록> 
FROM <첫 번째 테이블>     
	INNER JOIN <두 번째 테이블>     
	ON <조인 조건> 
[WHERE 검색 조건]  #INNER JOIN을 JOIN이라고만 써도 INNER JOIN으로 인식합니다.
```
#### OUTER JOIN
- 내부 조인은 두 테이블에 모두 데이터가 있어야만 결과가 나오지만, 외부 조인은 한쪽에만 데이터가 있어도 결과가 나온다.
```SQL
SELECT <열 목록> 
FROM <첫 번째 테이블(LEFT 테이블)>     
	<LEFT | RIGHT | FULL> OUTER JOIN <두 번째 테이블(RIGHT 테이블)>      
	ON <조인 조건> [WHERE 검색 조건]
```
- **LEFT** OUTER JOIN: 왼쪽 테이블의 모든 값이 출력되는 조인
- **RIGHT** OUTER JOIN: 오른쪽 테이블의 모든 값이 출력되는 조인
- **FULL** OUTER JOIN: 왼쪽 외부 조인과 오른쪽 외부 조인이 합쳐진 것
	![outer join](https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/OUTER-JOIN_%EB%8D%94%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-600x600.png)
#### CROSS JOIN (상호 조인)
- 한쪽 테이블의 모든 행과 다른 쪽 테이블의 모든 행을 조인시키는 기능
- 상호 조인 결과의 전체 행 개수는 두 테이블의 각 행의 개수를 곱한 수만큼 
- Cartesian Product라고도 한다.
```sQL
SELECT * 
	FROM <첫 번째 테이블>     
	CROSS JOIN <두 번째 테이블>
```
![cross join](https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/%ED%98%BC%EC%9E%90-%EA%B3%B5%EB%B6%80%ED%95%98%EB%8A%94-SQL_CROSS-JOIN-600x600.png)
#### SELF JOIN(자체 조인)
- 자체 조인은 자기 자신과 조인하므로 1개의 테이블을 사용
- 별도의 문법이 있는 것은 아니고 1개로 조인하면 자체 조인
```sQL
SELECT <열 목록> 
FROM <테이블> 별칭A     
	INNER JOIN <테이블> 별칭B 
[WHERE 검색 조건]
```
## 뷰
- `SELECT`문의 결과를 토대로 SQL문을 자주 실행할 때, 매번 서브쿼리 작성하기에는 중복되는 쿼리가 많아 번거롭다 → `VIEW` 사용
**VIEW**
- `SELECT` 문의 결과로 만들어진 가상의 테이블
- `SELECT` 문의 결과를 뷰로 생성한 뒤, 해당 뷰에 다양한 SQL 실행 가능
- 쿼리의 단순화, 재사용성을 높이기 위해 뷰를 많이 사용
- 특정 사용자에게 테이블의 특정 데이터만을 보여주고자 할 때도 `VIEW` 사용
	- 테이블 상의 노출 가능한 데이터만 포함하는 뷰를 만들어 특정 사용자에게 권한 부여 (`GRANT`)
- `VIEW`에 대한 조회(`SELECT`)는 제한이 없으나, `INSERT`, `UPDATE`, `DELETE` 등은 불가능 할 수 있다
	- `VIEW`는 조회를 목적으로 사용되는 경우가 많다
```SQL
CREATE VIEW 뷰 이름
AS
	SELECT 문 ;
```
## 인덱스
### 인덱스
- RDBMS의 성능을 향상시키는 가장 대중적인 방법
- 검색 속도 향상을 목적으로 만드는 하나 이상의 테이블 필드에 대한 자료 구조
	- 책의 찾아보기 (인덱스)와 유사한 기능
- 특정 필드에 대한 인덱스 생성하면, 인덱스를 기준으로 원하는 레코드에 더 빠르게 접근 가능
	- 특정 필드에 인덱스 생성 = 해당 필드를 기준으로 레코드를 조회하겠다
	- 인덱스가 없는 DB에서는 최악의 경우 원하는 레코드 조회 위해 모든 레코드 찾아야 할 수 있다
- 한 테이블에 2개의 인덱스 가능
- 테이블 삭제되면 인덱스도 삭제
- **데이터베이스에서 실제 데이터가 저장되는 곳**: **Heap 영역**
#### 인덱스 장단점
##### 장점
- 원하는 데이터 Full Scan 하지 않고도 빠르게 검색 가능
- 쿼리 부하 감소 → 시스템 전체 성능 향상
##### 단점
- 인덱스 생성 위해 데이터베이스의 약 10% 추가 공간 필요
- 인덱스는 항상 정렬된 상태를 유지하기 때문에 인덱스 컬럼의 `INSERT`, `DELETE`, `UPDATE` 수행에는 시간이 오래 걸릴 수 있다. (인덱스를 변경해야 하기 때문)
### B-Tree (Balanced Tree) 구조
![b-tree](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FpnnNC%2FbtsziMUf2Iw%2FAAAAAAAAAAAAAAAAAAAAACczMg4wSOyCUHsrFQxfqOfgXwA6tr_ixZnUG6gWUPso%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DAMNrJf13cSgiQwoszBH7NY4%252FVug%253D)
- 이진 트리 구조를 확장시켜 **하나의 노드가 가질 수 있는 자식 노드가 2개 이상인 트리 자료 구조**
	- 최상단 노드: 루트 노드
	- 중간 노드: 브랜치 노드
	- 최하위 노드: 리프 노드
- 중간 노드 (=브랜치 노드)는 다음 단계의 노드 가리키는 포인터 갖고 있다
- **노드 내의 데이터는 항상 정렬된 상태 유지**
- 노드 간의 데이터 범위 이용해서 자식 노드 갖는다
#### B-Tree 구조 인덱스 검색 예시
![b-tree 구조 인덱스 검색 예시](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2F3qrcb%2Fbtszi4UVzxN%2FAAAAAAAAAAAAAAAAAAAAAMQaydycfas2OtKzvvE5oOyfwrS-SuIhI41Acq2W5cLK%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3D0NEt2TiUXWUQIdICfDaVces705U%253D)
1. 11: 10과 20의 중간값 (10<11<20) → 10~20 사이의 자식 노드로 이동
2. 11 < 13 → 첫 번째 자식 노드로 이동
3. Leaf 노드에서 11 검색 완료

### 클러스터형 인덱스
- 테이블 당 1개씩 만들 수 있는 인덱스 (기본 키와 비슷)
	- `PRIMARY KEY`로 간주된 필드는 기본적으로 클러스터형 인덱스로 간주
	- 기본 키가 없는 경우: `NOT NULL` 제약 조건과 `UNIQUE` 제약 조건 있는 필드를 클러스터형 인덱스로 간주
		- `NULL`이 될 수 없는 고유한 값을 갖는 필드
![클러스터형 인덱스](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FcfULTD%2FbtsznAZ0KTP%2FAAAAAAAAAAAAAAAAAAAAAGLSpGALMgPJRcn2CKWFJWG7TguF3J16ZiVcTzzHAusS%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3Dy6Jk2gMTXyX%252FF9T5M8nbrsYdL4U%253D)
- **B-Tree 구조** 갖는 가장 일반적인 인덱스 종류
- Leaf 노드의 인덱스는 항상 정렬된 상태
	- **인덱스에 실제 데이터를 함께 저장**하고 있다
	- 인덱스를 기준으로 **실제 테이블 데이터가 항상 정렬된 상태를 유지**
- 인덱스 기준으로 정렬되므로 인덱스로 가장 적합한 컬럼 하나만을 클러스터 인덱스로 지정 (대표적으로 `PRIMARY KEY`)
#### 클러스터형 인덱스의 장단점
##### 장점
- 인덱스 기준으로 데이터가 함께 정렬된 상태 유지 → **검색 속도 빠르다**
##### 단점
- 데이터 `INSERT`, `UPDATE`, `DELETE` 마다 전체 데이터 페이지 정렬 필요해서 **속도 느리다**
### 세컨더리 인덱스 (논클러스터형 인덱스)
- 테이블당 여러개 존재할 수 있으나 클러스터 인덱스를 활용한 검색보다 느리다
![세컨더리 인덱스](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fc3EYSA%2FbtszlJ397RS%2FAAAAAAAAAAAAAAAAAAAAAEXlCQrr3m8lWFt2xpDdlth0_J5huoIi5EfmsTKxsu3A%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DqSdpp1tN%252FAYe%252Bp8xcEJ5xGUeVh0%253D)
- 인덱스 페이지 별도 관리하며, **실제 데이터가 저장된 Heap 영역과는 분리된 구조**
- Leaf 노드에서 인덱스는 `RID (=RowID, 데이터 주소)`를 가지며, **`RID` 값을 이용하여 실제 데이터가 저장된 Heap 영역에 접근**
	- 인덱스 페이지만 관리하고, 실제 테이블 데이터는 따로 정렬하지 않은 상태 유지
- 클러스터 인덱스와 달리 여러개의 인덱스 생성 가능하지만, 남용하면 성능 떨어질 수 있어 주의 필요
#### 논클러스터 인덱스 장단점
##### 장점
- 실제 데이터 페이지는 정렬되지 않기 때문에, 데이터 `INSERT`, `UPDATE`, `DELETE` 속도 빠르다
##### 단점
- 인덱스만 정렬되고 실제 데이터는 정렬되어 있지 않다 → 클러스터 인덱스에 비해 검색 속도 느리다
### 클러스터 인덱스와 논클러스터 인덱스 검색 비교
![클러스터 인덱스](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FdWqAYy%2FbtszkmojWhS%2FAAAAAAAAAAAAAAAAAAAAAB5ZckY-82cs1-TJAyJWEQa8po3B-wX9GIKNhXGw6Flf%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DydokYnljftWPZHfZ4PZWxfnA7iA%253D)
- 클러스터 인덱스
	- 인덱스를 갖고 Leaf 노드까지 탐색 → 원하는 데이터 바로 얻을 수 있다
![논클러스터 인덱스 검색](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fb6VaUF%2Fbtszk5NfN3p%2FAAAAAAAAAAAAAAAAAAAAAKkQg9k3KURmRH3Jgf0_55i4W8uY4rD4ILn0ggl4RiEp%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DZhQ9a3%252BTzY6SA4JXWBk4wE4DQSE%253D)
- 논클러스터 인덱스
	- 인덱스 가지고 Leaf 노드에 탐색해서 데이터 위치만 얻고, 실제 Heap 영역에 접근하여 데이터 얻을 수 있다
	- 인덱스 페이지는 항상 정렬 유지하지만, 데이터는 정렬하지 않은 상태로 보관한다.
### 인덱스 생성 이전에 고려해야 할 기회 비용
- 인덱스의 저장 공간과 생성 시간
- 인덱스는 `SELECT` 성능 향상은 가능하지만, `INSERT`, `UPDATE`, `DELETE`에 대해서는 성능 향상 가져오지 않는다
	- 새로운 데이터 삽입 or 기존 데이터 수정/삭제할 때 인덱스에 대한 작업도 동시에 이루어진다
	- 인덱스를 유지하고 갱신하는 추가적인 자원과 연산이 필요해서 성능이 떨어질 수도 있다
#### 인덱스는 언제 활용해야 할까?
- 데이터가 충분히 많지 않다 → 굳이 인덱스 사용 필요 없음
- 인덱스로 성능 높일 수 있는 `SELECT`보다 `INSERT`, `UPDATE`, `DELETE`가 더 많다면 굳이 인덱스 필요 없다
- 인덱스는 충분히 많은 테이블, 조회 빈번히 이루어지는 테이블 필드에 만들어 활용하느 것이 좋다
	- `SELECT`문 중에서도 자주 `JOIN`되거나 `WHERE`, `ORDER BY`에서 자주 언급되는 필드가 인덱스로 활용하기 좋다
- 일반적으로 테이블당 3개 이하의 인덱스 권장
	- 중복되는 데이터가 많은 필드에 인덱스 생성하면 인덱스 효능 저하
## References
[SQL 기본 문법: JOIN(INNER, OUTER, CROSS, SELF JOIN)](https://hongong.hanbit.co.kr/sql-%EA%B8%B0%EB%B3%B8-%EB%AC%B8%EB%B2%95-joininner-outer-cross-self-join/)
[SQL 튜닝 - 인덱스에 대하여](https://developers-haven.tistory.com/54)
[SQL튜닝 - 인덱스의 종류에 대하여 (클러스터/비클러스터 인덱스)](https://developers-haven.tistory.com/55)





