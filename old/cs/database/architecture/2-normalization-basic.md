# 2. normalization-basic

## 1정규화

- 각 행의 데이터들은 원자적 값을 가짐
- 각 행은 기본키를 가져야함

### 기본키

- 레코드 식별시 사용

- 특징
  - 간결해야함
  - 값 변경 불가

#### 만들기

기본키 특징을 가진 기존 열을 찾는것보다,
기본키를 위한 새로운 열을 생성
이 때, `auto_increment`를 이용

보통 ID라고 함

### 좋은점

- 중복 데이터가 없어서 데이터베이스 크기 줄여줌
- 찾아야 할 데이터가 적어 쿼리가 더 빨라짐