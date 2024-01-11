
- DBA(Data Block Address): 데이터 블록이 가지고 있는 고유 주소값
	- 디스크 상에서 몇 번 데이터파일의 몇 번째 블록에 위치하는지?
- ROWID: 테이블 레코드를 가리키는 주소값
- 블록 번호: 데이터 파일 내에서 부여한 상대적 순번
- 로우 번호: 블록 내 순번

> ROWID = 데이터 블록 주소 + 로우 번호
> 데이터 블록 주소(DBA) = 데이터 파일 번호 + 블록 번호
> 블록 번호 = 데이터파일 내에서 부여한 상대적 순번
> 로우 번호 = 블록 내 순번

- 수직적 탐색: 인덱스 스캔 시작지점을 찾는 과정
- 수평적 탐색: 데이터를 찾는 과정

- index range scan: 일반적으로 알고있는 인덱스 범위 검색
- index full scan: 인덱스 전체를 검색
	- 인덱스는 SingleBlock I/O 사용
- table full scan: 전체 테이블 검색
	- MultiBlock I/O 사용

- SingleBlock I/O: 한 개의 블록을 읽어서 메모리에 적재
- MultiBlock I/O: 여러개의 블록을 읽어서 메모리에 적재

- Hash Chain Latch: 동시성 제어를 위한 해시 체인 락 메커니즘(버퍼 Lock 과 다름)
- Hash Chain Scan: DBA로부터 변환된 해시 키값을 이용하여 버퍼 캐시 블록을 탐색하는 과정
	- 캐싱된 블록이 있을 경우, 변환된 키값을 이용해 해시 체인을 찾고 해당 버퍼 헤더에서 찾고자하는 블록을 탐색

- Clustering Factor(CF): 특정 컬럼을 기준으로 같은값을 갖는 데이터가 서로 모여있는 정도
	- CF가 높은 컬럼에 생성한 인덱스는 높은 효율을 보임: 첫 번째 레코드를 읽은 후 다음 인덱스 레코드를 읽었는데 같은 블록을 가리킬 경우 래치 획득, 해시 체인 스캔 과정을 생략 -> 테이블 블록 읽기 => 블록 I/O 과정이 생략되는 효과

### SQL trace
- cr(Consistent Read): 총 (버퍼 캐시에서)읽은 블럭 수
	- disk상에서 읽은 블럭 또한 버퍼 캐시에 올려두고 읽기 때문에 disk + cache에서 읽은 수라고 해석 가능
- pr(Physical Read): 디스크에서 읽은 블럭 수
- pw(Physical write): 디스크에 저장한 블럭 수
- card: cardinality