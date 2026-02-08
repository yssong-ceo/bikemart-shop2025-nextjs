(변환 완료 후 페이지별 점검 지시문)

목적
Next.js 변환이 SEO / SSR 관점에서 올바르게 되었는지
페이지 단위로 검증한다.

1. 페이지 분류
A. SEO 대상 페이지

메인

상품 리스트

상품 상세

콘텐츠/게시글

B. SEO 비대상 페이지

로그인

마이페이지

장바구니

관리자

2. SEO 대상 페이지 점검 체크리스트
2.1 구조 점검

 "use client" 없음

 Server Component

 fetch로 데이터 로딩

 ISR 적용 (revalidate)

2.2 HTML 본문 점검

 <article> 존재

 <h1> 1개 이상

 <p> 본문 텍스트 존재

 본문이 JS 실행 없이 HTML에 포함됨

2.3 SEO 메타 점검

 title 설정

 description 설정

 og:title / og:description / og:image

 페이지별 동적 metadata

2.4 네이버 기준 검증

 curl URL 시 본문 출력

 네이버 웹마스터 “수집 미리보기” 정상

 본문 키워드 인식 가능

3. SEO 비대상 페이지 점검

 "use client" 선언

 기존 React 로직 유지

 useEffect / axios 정상 동작

 SEO 관련 코드 없음

4. 혼합 페이지(Server + Client) 점검

 본문은 Server

 버튼 / 이벤트는 Client

 props로 데이터 전달

 Client에서 본문 렌더링 ❌

5. 최종 판정 기준
✅ 합격

검색엔진 HTML 수집 OK

UX 변화 없음

SEO 도구 정상

❌ 불합격

빈 HTML + JS 렌더링

본문 Client 렌더링

h1 없는 페이지

6. 점검 완료 문장 (AI 응답 기준)

“이 페이지는 네이버 검색엔진이 본문 키워드를 인식할 수 있는