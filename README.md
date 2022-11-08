# 🥨 (28팀)빵지순례

## 📋 순서

- [프로젝트 기획](#--프로젝트-기획)

- [실제 구현 화면](#--실제-구현-화면)
  
  - 헤로쿠 배포 사이트: <https://calm-retreat-05431.herokuapp.com/>

- [개발 이슈 / 해결 과정 / 추가로 구현해 보고 싶은 기능](#--개발-이슈--해결-과정--추가로-구현해-보고-싶은-기능) 
- [KPT 회고](#--kpt-회고)

## 📊 프로젝트 기획

### ▶ 프로젝트 개요

1. 프로젝트 목적
   
   - 웹 프레임워크 Django와 HTML / CSS / JavaScript를 활용한 콘텐츠 기반 커뮤니티 웹 플랫폼 개발

2. 프로젝트 기간
   
   - 10.31 (월) ~ 11.07 (월)

3. 팀명
   
   - 🥖빵지순례

4. 주제
   
   - 빵집 정보 및 후기 공유 커뮤니티 서비스

5. 개발 역할 분담
   
   - 윤혜진: 콘텐츠 데이터 수집, 콘텐츠 앱 개발 (bakeries앱)
   - 조승윤: 베이스 코드 작성, 회원 관리 앱 개발 (accounts앱)
   - 차화영: 콘텐츠 후기 앱 개발 (reviews앱)
   - 박혜진: 프론트엔드 전체 관리 & 개발

### ▶ 주제 사전 조사 & 분석

- [망고플레이트](https://www.mangoplate.com/)
- [캐치테이블](https://app.catchtable.co.kr/)
- [식신](https://www.siksinhot.com/)

### ▶ 서비스 주요 기능

#### 📍 Model(모델)

1. 회원 관리 앱

   - 모델 이름: User
   - 모델 필드: AbstractUser를 상속받음

        | 필드명        | 역할       | 필드              |
        | ---------- | -------- | --------------- |
        | gender     | 성별       | CharField       |
        | followings | 팔로잉한 사용자 | ManyToManyField |

    - 모델 이름: Profile
    - 모델 필드

        | 필드명   | 역할           | 필드         |
        | ----- | ------------ | ---------- |
        | image | 프로필 사진       | ImageField |
        | user  | 프로필을 가지는 사용자 | ForeignKey |



2. 콘텐츠 후기 앱

   - 모델 이름: Review
   - 모델 필드

        | 필드명        | 역할       | 필드                  |
        | ---------- | -------- | ------------------- |
        | shop_id    | 빵집 고유번호  | IntegerField        |
        | shop_name  | 빵집 상호명   | CharField           |
        | content    | 리뷰 내용    | TextField           |
        | user       | 리뷰 작성자   | ForeignKey          |
        | created_at | 리뷰 생성 시간 | DateField           |
        | visited_at | 최근 방문일   | DateField           |
        | like_users | 좋아요한 사용자 | ManyToManyField     |
        | image      | 업로드 이미지  | ProcessedImageField |

   - 모델 이름: Comment
   - 모델 필드

        | 필드명        | 역할       | 필드            |
        | ---------- | -------- | ------------- |
        | review     | 참조 리뷰    | ForeignKey    |
        | user       | 댓글 작성자   | ForeignKey    |
        | content    | 댓글 내용    | TextField     |
        | created_at | 댓글 생성 시간 | DateTimeField |

#### 📍 View(뷰)

1. 회원 관리 앱

    - 회원가입
      - `POST` `http://127.0.0.1:8000/accounts/signup/`

    - 로그인
      - `POST` `http://127.0.0.1:8000/accounts/login/`

    - 로그아웃
        - `GET` `http://127.0.0.1:8000/accounts/logout/`

    - 회원 프로필 상세 보기
      - `GET` `http://127.0.0.1:8000/accounts/<int:user_pk>/`

    - 회원 정보 수정
      - `POST` `http://127.0.0.1:8000/accounts/<int:user_pk>/update/`

    - 팔로우
      - `POST` `http://127.0.0.1:8000/accounts/<int:user_pk>/follow/`

    - 회원 정보 삭제
      - `POST` `http://127.0.0.1:8000/accounts/<int:user_pk>/delete/`

2. 콘텐츠 후기 앱

    - 리뷰 작성
      - `POST` `http://127.0.0.1:8000/reviews/<int:shop_id>/create/`

    - 리뷰 목록 조회
      - `GET` `http://127.0.0.1:8000/reviews/`

    - 특정 리뷰 상세 보기
      - `GET` `http://127.0.0.1:8000/reviews/<int:review_pk>/`

    - 리뷰 수정
      - `POST` `http://127.0.0.1:8000/reviews/<int:review_pk>/update/`

    - 리뷰 삭제
      - `POST` `http://127.0.0.1:8000/reviews/<int:review_pk>/delete/`

    - 리뷰 좋아요
      - `POST` `http://127.0.0.1:8000/reviews/<int:review_pk>/like/`
  
    - 댓글 작성
      - `POST` `reviews/<int:review_pk>/comments/`
  
    - 댓글 삭제
      - `POST` `reviews/<int:review_pk>/comments/<int:comment_pk>/delete/`


3. 콘텐츠 앱

    - 지역별 빵집 목록 조회
      - `GET` `/bakeries/shops/region/<str:region_name>/`

    - 빵 종류별 빵집 목록
      - `GET` `/bakeries/shops/bread/<str:bread_name>/`

    - 특정 빵집의 홈 상세 정보 조회
      - `GET` `/bakeries/shop/<int:shop_id>/`

    - 지역별, 빵 종류별, 특정 빵집 이름으로 빵집 검색 기능
      - `GET` `/bakeries/search/`

#### 📍 Template(템플릿)

1. 콘텐츠 앱
   
    - 식신사이트를 크롤링해 콘텐츠 앱의 지역별 빵집 목록페이지, 빵 종류별 빵집 목록 페이지, 특정 빵집의 홈 상세 정보 페이지 구성

### ▶ 화면 설계
- 피그마를 이용해 최소 페이지의 레이아웃을 설계함
- [피그마 공동 설계 페이지로 이동](https://www.figma.com/file/snau1DXPevR8kdEJuMbejC?embed_host=notion&kind=&node-id=0%3A1&viewer=1)



## ✨ 실제 구현 화면

- 헤로쿠 배포 사이트: <https://calm-retreat-05431.herokuapp.com/>
- [발표 PDF 파일](gif/1%ED%9A%8C%EC%B0%A8_%EC%9C%A4%ED%98%9C%EC%A7%84_%EC%84%B8%EB%AF%B8%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B82.pdf)

### ▶ 메인 페이지 (home.html)

![](gif/1st-pjt-bread-road-main-page.gif)

- 장소 / 리뷰 / 빵
- 장소 : 장소 아이콘을 클릭하면 해당 장소의 빵집 리스트가 보여진다
- 리뷰 : 회원들이 작성한 리뷰를 한 눈에 볼 수 있다
- 빵 : 빵 아이콘을 클릭하면 해당 빵의 빵집 리스트를 볼 수 있다

### ▶ 콘텐츠 앱 (bakeries 앱)

![](gif/1st-pjt-bread-road-shop-page.gif)

- 식신사이트에서 크롤링한 빵집 페이지를 보여준다.
- 홈 : 기존에 작성된 기본 정보와 리뷰어가 등록한 사진을 보여준다.
- 메뉴 : 해당 빵집의 메뉴를 보여준다.
- 상세 : 해당 빵집의 상세정보를 보여준다.
- 리뷰 : 해당 빵집의 작성된 리뷰를 보여준다.

### ▶ 콘텐츠 후기 앱 (reviews 앱)

![](gif/1st-pjt-bread-road-review-page.gif)

- 빵집 별로 리뷰를 작성할 수 있다.
- 좋아요 : 작성한 리뷰를 좋아요할 수 있다.
- 댓글 : 작성한 리뷰에 댓글을 달 수 있다.
- 팔로우 : 리뷰 작성자를 팔로우할 수 있다.

### ▶ 회원 관리 앱 (accounts 앱)

![](gif/1st-pjt-bread-road-accounts-page.gif)

- 회원가입 후 로그인을 하면 마이페이지를 볼 수 있다.
- 회원정보 수정 : 회원정보를 수정할 수 있다.
- 프로필이미지 수정 : 프로필이미지를 수정할 수 있다.
- 작성한 글 : 작성한 글을 카드형태로 보여준다.
- 댓글단 글 : 댓글을 단 글을 카드형태로 보여준다.
- 좋아요한 글 : 좋아요한 글을 카드형태로 보여준다.
- 팔로우 : 다른 회원를 팔로우할 수 있다.


## 💥 개발 이슈 / 해결 과정 / 추가로 구현해 보고 싶은 기능

기획 당시 빵집 데이터를 어떻게 만들어낼까 고민을 하다가, 네이버나 구글 지도에서 빵집 정보를 크롤링해 오기로 결정을 내렸다. 지도에서 빵집 정보를 크롤링 해오려면, `Selenium`을 사용해야한다는 것을 알고 도전해봤지만 아무래도 처음 사용하는 것이라 어려움이 있었다. 이틀동안 시도를 해봤지만 결국 실패를 겪었고 우리한텐 일주일이라는 짧은 시간 밖에 주어지지 않았기 때문에 `Selenium`을 통한 크롤링에 많은 시간을 투자할 수가 없어서 차선책을 선택했다. 지도 대신 정적 사이트인 '식신'에서 단순히 `request`와 `Beautifulsoup`을 사용해 크롤링하는 것으로 선회했고, 하루의 시간을 온전히 바쳐 크롤링하는데 성공했다. 

크롤링은 처음이라 계속 구글링을 통해 메서드 하나하나 찾아가면서 기능을 구현해냈다. `Beautifulsoup`을 사용한 크롤링 과정은 생각보다 꽤 재미있었고 많이 배울 수 있는 시간이었지만, `Selenium`을 이용해 지도 크롤링을 성공하지 못했던 것이 아쉬움으로 계속 남았다. 다음 프로젝트때는 `Selenium`을 이용해 동적 페이지를 크롤링해오는 방법을 연구해보고 적용해 보고자 한다.

또한 구글 지도 API를 이용해 빵집에 대한 위치 정보도 지도에 나타나게끔 구현하고 싶었지만, 시간 관계상 시도해 보지 못해 아쉬웠다. 나중에 기회가 된다면 다음 프로젝트 때 사이트에 지도 API도 녹여내보고자 한다.

## 🎠 KPT 회고

### ▶  앞으로도 유지했으면 하는 좋은 점(Keep)
- 윤혜진: 팀원들 각자 하나의 앱 및 역할을 담당해서 기능을 잘 구현하고 서로의 앱들을 연결시키는 과정이 보람있었다.
- 차화영: 프론트, 백을 나누긴 했으나 각자가 담당한 기능과 앱에서는 각자 최소한의 css 및 디자인을 구현하였기에 한쪽으로 치우치지 않아 좋았다.
- 조승윤: 각자 담당의 역할별로 나누어 작업하여 효율적으로 진행 되어서 좋았다.

### ▶ 고쳐졌으면 하는 문제점이나 아쉬운 점(Problem)
- 윤혜진: 초기 기획 시에 좀 더 구체적으로 기능을 생각해보고 ERD도 작성해보면서 기초를 단단히 다지는 과정이 중요하다는 것을 깨달았다. 
- 차화영: 개인적으로 비동기에 대한 개념이 부족해서 좋아요와 팔로우 구현하는 것에 시간이 많이 소요된 것이 아쉬웠다. 
- 조승윤: 초기 설계를 탄탄하게 완벽히 결정을 하고 작업을 시작했으면 더욱 효율적으로 진행 되었을거 같았다.

### ▶ 문제점을 해결하기 위해 시도해볼 수 있는 해결책(Try)
- 윤혜진: 초기 기획 시에 기능을 명확히 하진 않았지만, 차후에 서로 대화를 통해 기능을 추가하면서 보완해나갈 수 있었다.
- 차화영: 혼자 해결하지 않고 팀원에게 배워서 수월하게 비동기를 구현할 수 있었다.
- 조승윤: 비동기를 처음 해보았고 새로운 기능도 구글링과 팀원의 도움을 받아 성공 할 수 있었다



참고) README template created by https://github.com/jelly12paw
