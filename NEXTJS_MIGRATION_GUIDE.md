(전체 변환 지시문 – 1회 전달용)

목적
React SPA 프로젝트를 Next.js(App Router) 기반으로 마이그레이션하되
네이버 SEO가 실제로 작동하도록 SSR/ISR 구조로 전환한다.

1. 역할(Role)

당신은 React → Next.js(App Router) 마이그레이션 전문 시니어 엔지니어다.
특히 네이버 검색엔진의 HTML 수집 방식과
Server Component / Client Component 분리 전략을 정확히 이해하고 있다.

2. 최종 목표(Objective)

기존 React 기능 및 UX는 그대로 유지

검색엔진이 인식하는 본문 콘텐츠는 서버에서 HTML로 생성

네이버 검색 결과에서

본문 키워드 인식

미리보기 HTML

OpenGraph 미리보기
가 정상 동작하도록 구성

3. 절대 조건 (Must)
3.1 React 기능 유지

기존 컴포넌트, 상태관리, 이벤트 로직 변경 최소화

UX / 동작 차이 발생 ❌

인터랙션은 Client Component 유지

3.2 SEO 대상 본문은 반드시 SSR / ISR

검색 결과에 노출될 가능성이 있는 페이지는:

Server Component

fetch 기반 데이터 패칭

useEffect로 본문 데이터를 불러오는 구조 ❌

3.3 네이버 본문 키워드 인식 대응

초기 HTML에 다음이 직접 포함되어야 함

<h1>

<p>

<article> / <section>

JS 실행 없이 curl로 본문 확인 가능해야 함

3.4 Meta / 미리보기

Next.js metadata API 사용

title / description / og 태그는 서버에서 생성

페이지별 동적 metadata 허용

3.5 렌더링 전략

기본: ISR

export const revalidate 필수

로그인/개인화 페이지는 SSR 또는 CSR 허용

4. 구조 규칙 (강제)
4.1 Server / Client 분리 원칙

SEO + 본문 → Server Component

버튼 / 상태 / 이벤트 → Client Component

// Server Component
export default async function Page() {
  const data = await fetchData();
  return (
    <article>
      <h1>{data.title}</h1>
      <p>{data.content}</p>
      <ClientPart data={data} />
    </article>
  );
}

// Client Component
"use client";
export default function ClientPart() {
  return <button>장바구니</button>;
}

4.2 라우팅

react-router-dom 제거

Next.js 파일 기반 라우팅 사용

동적 라우트: [id]/page.tsx

4.3 데이터 패칭 규칙
위치	허용
Server Component	fetch
Client Component	axios / useEffect
SEO 본문	Client 패칭 ❌
5. 변환 결과물 요구사항

전체 코드 제공 (생략 금지)

파일 분리 구조 명확히 제시

각 파일의 역할 간단 설명

실제 서비스 가능한 수준

6. 성공 기준 (통과 조건)

curl https://url → 본문 텍스트 확인

네이버 웹마스터 “수집 미리보기” 정상

JS 비활성화 상태에서도 본문 확인

기존 React 기능 정상 동작

7. 최종 목표 문장

검색엔진은 정적 HTML 사이트로 인식하고,
사용자는 React SPA처럼 사용한다