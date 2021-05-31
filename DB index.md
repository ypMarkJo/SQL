### 1.색인(Index)의 역할
- 모든 데이터를 검색하지 않고 부분별로 잘라서 먼저 검색시켜 검색시간을 최소화시켜줌.

### 2.Index Scan Vs. Table Scan
- 테이블 데이터 전부를 읽어서 찾는 것이 테이블 스캔, idx=1 처럼 인덱스 영역만 읽는 걸 인덱스 스캔.

### 3.Clustered Index Vs. Non-Clustered(Secondary) Index
- 3.1: Primary Key(*),Unique index(*), Physical Data File(*).
- idx=1,2,3,4,5...처럼 분류가 펼쳐져 있을 때 카디널리티가 높으며 Index로 적합하다.남,녀 처럼 두가지만으로만 나눠질 때는 Index로 부적합. 

### 4.B(Balanced)-Tree
- Root,Branch and Leaves Node로 구성.
- Node == Page ( 1Page=16KB )
- Page Splits(50% Threshold, 8KB)
![image](https://user-images.githubusercontent.com/80379900/120139219-f7f28c80-c212-11eb-9cda-ebb352baaf09.png)
- Leaf Node<br>
 : Clustered Index => Data<br>
 : Non-Clustered Index => RID
 
 ### 5.Index 정리
 - 클러스터(Clustered)인덱스는 데이터 파일과 직접 연관.
 - 데이터 크기가 너무 크면 페이지 분할이 빈번하여 쓰기 성능 저하.
 - 인덱스는 카디널리티(Cardinality)가 높을수록 유리.
 - 클러스터 인덱스는 읽기 성능은 보조 인덱스보다 빠르지만 쓰기는 느림.
 - 페이지 분할은 시스템 부담!
 - 다중 컬럼 인덱스는 순서를 고려해서
 - 인덱스는 꼭 필요한 것만
 - 전체 테이블의 10~15% 이상을 읽을 경우 보조 인덱스 사용 안함.
 
 ### 6.Sargable(Search ARGumentABLE) Query : 검색할 때 효율적으로 Index가 이용되는 쿼리.
 - where, order by,group by등에는 가능한 index가 걸린 컬럼 사용.
 - where 절에 함수,연산,Like(시작 부분 %)문은 사거블하지 않다.(ex: where dept*4=20 or like '%천%'<- 첫번째에 %를 사용하는 경우 Index가 안잡힘)
 - between,like,대소비교(>,< 등)는 범위가 크면 사거블하지 않다.
 - or 연산자는 필터링의 반대 개념(로우수를 늘려가는)이므로 사거블이 아니다.
 - offset이 길어지면 사거블하지 않다.
 - 범위보다는 in절을 사용하는게 좋고 in 보다는 exists가 더 좋다.
 - 꼭 필요한 경우가 아니라면 서브 쿼리보다는 조인(join)을 사용하자.
 
 ### 7.Fulltext Search
 - Search Expression: (*): Partial Search/(+): Required Search/(-): Excluded Search.
 - Query : select * from Notice where match(title,contents[인덱스 칼럼]) against('조선'[search_modifier<-fulltext]) 
 - Query : select * from Notice where match(title,contents[인덱스 칼럼]) against('문신* 무신*'in boolean mode) <=문신과 무신 모두 출력하시오.
 - Query : select * from Notice where match(title,contents[인덱스 칼럼]) against('문신* 무신* -후기'IN BOOLEAN MODE);<=문신과 무신 중 후기 인물은 제외하시오.
 - Query : select * from Notice where match(title,contents[인덱스 칼럼]) against('문신* 무신* +중기'IN BOOLEAN MODE);<=문신과 무신 중 중기 인물만 출력하시오.
 
 ### 8.Partition
 - DB를 기준에 따라 분산저장해서 기준에 따라 할당된 파티션만 조회한다.<br>
 ![image](https://user-images.githubusercontent.com/80379900/120161999-e7eaa500-c232-11eb-8558-38bf12e369f1.png)
