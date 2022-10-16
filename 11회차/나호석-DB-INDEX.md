# DB INDEX

## INDEX란

- DB 분야에 있어서 테이블에 대한 동작의 속도를 높여주는 자료 구조.
- 일종의 책 뒤의 찾아보기나 책 앞의 목차의 개념.
- 칼럼의 값과 해당 레코드가 저장된 주소를 키와 값의 쌍으로 인덱스를 만든다.

## INDEX 자료구조

### Hash Table

- 해시 값을 계산해서 인덱싱하는 알고리즘으로 매우 빠른 검색을 지원.
- 값을 변형해서 인덱싱하므로, 특정 문자로 시작하는 값으로 검색을 하는 전방 일치와 같이 값의 일부만으로 검색하고자 할 때는 해시 인덱스를 사용할 수 없음.
  - ex) 부등호(<, >)
- 주로 메모리 기반의 데이터베이스에서 많이 사용.

### B+ Tree

- 대부분의 DBMS 그리고 오라클에서 특히 중점적으로 사용하고 있는 가장 보편적인 인덱스.
- Root Node(기준)/Branch Node(중간)/Leaf Node(말단)으로 구성.
- Leaf Node에만 데이터를 저장하고 Leaf Node들끼리 LinkedList로 연결되어 있 선형 시간이 소모되어 시간 효율이 올라감.

![b+tree](https://user-images.githubusercontent.com/16220817/196031223-b444657e-da41-4ab8-991a-faa2aaba720c.png)

## 장점

- 데이터가 정렬되어 있기 때문에 테이블에서 검색과 정렬 속도를 향상.
- 시스템의 전반적인 부하를 줄일 수 있다.

## 인덱스 사용시 주의할 점

- 정렬된 상태를 계속 유지시켜야 한다는 점.
  - INSERT: 새로운 데이터에 대한 인덱스를 추가
  - DELETE: 삭제하는 데이터의 인덱스를 사용하지 않는다는 작업 진행
  - UPDATE: 기존의 인덱스를 사용하지 않음 처리하고, 갱신된 데이터에 대해 인덱스 추가
  - 데이터 갱신보다는 **조회**에 주로 사용되는 컬럼에 Index를 생성하는 것이 유리.
- 데이터의 형식에 따라서 인덱스의 성능이 악영향을 미칠 수 있다.
  - 이름 vs 성별, 나이

## 다중 칼럼 인덱스

- 컬럼 순서가 매우 중요.
- Cardinality(기수성)
  - 카디널리티는 유니크한 값의 개수.
  - cardinality(기수성)가 높은 컬럼이 앞에와야 검색 효율이 좋다.
- Selectivity (선택도)
  - 선택도란 얼마나 값을 잘 가져오는지 의미, 1이면 유니크하다는 의미.
  - 20~25% 를 넘어서면 그 인덱스를 사용하지 않는게 좋음.
- 활용도
  - 해당 컬럼이 실제 작업에서 얼마나 활용되는지에 대한 값.
    - 수동 쿼리 조회, 로직과 서비스에서 쿼리를 날릴 때 WHERE 절에 자주 활용되는지를 판단.
  - 활용도가 높을 수록 인덱스 설정에 좋음.
- 중복도
  - 중복도가 없을수록 인덱스 설정에 좋은 컬럼.

## 참고자료

- [DB INDEX](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Database)
- [DB의 INDEX 개념정리](https://azderica.github.io/00-db-index/)
- [DB INDEX 입문](https://tecoble.techcourse.co.kr/post/2021-09-18-db-index/)
- [DB INDEX란](https://velog.io/@alicesykim95/DB-%EC%9D%B8%EB%8D%B1%EC%8A%A4Index%EB%9E%80)
- [B-TREE, B+TREE란](https://zorba91.tistory.com/m/293)
- [다중 칼럼 INDEX](https://intrepidgeeks.com/tutorial/multiple-column-indexes)
