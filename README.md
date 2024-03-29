## 리팩토링

#### 1. PC 반응형 디자인
  - `문제`: 모바일 뷰를 주요 고려 대상으로 삼아 웹 페이지를 개발함으로써,  
    다양한 PC 화면 크기와 해상도에 대응하지 못하는 문제가 발생했습니다.   
    이로 인해 사용자 경험이 일관되지 않아지는 문제가 예상되었습니다.  
  - `해결`: 미디어 쿼리를 활용으로 PC 반응형 디자인을 적용하여 웹 페이지가 다양한 PC 환경에서 최적으로 표시되도록 조정하였습니다.  
    더불어, PC 버전에서의 헤더와 네브바를 추가하고, 계정 추천 영역을 추가하여 개별 기능을 강화하였습니다.  
    이로써 PC 사용자들이 다양한 화면 크기와 해상도에서 웹 페이지를 원활하게 이용할 수 있게 되었습니다.

---
### 성능개선
#### 2. 검색 기능 디바운스 적용
  - `문제`: 계정 검색 시에 사용자가 글자를 입력할 때마다 매번 검색 요청이 발생하여  
    데이터 트래픽 증가로 인해 서버에 불필요한 부하가 발생하는 문제가 있었습니다.  
    또한, 사용자가 검색어 입력을 완료하지 않은 상태에서 결과가 표시되다보니 부정확한 결과가 표시되어 사용자 경험이 저하되는 문제도 있었습니다.
  - `해결`: 계정 검색에 디바운스를 적용하여, 사용자가 글자를 입력하는 동안 일정 시간 동안 검색 요청을 지연시켰습니다.  
    따라서, 연속된 검색 요청을 방지하고, 사용자가 입력을 마친 후 한 번의 검색 요청을 처리하도록 할 수 있었습니다.  
    이로써, 서버 부하를 감소시키고 사용자가 더 효과적으로 계정을 검색할 수 있게 되었습니다.

#### 3. 이미지 레이지 로딩 적용
  - `문제`: 모든 이미지가 페이지 로딩 시 동시에 다운로드 되어 초기 페이지 로딩 속도가 느려지는 문제가 있었습니다.  
    특히 대용량 이미지 파일이 많은 경우 초기 로딩 시간이 길어져 사용자가 페이지 로딩에 상대적으로 더 많은 시간을 소비하게 되었습니다.  
  - `해결`: 이미지 레이지 로딩을 도입하여, 초기 페이지 로딩 시에는 이미지를 포함하지 않는 기본 내용만을 로드하도록 했습니다.  
    그 다음, 사용자가 스크롤하거나 이미지가 화면에 나타나야 할 때 해당 이미지만 비동기적으로 로딩되도록 조정하였습니다.   
    이로써 초기 페이지 로딩 속도가 개선되었으며, 사용자가 페이지 로딩을 기다리는 시간을 단축시켰습니다.

#### 4. 이미지 압축 적용
  - `문제`: 대용량 이미지 파일이 웹 페이지에 업로드되는 경우,  
    해당 이미지를 로딩하는 과정에서 용량이 상대적으로 작은 이미지보다 화면에 늦게 나타나게 되어  
    사용자가 이미지를 곧바로 확인하지 못하는 문제가 종종 있었습니다.  
  - `해결`: 이미지 업로드 시 이미지 압축을 적용하여 이미지 파일의 크기를 줄였습니다.  
    이미지 압축을 통해 대역폭과 데이터 부하도 줄이며, 페이지 로딩 속도를 개선하여 사용자가 웹 페이지를 빠르게 로드할 수 있도록 하였습니다.

---

#### 5. 잘못된 확장자로 여러번 파일 업로드 시 예외처리
  - `문제`: 잘못된 확장자로 파일 업로드를 할 경우 조건문으로 인해 동작되어야하는 state의 변화가 이루어지지 않아서,  
    사용자 입장에서 잘못된 접근임을 인지하기 어려웠습니다.
  - `해결`: useEffect Hook을 이용해서 state의 상태값의 변화를 감지하도록 하여  
    상태의 boolean 값에 따라 state의 변화를 일으키도록 했습니다.  
    이를 통해 상태값이 변화하지 않는 현상에 대응하고 처리할 수 있었습니다.

#### 6. 프로필 페이지에서의 탭바 활성화 상태 유지처리
  - `문제`: 프로필 페이지에서 탭바를 useState Hook으로 state를 관리하기 때문에,  
    다른 컴포넌트로 이동 후에 뒤로가기를 버튼을 누를 경우 설정해놓은 state의 초기값에 해당하는 탭바가 활성화됐었습니다.
  - `해결`: useParams Hook 이용해서 추출한 경로를 활용하여 유저 프로필 페이지에서 탭을 선택하고 해당하는 컨텐츠를 표시할 수 있게 했습니다. 

#### 7. 로그인 유효성 검사 개선
 
  - `문제`: 로그인 시 발생하는 실패 메시지가 인풋 필드에 값을 입력해도 자동으로 사라지지 않아, 사용자가 로그인을 진행하는데 불편하고 혼란스러웠습니다.
  - `해결`: 유효성 검사 로직을 개선하여 사용자가 로그인을 실패한 후, 사용자가 인풋 필드에 유효한 값을 입력할 때 실패 메시지가 자동으로 사라지도록 조정했습니다.
    더불어, 사용자가 로그인 시도를 재시작하거나 입력을 수정하는 데 드는 시간을 최소화하기 위해 인풋 필드와 실패 메시지를 더 효율적으로 연동하도록 개선했습니다.

#### 8. 회원가입 리다이렉션 개선

  - `문제`: 회원가입을 완료한 후, 사용자는 스플래시 페이지를 통과한 후 로그인 페이지로 이동해야 했습니다.   
    이로 인해 사용자 경험이 불필요하게 지연되었고, 로그인 절차가 번거로웠습니다.   
  - `해결`: 회원가입 프로세스를 완료한 후, 사용자를 스플래시 페이지가 아닌 직접 로그인 페이지로 리다이렉트하도록 조정했습니다.   
    이로써 사용자는 더 빠르게 로그인을 시작할 수 있으며, 웹 사이트를 더 원활하게 이용할 수 있게 되었습니다.

<br />
