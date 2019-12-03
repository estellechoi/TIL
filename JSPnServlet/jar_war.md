## JAR 과 WAR 의 차이

 #### JAR 과 WAR
 - Java jar tool 을 이용하여 압축된 파일로, 이 압축방식들은 압축의 해제없이 JDK에서 각 파일들에 접근하여 사용할 수 있도록 설계되어 있다.
 - Class < JAR < WAR < EAR
 - WAR 파일과 JAR 파일 모두 파일 시스템 측면에서는 ZIP 파일과 유사하며 JVM 위에서 실행할 수 있도록 메타 정보가 추가되어 있다.


 #### JAR(Java Archive) 개념
 - JAR(Java Archive, 자바 아카이브) : 여러개의 자바 클래스 파일과 리소스(텍스트, 그림 등), 메타데이터를 하나의 파일로 모아서 자바 플랫폼에 응용 소프트웨어나 라이브러리를 배포하기 위한 소프트웨어 패키지 파일 포맷

 #### WAR(Web Application Archive) 개념
 - WAR(Web Application Archive, 웹 애플리케이션 아카이브) : 소프트웨어 공학에서 자바서버 페이지, 자바 서블릿, 자바 클래스, XML, 파일, 태그 라이브러리, 정적 웹페이지 (HTML 관련 파일) 및 웹 애플리케이션을 함께 이루는 기타 자원을 한데 모아 배포하는데 사용되는 JAR 파일
 - 웹 프로젝트에서 배포를 위한 최소한의 단위이다.
 - 웹컨테이너(Web Container)는 서블릿, JAR 파일과 WEB-INF 폴더에 있는 web.xml 파일을 포함한 WAR 파일로 패키징된 웹 모듈을 필요로 한다. (패키지 개념)
 - JAR의 경우에는 실행될 클래스(main)를 명시하며, WAR 파일의 경우에는 단독으로 실행할 수 없고 서버 컨테이너에 의해서 실행되므로 배포에 대한 메타 정보가 담겨 있다. 그래서 web.xml을 배포 서술자(DD, Deploy Description)라고 부르며, 이 파일에는 웹 프로젝트에 대한 설정 정보가 담겨 있다.

 #### EAR(Enterprise Archive) 개념
 - EAR(Enterprise Archive) : 하나의 웹 어플리케이션 단위를 넘어서 실제 서버에서 배포하기 위한 단위
 - JAR와 WAR를 묶어서 각각의 기능을 지원한다.
