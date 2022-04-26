# CORS 에러 해결하기

**클라이언트에서 해결하기**

1. 웹 브라우저 실행 옵션이나 플러그인을 통한 동일한 출처 정책 회피하기
    - 동일 출처 정책은 브라우저에서 임의로 하는 것이기 때문에 브라우저에서 동일 출처 정책을 사용하지 않으면된다.
2. jsonp방식으로  json 데이터 가져오기
    - 자바스크립트 파일이나 css파일은 동일 출처 정책에 영향을 받지 않고 가져올 수 있다.
    - 이를 이용해서 자바스크립트 파일을 가져와서 이를 json형식으로 파싱해서 데이터를 사용할 수 있다.
- jsonp방식
    
    ```json
    $.ajax({
    	url: url,
    	dataType: 'json',
    	data: data,
    	success: callback
    });
    $.getJSON(url, data, callback);
    ```
    
    ```json
    $.ajax({
    	url: url,
    	dataType: 'jsonp',
    	jsonpCallback: "myCallback",
    	uccess: callback
    });
    $.getJSON(url + "?callback=?", data, callback);
    ```
    
    - dataType만 변경하거나, 요청 url뒤에 callback파라미터를 붙일 뿐.
        
        → 이것만으로도 완전히 다른 동작.
        
    - ‘jsonp’라는 데이터 타입 요청이 아닌 <script> 호출 방식
        - HTML의 script요소로부터 요청되는 호출에는 보안상 정책이 적용되니 않는다는 점을 이용한 우회 방법.
    - jsonp callback은 서버에서 지원해줘야한다.
        - 응답 데이터는 Text형이고 callback 함수명으로 감싸진다.
        
        ```json
        "myCallback({'name':'test','age':28})"
        
        $.ajax({
        	url: url,
        	dataType: 'jsonp',
        	jsonpCallback: "myCallback",
        	success: callback
        });
        $.getJSON(url + "?callback=?", data, callback);
        ```
        
    

**서버에서 해결하기**

- 스프링 CORS
    - @CrossOrigin 어노테이션 사용.
        - @CrossOrigin은 모든 출처, 모든 헤더, @RequestMapping 주석에 지정된 Http 메소드에 최대 30분을 허용. 이 어노테이션을 속성 값에 넣어 기본값을 대체할 수 있다.
        
        ```java
        @CrossOrigin(origin="*", allowedHeaders = "*")
        @Controller
        public class MainController {
        	@GetMapping(path = "/")
        	public String main(Model model) {
        		return "main";
        	}
        }
        ```
        
        - origins
            - 허용된 출처, 이 값은 pre-flight 응답과 실제 응답 모두에 access-control-allow-origin헤더에 배치된다.
        - allowedHeaders
            - 실제 요청 중에 사용할 수 있는 요청 헤더 목록이다. pre-flight의 응답 헤더인 access-control-allow-header에 값이 사용된다.
        - allowCredential
            - 브라우저가 요청과 관련된 쿠키를 포함해야 되는지 여부를 결정한다.
            - 이 값이 true이면, pre-flight 응답에는 값이 true로 설정된 access-control-allow-credentials 헤더가 포함된다.
    - CorsFilter사용하기.
        - 서블릿 필터 인터페이스를 이용하여 개발되었다. 웹 서버의 모든 리소스의 요청을 가로채서 Cross domain request인지 체크하여 실제 요청 페이지에 전달하기전에 적절한 CORS 정책과 해더들을 적용한다.
            
            ```java
            @Component
            public class SimpleCorsFilter implements Filter {
            
                @Override
                public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
                    HttpServletRequest request = (HttpServletRequest) req;
                    HttpServletResponse response = (HttpServletResponse) res;
                    response.setHeader("Access-Control-Allow-Origin", request.getHeader("Origin"));
                    response.setHeader("Access-Control-Allow-Credentials", "true");
                    response.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE, PUT, PATCH");
                    response.setHeader("Access-Control-Max-Age", "3600");
                    response.setHeader("Access-Control-Allow-Headers", "Content-Type, Accept, X-Requested-With, remember-me");
                    chain.doFilter(req, res);
                }
            
                @Override
                public void init(FilterConfig filterConfig) {
                }
            
                @Override
                public void destroy() {
                }
            }
            ```
            
            - Access-Control-Allow-Origin
                - 도메인 간 요청을 할 수 있는 권한이 부여된 도메인을 지정한다.
            - Access-Control-Allow-Credentials
                - 도메인 간 요청에 credential 권한이 있는지 없는지 지정한다.
            - Access-Control-Expose-Headers
                - 노출하기에 안전한 헤더를 나타낸다.
            - Access-Control-Max-Age
                - pre-flighted 요청이 얼마만큼의 시간동안 캐시되는지
            - Access-Control-Allow-Methods
                - 리소스에 접근할 때 메소드가 허용되는지
            - Access-Control-Allow-Headers
                - 어떤 헤더 필드 네임이 실제 요청에서 사용할 수 있는지 가리킨다.
