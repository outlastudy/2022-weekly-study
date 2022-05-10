# HTML5 기본 문법

## 1. HTML5

HTML (HyperText Markup Language)은 웹페이지를 기술하기 위한 마크업 언어이다. 조금 더 자세히 말하면 웹페이지의 내용(content)과 구조(structure)을 담당하는 언어로써 HTML 태그를 통해 정보를 구조화하는 것이다.

![html_css_js](https://poiemaweb.com/img/html5.png)

HTML5는 2014년 10월 28일 확정된 차세대 웹 표준으로 아래와 같은 기능들이 추가되었다.

<pre>
멀티미디어(Multimedia)
플래시와 같은 플러그인의 도움없이 비디오 및 오디오 기능을 자체적으로 지원한다.
</pre>
<pre>
그래픽(Graphics & Effects)
SVG, 캔버스를 사용한 2차원 그래픽과 CSS3, WebGL을 사용한 3차원 그래픽을 지원한다.
</pre>
<pre>
통신(Connectivity)
지금까지의 HTML은 단방향 통신만이 가능하였으나 HTML5는 서버와의 소켓 통신을 지원하므로 서버와의 양방향 통신이 가능하다.
</pre>
<pre>
디바이스 접근(Device acess)
카메라, 동작센서 등의 하드웨어 기능을 직접적으로 제어할 수 있다.
</pre>
<pre>
오프라인 및 저장소(Offline & Storage)
오프라인 상태에서도 애플리케이션을 동작시킬 수 있다. 이는 HTML5가 플랫폼으로서 사용될 수 있음을 의미한다.
시맨틱 태그(Semantics)
</pre>
<pre>
HTML 요소의 의미를 명확히 설명하는 시맨틱 태그를 도입하여 브라우저, 검색엔진, 개발자 모두에게 콘텐츠의 의미를 명확히 설명할 수 있다. 이를 통해 HTML 요소의 의미를 명확히 해석하고 그 데이터를 활용할 수 있는 시맨틱 웹을 실현할 수 있다.
</pre>
<pre>
CSS3
HTML5는 CSS3를 완벽하게 지원한다.
</pre>

## 2. Hello HTML5

HTML5 문서는 반드시 <!DOCTYPE html>으로 시작하여 문서 형식(document type)을 HTML5로 지정한다.

실제적인 HTML document은 2행부터 시작되는데 `<html>`과 `</html>` 사이에 기술한다.

`<head>`와 `</head>` 사이에는 document title, 외부 파일의 참조, 메타데이터의 설정 등이 위치하며 이 정보들은 브라우저에 표시되지 않는다.

웹브라우저에 출력되는 모든 요소는 `<body>`와 `</body>` 사이에 위치한다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Hello World</title>
  </head>
  <body>
    <h1>Hello World</h1>
    <p>안녕하세요! HTML5</p>
  </body>
</html>
```

<code>
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="utf-8">
            <title>Hello World</title>
        </head>
        <body>
            <h1>Hello World</h1>
            <p>안녕하세요! HTML5</p>
        </body>
    </html>
</code>
HTML document는 .html 확장자를 갖는 순수한 텍스트 파일이다. 따라서 메모장 등으로도 편집할 수 있으나 다양한 편의 기능을 제공하는 editor 또는 IDE(Integrated Development Environment)를 사용하는 것이 일반적이다. 대표적인 editor 또는 IDE는 아래와 같다.

- Visual Studio Code
- WebStorm
- Atom
- Brackets
- Sublime text

## 3. HTML5의 기본 문법

### 3.1 요소 (Element)

HTML 요소는 시작 태그(start tag)와 종료 태그(end tag) 그리고 태그 사이에 위치한 content로 구성된다.
![tag](https://poiemaweb.com/img/tag.png)
HTML document는 요소(Element)들의 집합으로 이루어진다.

태그는 대소문자를 구별하지 않으나 W3C: World Wide Web Consortium에서는 HTML4의 경우 소문자를 추천하고 있으므로 HTML5에서도 소문자를 사용하는 것이 일반적이다.

#### 3.1.1 요소의 중첩 (Nested Element)

요소는 중첩될 수 있다. 즉, 요소는 다른 요소를 포함할 수 있다. 이때 부자관계가 성립된다. 이러한 부자관계로 정보를 구조화하는 것이다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
  </head>
  <body>
    <h1>안녕하세요</h1>
    <p>반갑습니다!</p>
  </body>
</html>
```

<code>
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="utf-8">
        </head>
        <body>
            <h1>안녕하세요</h1>
            <p>반갑습니다!</p>
        </body>
    </html>
</code>

html 요소는 웹페이지를 구성하는 모든 요소들을 포함한다. 위 예제를 보면 html 요소는 body 요소를 포함하며 body 요소는 h1, p 요소를 포함한다. 이 중첩 관계(부자 관계)로 웹페이지의 구조(structure)를 표현한다.

이런 중첩 관계(부자 관계)를 시각적으로 파악하기 쉽게 indent(들여쓰기)를 활용한다. 보기좋은 코드는 읽기 쉬우며 읽기 쉬운 코드는 좋은 코드이다.

#### 3.1.2 빈 요소 (Empty Element)

content를 가질 수 없는 요소를 빈 요소(Empty element or Self-Closing element)라 한다. 아래의 예와 같이 빈 요소는 content가 없으며(필요가 없다) 어트리뷰트(Attribute)만을 가질 수 있다.

```
<meta charset="utf-8">
```

빈 요소 중 대표적인 요소는 아래와 같다.

- br
- hr
- img
- input
- link
- meta

### 3.2 어트리뷰트 (Attribute)

어트리뷰트(Attribute 속성)이란 요소의 성질, 특징을 정의하는 명세이다. 요소는 어트리뷰트를 가질 수 있으며 어트리뷰트는 요소에 추가적 정보(예를 들어 이미지 파일의 경로, 크기 등)를 제공한다. 어트리뷰트는 시작 태그에 위치해야 하며 이름과 값의 쌍을 이룬다. (e.g. name=”value”)

html attribute

HTML Attribute

```html
<img src="html.jpg" width="104" height="142" />
```

위의 예에서 어트리뷰트 src는 이미지 파일의 경로와 파일명, width는 이미지의 너비, height는 이미지의 높이 정보를 브라우저에 알려준다. 이 정보들을 사용하여 브라우저는 img 요소를 화면에 출력한다.

#### 3.2.1 글로벌 어트리뷰트 (HTML Global Attribute)

글로벌 어트리뷰트는 모든 HTML 요소가 공통으로 사용할 수 있는 어트리뷰트다. 몇몇 요소에는 효과가 적용되지 않을 수 있지만, 글로벌 어트리뷰트는 대체로 모든 요소에 사용될 수 있다.

자주 사용되는 글로벌 어트리뷰트는 아래와 같다.

| Attribute | Description                                                                                |
| --------- | ------------------------------------------------------------------------------------------ |
| id        | 유일한 식별자(id)를 요소에 지정한다. 중복 지정이 불가하다.                                 |
| class     | 스타일시트에 정의된 class를 요소에 지정한다. 중복 지정이 가능하다.                         |
| hidden    | css의 hidden과는 다르게 의미상으로도 브라우저에 노출되지 않게 된다.                        |
| lang      | 지정된 요소의 언어를 지정한다. 검색엔진의 크롤링 시 웹페이지의 언어를 인식할 수 있게 한다. |
| style     | 요소에 인라인 스타일을 지정한다.                                                           |
| tabindex  | 사용자가 키보드로 페이지를 내비게이션 시 이동 순서를 지정한다.                             |
| title     | 요소에 관한 제목을 지정한다.                                                               |

- Global attributes

### 3.3 주석 (Comments)

주석(comment)는 주로 개발자에게 코드를 설명하기 위해 사용되며 브라우저는 주석을 화면에 표시하지 않는다.

```
<!--주석은 화면에 표시되지 않는다.-->
<p>Lorem ipsum dolor sit amet</p>
```

<code>
    <!--주석은 화면에 표시되지 않는다.-->
    <p>Lorem ipsum dolor sit amet</p>
</code>
