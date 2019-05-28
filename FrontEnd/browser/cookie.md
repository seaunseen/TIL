# 쿠키와 인증

개인화란
장바구니에 상품을 담아놓고 다음에 다시 브라우저를 실행하여 쇼핑몰에 들어갈 경우 담겨있는 걸 알 수 있다.
한번 로그인시 다음 로그인시에 로그인할 필요가 없다.

쿠키를 통해 웹 브라우저는 이전에 접속했던 사용자 정보를 웹서버에 전송 가능.
현재 접속한 사용자가 누군지도 알 수 있게 되었다.

교육은 위한 쿠키 인증을 해보자.

## 쿠키 생성하기

서버쪽으로 전송한 쿠키를 웹브라우저로 볼 수 있느냐

## 쿠키 활용

항상 한국어로 보시겠습니까?

탭 닫고 다시 탭 열었는데 계속 한국어로 나옴. (구글 번역도 비슷한거일듯?)

---

로그인을 해보면 sessionID라는 쿠키가 생김.
나에 대한 식별자임. 로그인 성공했을 때, 성공한 사람에 대한 식별자.
식별자엔 ID, 비번 등 개인정보가 없지만, 저 값이 훔쳐지면 나 대신에 로그인할 수 있는 보안사고가 일어남.

해커들이 세션 ID를 훔치기 위해 오만가지 방법을 다 동원함.

## Session 쿠키 VS Permanent 쿠키

Session 쿠키는 브라우저 끄면 사라짐.
Permanent 쿠키는 브라우저 꺼도 살아있음.

- expires, max-age와 같은 프로퍼티 추가하면 permanent 쿠키 됨.
  - max-age 상대값 (초가 들어감)
  - expires 절대값 (날짜 들어감)