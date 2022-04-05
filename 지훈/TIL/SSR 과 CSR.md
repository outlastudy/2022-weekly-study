# SSR 과 CSR

## 목차
  - [Rendering 이란?](#rendering-이란)
  - [SPA (Single Page Application) 와 CSR (Client Side Rendering)](#spa-와-csr)
    - [SPA](#spa-single-page-application)
    - [CSR](#csr-client-side-rendering)
  - [MPA (Multi Page Application) 와 SSR (Server Side Rendering)](#mpa-와-ssr)
    - [MPA](#mpa-multi-page-application)
    - [SSR](#ssr-server-side-rendering)
  - [SSR 과 CSR 을 적절히](#ssr-과-csr-을-적절히)



## Rendering 이란?

    요청해서 서버로부터 받은 내용을 브라우저에 표시하는 것


## SPA 와 CSR 

### SPA (Single Page Application)
> 하나의 페이지로 구성된 웹 애플리케이션
> 
> 최초 한번 페이지 전체를 로딩(빈 HTML)한 후 이후 부터는 데이터만 변경하여 사용할 수 있는 웹 애플리케이션

### CSR (Client Side Rendering)
> Server에서 빈 HTML 만을 받아온 이후 사용자의 행동에 따라 필요한 데이터를 주고 받아서 Client 에서 Rendering 하는 방식

특징
 - 웹 에플리케이션에 필요한 모든 페이지의 정적 리소스(HTML, Javascript, CSS)를 최초 한 번에 다운로드한다.
   - 첫 로딩이 오래걸린다.
 - 사용자의 행동에 Page 전체를 요청하지 않고 필요한 데이터만 요청하여 변경한다.
   - 렌더링이 빠르다.
   - 화면이 깜빡이지 않는다.
   - Server에 부담이 적다.
 - 사용자가 요청하기전에는 빈 HTML 만 가지고 있기 때문에 SEO(검색 엔진 최적화)가 좋지않다

## MPA 와 SSR 

### MPA (Multi Page Application)
> 여러개의 페이지로 구성된 웹 애플리케이션
> 
> 새로운 페이지를 요청할 때마다 데이터가 포함된 HTML을 로딩하는 웹 애플리케이션

### SSR (Server Side Rendering)
> 각각의 페이지를 요청할때 마다 Server에서 필요한 데이터로 Rendering 하여 완성된 HTML 을 Client 로 넘겨주는 방식

특징
- 새로운 페이지를 요청할 때마다 해당 페이지의 정적 리소스를 다운로드한다.
  - 각각 페이지의 로딩이 짧음.
- 사용자의 행동에 페이지 전체를 요청한다
  - 렌더링이 느리다.
  - 화면이 깜빡인다.
  - Server에 부담이 된다.
- 데이터가 포함된 HTML을 가지고 있기 때문에 SEO에 용이하다.

## SSR 과 CSR 을 적절히

SSR 의 SEO 이점과 CSR의 UX 이점을 활용하기위해 웹 애플리케이션을 SPA / MPA 하나의 형태로 제작하는것이 아닌 UX 를 해치지 않는 선에서 페이지를 나누고(MPA) 나눈 페이지 안에서(SPA) UX를 향상시키는 방법도 있다.



