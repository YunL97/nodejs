* nodejs: javascript런타임이다 -> nodejs는 다른버전의 javascript라고 할 수 있다 -> 즉 자바스크립트를 다른환경에서 구현한것
* 자바스크립트 기반으로 반들어졌으며 몇몇 기능을 더하고 기존 자바스크립트로 브라우저에서 가능했던 기능중 일부가 빠짐
* 이론적으로 서버 뿐만아니라 어떤 기기에서도 실행 할 수 있다
* v8 js엔진을 사용중인데 js코드를 머신코드로 컴파일 한다
* 브라우저는 항상 속일 수 있기 때문에 백엔드가 필요하다
* nodejs는 단지 유틸리티 스크립트를 작성하는데 훌륭한도구 -> vue, react, express
* 아 nodejs는 v8 엔진이 내장되어 있으니까 브라우저가 아닌 컴퓨터 안에서도 실행이 되는거네 
* heap: 
  * 동적으로 할당된 데이터, 객체, 배열 및 함수와 같은 자료구조의 인스턴스를 저장하는것
  * 참조로 연결되어 있음
  * 힙에 저장된 데이터에 대한 참조가 stack 내에서 사용

* nodejs = js engine + LIBUV
* javascript engine(stack + heap) + webapis + callback queue + enent loop
  * Stack -> WebAPIs -> callback queue -> event loop
  * Stack: 코드의 실행이 주로 callStack 을 통해 시작, 함수 호출 및 실행은 여기서 관리
  * Web Apis: 비동기작업을 여기서 처리, 작업이 완료되면 콜백 함수를 호출하거나 이벤트를 생성
  * callback queue(task queue): 비동기 작업이 완료되면 콜백 함수가 callback queue or tackqueue에 추가. 여기에 대기중인 작업들이 들어옴
  * event loop: call stack 과 callback queue 간의 상호작용을 관리, 이벤트 루프는 연산을 다루지 않고 오직 완성된 콜백에 대한 모든 코드들을 처리
    * 이벤트 루프가 주기적으로 call stack이 비어 있는지 확인하고 비어 있을때 callback queue에서 대기 중인 작업을 가져와 call stack으로 이동 -> 비동기 작업의 결과 또는 콜백함수가 실행

* libuv: 비동기에 집중하는 멀티 플랫폼 라이브러리
  * c언어로 개발
  * thread pool이 존재하는데 이 스레드가 동기적인 입출력 작업을 이벤트 루프대신 처리
* nodejs는 요청을 처리하는것이 매우 빠르고 배후에서는 운영체제의 영향력에 따라 약간의 다중 스레드를 사용한다? 이게 웹 api인가
* 데이터 스트리밍: 데이터를 조각조각 처리하고 전송하는 프로세스
  * 파일을 읽을때 그 파일의 크기만큼 메모리에 공간을 마련해두고 그 공간에 파일 데이터를 저장하는데 이거를 버퍼라고함
  * 버커 크기가 너무 크기때문에 100mb 를 1mb로 100번 보내는 걸 스트림, 이때 나눠진 조각을 청크
  * 간단한 요청의 경우에는 이과정이 필요 없지만 업로드 파일의 경우 상당히 오래걸리기 때문에 사용
  * 파일 전체가 분석 완료되고 전부 업로드 되기 전까지 아무것도 안하면서 기다릴 필요가 없음
  * 대량의 데이터를 처리하거나 네트워크를 통해 데이터를 효율적으로 전송해야 할때 유용하다
  * 메모리 사용을 최소화 한다

* nodejs 는 기다리거나 중지되지 않는 특성이 있다
* nodejs는 논블로킹 방식 -> 수많은 콜백과 이벤트를 등록해두면 특정 작업이 끝난후에 nodejs가 해당 코드를 작동 시킨다는뜻
* 어느경우에도 메인 스레드가 막히면 안될뿐더러 nodejs는 그런 문제를 피하도록 설계되어 있다


* express 장점: 매우 유연하며 특별한 기능들을 과도하게 추가하지 않는다
* express는 미들웨어와 깊게 연관이 되어있다
* 미들웨어의 뜻은 들어오는 요청을 express에 의한 다양한 함수를 통해 자동으로 이동하는것
  * http 요청과 응답 사이에서 단계별 동작을 수행해주는 함수
```
// 미들웨어
app.use('/',(req, res, next) => {
  console.log('asd')
  next(); // 응답을 보내지 않으면 next()를 호출해야 다음 미들웨어 실행 가능
})
```
* '/' 경로는  다타기 때문에 가장 마지막에 있어야 한다
* use 대신 get, post,delete 등등을 사용해도 됨
* __dirname: node.js 환경에서 사용되는 내장전역변수로 운영체제의 절대경로를 프로젝트 폴더로 고정해준다
  * 윈도우는 경로설정할때 \ 사용하기때문에 / 로 경로를 설정하면 윈도우에서 작동하지 않는다
  * path.join은 실행중인 운영체제를 감지해서 자동으로 올바른 경로를 생성

* get: 클라이언트에서 서버로 어떠한 리소스로 부터 정보를 요청하기 위해 사용되는 메서드
  * 조회
  * 캐시가능: 서버에 리소스를 요청할 때 웹 캐시가 요청을 가로채 서버로부터 리소스를 다시 다운로드 하는 대신 리소스의 복사본을 반환
  * 브라우저 히스토리에 남는다
  * 북마크 될 수 있다 -> 즐겨찾기로 저장될 수 있다는 의미
  * 길이제한이 있다 -> 브라우저마다 제한이 다름
  * 중요한 정보를 다루면 안된다  -> 파라미터에 다 노출되기 대문에 최소한의 보안 의식
  * 데이터를 요청할때만 사용
* post: 클라이언트에서 서버로 리소스를 생성하거나 업데이트 하기 위해 데이터를 보낼때 사용되는 메서드
  * 전송할 데이터를 http메시지 body부분에 담아서 서버로 보낸다 body 타입은 content-type 헤더에 따라 결정
  * get에서 url 파라미터로 a1=1&a2 가 body에 담겨 보내진다
  * 길이제한이 없어서 용량이 큰데이터를 보내거나 보인이 필요한 부분에 많이 사용
  * 보통 form을 통해 서버로 전송
  * 요청은 캐시되지않음
  * 브라우저 히스토리에 남지않음
  * 북마크되지않음


# SQL
스키마: db구조와 제약조건에 관한 전반적인 명세를 정의한 메타데이터의 집합 -> 제약조건, 데이터베이스 구조
* SQL: 테이블을 이용한 데이터 저장
  * 수평스케일링이 매우어려움 (수평스케일링: 서버추가, 수직스케일링: cpu, 메모리추가)
  * 상관관계가 정말 중요한 곳에 저장하고 테이블 전반에 걸쳐 분할한다던가
  * 강력한 제한조건(스키마) 를 보유해야하면 sql이 완벽하다
  * 데이터가 자주 변경되지 않을때 유용
  * 애플리케이션 논리가 복잡해질수록 쿼리 역시 점점 복잡해진다
* noSQL: 엄격한 스키마가 없음
  * 매운 빠른속도로 진행 가능
  * 강한 데이터 스키마가 없으며 
  * 동일한 집합에 혼합된 데이터가 존재할 수 있고 
  * 어떤 구조도 강요되지 않으며  
  * 일반적인 데이터 상관관계가 없다
  * 처리량이 많은 애플리케이션에서 매우 고성능
  * 장바구니 같이 애플리케이션에서 ㅈ다주 변경되는 부분을 nosql 사용하면 좋음


* sql사용시 단일 연결을 사용해도되고 creatatePool을 호출해서 실행할 쿼리가 있을때마다 다중연결해서 사용가능
* sequelize: 백그라운드에서 실제로 sql코드를 처리하면 js객체로 매핑애 sql코드를 실행하는 편리한 메서드를 제공 -> sql코드를 직접 작성하지 않아도 된다

*  nosql은 db가 있고 테이블이 아닌 user, order등의 컬렉션이 있다
*  각 컬렉션에는 기록이 아닌 문서가 있다
*  컬렉션 안의 문서에는 같은 구조를 가질 필요가 없다
*  mongodb같은 경우 json을 통해 컬렉션에 데이터를 저장한다
*  데이터가 필요한 형식으로 쿼리를 하도록 만들어서 필요한 형식으로 데이터를 저장해서 추후에 병합하는 과정을 많이 거치지 않고 -> 즉 서버의 백그라운드에서 여러 컬렋션을 합치지 않고도 필요한 형식의 데이터를 가져올 수 있게 끔만든다

# 쿠키 세션
* 쿠키: 클라이언트 측 브라우저에 데이터를 저장하는 용도
  * 사용자가 볼 수 있기 때문에 쿠키에 민감한 데이터를 저장하면 안된다
  * 쿠키는 조작가능
  * 브라우저를 닫으면 디폴트로 만료되는 쿠키를 세션쿠키라고한다
  * 세션쿠키는 세션을 식별하기 위해 사용되는 쿠키가 아님
  * 세션큐키: 현재 브라우저의 해당 페이지에 있는 동안 유효하기 때문에 세션쿠키라고 한다 -> 브라우저 닫으면 만료
  * 지속쿠키: 만료날자나 쿠키가 무효화 되는 수명을 설정하는 쿠키, 브라우저를 닫아도 설정된 만료 날짜까지 쿠키 유지
  * secure를 추가한 쿠키는 https를 통해 페이지가 제공될 경우에만 설정된다
  * 글로벌 변수는 요청주기와 관계없이 계속 사용가능하지만 모든 요청간에 공유되기 때문에 사요자간에도 공유가 된다 -> 이때 쿠키가 유용하게 쓰이는데 쿠키를 이용하면 특정 사용자의 브라우저에 데이터를 저장해서 데이터가 해당 사용자에 맞춤화 되어 다른 사용자에 영향을 끼치지 않는다
  * 브라우저에 쿠키가 있으면 브라우저가 디폴트로 서버에 쿠키를 보낸다
  * 민담한데이터는 브라우저에 저장하면 안된다
  * 쿠키는 다른 페이지로 전달되 수 있으며 이는 실제 이미지가 없는 이미지 url인 페이지에 트래킹 픽셀이 있는 위치 추적에 흔히 활용되는 도구
  * 브라우저 js코드 내부에서는 쿠키를 읽을 수 없다

* 세션
  *  서버에서 만든 세션을 브라우저 쿠키에 저장 -> 이 쿠키가 서버와 통신할때 인증역할을한다
  *  세션의 모든 정보를 메모리에 저장하면 메모리가 빠르게 오버플로우 된다 -> 바람직하지 않고 보안 측면에서도 이상적이지 않음 -> db에 저장하는게 좋음
  *  세션에는 장바구니등 어떤것도 저장가능, 특히 사용자인증에 흔히 사용된다  
  *  세션은 서버에 저장
  *  사용자를 식별하기위해 세션을 사용
  *  응답을 매번 전달할 때마다 잃어버리고 싶지 않으며 다른 사용자에게 보여는 안되는 사용자 관련 데이터를 대상으로 사용
  *  리다이렉트를 하는경우에 리다이렉트는 독립적으로 실행되므로 너무 일찍일어날 수 있는데 req.session.save(() => {})를 호출하면 된다. -> 세션이 설정된것을 보장해야할때 사용


# 인증
*  비밀번호는 무조건 암호화 해야한다 다른사람들이 비밀번호를 유추 할 수없게 -> 비밀번호 단뱡향 해시화 -> 유효기간이 있는 고유한 토큰 생성 후 데이터베이스 저장 -> 토큰이 추가된 url을 버튼에 추가후 email 발송
*  bcryptjs: 사용자 비밀번호 암호화 라이브러리
* CSRF Attack: 사이트간 위조 -> 세션을 악용하고 애플리케이션 사용자를 속여서 악성코드를 실행하도록 하는 특수한 공격방법 -> csurf 패키지사용 CSRF토큰생성해준다
*  

# 이메일 전송
* 메일서버를 구축하는일은 매우 어렵다
* 처리하고 메일서버를 만드는일은 보안등 여러 면에서 매우 복잡 -> 3자 메일서버 사용 ex) aws
* crypto: 내장된 암호라이브러리
* 비밀번호 재설정 같은 경우 이메일을 받을때 유효한 토큰이 포함된 이메일로만 비밀번호를 변경할 수 있도록 해야한다