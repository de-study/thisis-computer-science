# 5. 데이터 베이스 설계
## ER 다이어그램
### ERD (Entity Relatioship Diagram)
- 데이터베이스에 저장되는 엔티티의 구조를 모델링하는 것이 목적
	- 데이터베이스로 표현할 대상을 시각적으로 설계하는 것을 의미
- ERD를 활용해 데이터베이스의 구조를 명확하게 정의하면, 추후 데이터베이스 확장 및 수정할 때 어떤 부분이 영향 받는지 쉽게 파악 가능
	- 유지 보수 용이, 개발자 간 원활한 소통 가능
### IE 표기법 (Information Engineering Notation)
![ie](https://velog.velcdn.com/images%2Fkw78999%2Fpost%2Fcd3e7c15-476d-40f2-9489-e02526b80f5d%2Fimage.png)
![ie barker](https://velog.velcdn.com/images/kw78999/post/e435fd24-76f9-4ca5-b026-56c5c31a5207/image.png)
## 정규화
- 하나의 테이블을 어떤 필드로 구성해야 할지 결정하는 작업이 정규화
	- 정규화된 테이블 형태: 정규형 (Normal Form)
- 정규화는 잠재적 문제가 발생하지 않도록 테이블 필드 구성하고, 필요한 경우 테이블 나누는 작업
	- 대부분 제 1 정규형, 제 2 정규형, 제 3 정규형, or 보이스/코드 정규형까지만 수행
### 제 1 정규형 (1NF)
- 모든 속성이 **원자 값을 가진다** (**Atomic Value**)
	- 필드 데이터가 더 이상 쪼개어질 수 없는 값을 가진다는 조건
![제 1 정규형 불만족](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FcQ0ZsC%2Fbtra8zesYLy%2FAAAAAAAAAAAAAAAAAAAAADo_XzwntMgHgtRF2qadUI7va5pRQ_lw2-pkAbyNeKX8%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DKt4SuI3vBIiJ%252FXCmsZ3s%252FboQAB8%253D)
![제 1 정규형 만족](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FOFh68%2Fbtra8nxT52M%2FAAAAAAAAAAAAAAAAAAAAAMRnn5mR5s0vaaTqHGFoh4JoKcgUdPeexYJyUeqbXCAE%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DT5uqmQ23wBHIQsEEoH428tbnwCM%253D)
#### 제 1 정규형에서 발생 가능한 이상 현상
- 삽입 이상
	- 학생이 새 과목 수강 신청할 때, 반드시 학생의 학과, 지도 교수 알아야 한다
- 삭제 이상
	- 300번 학생이 C400 과목 취소하면, 해당 과목에 대한 정보 모두 사라진다
- 갱신 이상
	- 100번 학생이 지도 교수 변경할 때, P1인 행 모두 찾아서 변경해야 한다
#### 부분 함수 종속성
- 기본 키가 아닌 속성들이 기본 키에 완전하게 함수 종속되지 못하고 **부분 함수 종속** 되어 있기 때문에 위와 같은 문제들이 발생한다
	- 기본 키의 일부 속성에만 의존하기 때문
	- 지도교수, 학과는 기본키 전체 (학번 + 과목 번호)가 아니라, **기본키의 일부인 학번에만 종속**
![부분함수종속](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FBjwFi%2FbtrbhZbKNJ8%2FAAAAAAAAAAAAAAAAAAAAANNKvb6sn3Czi9QFjEpuOizbQLT3m-jpSaJUGI0rEAZB%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3Dpct%252B7w20cBdE3okJ%252FJq%252FeYKMcZ0%253D)
### 제 2 정규형 (2NF)
- 제 1 정규형이면서, **기본 키에 속하지 않은 속성 모두가 기본키에 완전 함수 종속인 정규형**
	- 보통 기본 키가 2개 이상의 필드로 구성될 때 고려된다.
	- 기본 키의 일부에만 종속되는 필드가 있다 → 이를 제거하여 기본 키 전체에 종속되도록 필드 구성해야 한다.
- 제 2 정규형은 **부분 함수 종속성이 없는 상태**
- 후보 키에 속하지 않는 모든 필드가 **기본 키에 완전 함수 종속인 상태**
	- 완전 함수 종속성: 기본 키가 아닌 필드가 기본 키 전체에 완전하게 종속되어 있는 경우
![릴레이션 분리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2F7SW2J%2FbtraXSS2hwT%2FAAAAAAAAAAAAAAAAAAAAAIgoL2UUKUal0ndm4rNW7z2rtlPbyBPs2cErYSk_mtsV%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DUanOUdRJpkgWW1s%252BvOLoeoyRp3A%253D)
![테이블 분리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbilRd5%2FbtrbgkmPv36%2FAAAAAAAAAAAAAAAAAAAAAPZCv28aDxqy1jdDSMChDG7gYT5FLENWQpRioLoFy_5M%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DhQjqzzYBy1EHCBIaFkgJCQP0L34%253D)
#### 제 2 정규형에서 발생 가능한 이상 현상
- 삽입 이상
	- 지도 교수가 학과에 소속되어 있음을 추가할 때, 반드시 지도 학생이 있어야 한다 (불필요한 정보)
- 삭제 이상
	- 300번 학생이 자퇴하는 경우, P3 교수의 학과 정보 사라진다
- 갱신 이상
	- 지도 교수의 학과 변경되는 경우, 모두 찾아서 변경해야 한다 (동일한 지도 교수인 학생 여러명 있는 경우)
#### 이행 함수 종속성
- 속성이 A→B이고, B→C이면서 A→C의 관계에 있는 것
	- 위 예시에서는 학번 → 지도교수, 지도교수 → 학과, 학번 → 학과의 관계가 존재
	- 따라서 지도교수의 학과를 추가하기 위해서 지도 학생까지 필요 → 학생이 자퇴하였는데 지도교수의 학과 정보가 사라지는 문제점이 발생
### 제 3 정규형
- 제 2 정규형이면서, **이행적 함수 종속성을 제거한 정규형**
	- 기본 키에 속하지 않은 모든 속성이 기본 키에 이행적 함수 종속이 아닐 때 → 제 3 정규형
![릴레이션](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fbx7Ney%2Fbtra8nkprGe%2FAAAAAAAAAAAAAAAAAAAAAG9Y_ioH1tG7U0mjPq5wTqH3rp_iZYx_NQjq5azufUda%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DIeyA9AMlMH1j9s3TE5vKcmiPulg%253D)
![제3 테이블](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fb5GwKj%2Fbtrbgo3YxyO%2FAAAAAAAAAAAAAAAAAAAAANLHcrDbnXdyMLimkjFZ8-aMpRVvvE_ldUiz7KoWY6Fe%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DWVqB0uhsp3uA4M5YcnRBaI6B4Nc%253D)
- 기본 키가 아닌 나머지 모든 필드들이 간접적으로도 종속되어서는 안 된다
	- 기본 키가 아닌 나머지 모든 필드는 서로를 유추하거나 결정할 수 없어야 한다
- 제 3 정규형을 만족시키려면 이행적 종속 관계 없애야 한다 → 테이블 쪼갠다
### 보이스 / 코드 정규형 (BCNF)
- 제 3 정규형을 조금 더 강화시킨 개념
	- **제 3 정규형을 만족하는 동시에 모든 결정자가 후보 키여야 한다**
		- 결정자: 특정 필드를 식별할 수 있는 필드
		- e.g. 필드 A가 필드 B를 결정할 경우 (B가 A에 종속적일 경우), A는 B의 결정자

![BCNF](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FcM3W41%2Fbtra6HqcuBT%2FAAAAAAAAAAAAAAAAAAAAAAgY8HLKAS1fW2YEqL9MqL5UH_U2LzX9xIkBfYxqKSFL%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DRL6y922qM0Bmd8FWvZWYB0CE5lM%253D)
- 한 교수가 하나의 수업만 맡는다고 가정할 때, 제 3 정규형 만족한다
#### 제 3 정규형에서 발생 가능한 이상 현상
- 삽입 이상
	- 새로운 교수가 특정 과목 담당한다는 새로운 정보 추가 불가능. 적어도 한 명 이상의 수강 학생이 필요하다.
- 삭제 이상
	- 학번 100이 C234 과목 취소하면, P2가 C234 과목 담당한다는 정보도 삭제
- 갱신 이상
	- P1의 과목 변경되면, P1인 행 모두 찾아 변경해야 한다
- 이상 현상이 생기는 이유는, **결정자가 후보 키 (Alternative Key)로 취급되고 있지 않기 때문이다.**
	- 후보 키: 슈퍼 키(Super Key) 중에서 희소성을 갖는 키
		- 이 릴레이션에서는 (학번, 과목명)이나 (학번, 담당교수)가 후보 키가 된다
		- 담당 교수 만으로는 후보 키가 될 수 없다
			- 하지만 **후보 키가 아님에도 과목명을 결정할 수 있기 때문에 담당 교수는 결정자에 속한다**
- 이상 현상 해결 방법 → **모든 결정자가 항상 후보 키가 되도록 릴레이션을 분해하면, BCNF 만족**
![BCNF 만족](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FcImfI0%2FbtrbgktIwGf%2FAAAAAAAAAAAAAAAAAAAAADoDE1426H_ZsCms8fkBuJgMBj9SQuagHyCXHFPFHySf%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DwnDh9ymkoOi8R0G1enEtsNo8V5c%253D)
