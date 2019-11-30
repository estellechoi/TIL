## &#128187; 컴알못 탈출하기
  > 컴알못에서 개발자가 되기까지 과정의 기록

 ### 1. 환경변수(PATH) 설정하는 이유
  > 나만 모르는 걸까?

 #### 자바 환경변수를 설정하지 않으면 java 컴파일을 못하는 걸까 ?
  - 아니다. 컴파일러 실행파일(javac.exe)을 실행할 수 있다면 당연히 컴파일 할 수 있다.
  - 환경변수가 필요없는 경우는 이클립스와 같은 그래픽 유저 인터페이스 환경(GUI, Graphical User Interface)에서 개발하는 경우 등이다.
  - GUI가 내 컴퓨터의 어딘가에 있는 javac.exe 파일을 스스로 찾아서 컴파일 해주기 때문이다.

 #### 사람들은 왜 환경변수를 설정할까 ? 환경변수란 무엇인가 ?
  - 모든 일을 개발자가 직접 해야하는 명령줄 실행 환경(CLI, Command Line Interface)에서는 환경변수가 필요하다.
  - 가령, 맥의 터미널에서 wallpaper_beatles.jpeg 라는 이미지 파일을 열려고 할 때, 이미지 파일이 현재 위치한 디렉토리가 아닌 다른 디렉토리 어딘가에 있다면 컴퓨터는 해당 파일을 열 수 없다.
  - 컴퓨터가 모든 디렉토리를 다 뒤져본다면 해당 파일을 찾을 수 있겠지만, 시간이 너무 오래 걸릴 것이기 때문에 애초에 포기하는 것이다.
  > 또한, 같은 이름을 가진 다른 파일이 잘못 실행되는 경우를 방지하기 위한 목적도 있다.

  - 간단한 테스트를 해보자.
      - wallpaper_beatles.jpeg 파일을 /Users/youjin/test 디렉토리에 만들었다.
        ![screenshot1](./img/screenshot_dir.png)
      - 이제 터미널을 열고 현재 위치를 확인해보자. 현재 위치는 /Users/youjin 이라고 확인된다.
        ![screenshot2](./img/screenshot_iterm1.png)
      - 바로 이미지 파일을 열어보자. \"파일이 존재하지 않습니다\" 라는 메세지와 함께 파일이 열리지 않는다. 컴퓨터가 해당 파일을 현재 위치한 디렉토리 내에서만 찾기 때문이다.
        ![screenshot3](./img/screenshot_iterm_fail.png)
      - 이번에는 이미지 파일이 위치한 디렉토리로 이동한 후 파일을 열어보자. 잘 열린다 !
        ![screenshot4](./img/screenshot_iterm_success.png)

  - 그렇다면, 명령을 했을 때 컴퓨터에게 자주 사용하는 특정 디렉토리 몇 개만 간단히 뒤져보라고 지정해주면 되지 않을까?
  - 그렇게 지정해주는 디렉토리 경로를 PATH 라고 한다. PATH 는 환경변수(Environment variable) 라는 메모리에 저장된다. *
  - PATH 로 지정된 디렉토리에 있는 파일들은 해당 위치까지 이동하지 않고도 어디에서든 열 수 있다. 컴퓨터가 PATH 로 지정된 경로들을 뒤져서 해당 파일을 찾아내기 때문이다.


<hr/>

 ### 2. 자바 환경변수 설정하기
  - 자바 컴파일 실행파일(javac.exe)을 어디에서든 실행할 수 있도록 PATH 로 지정해보자.
  - 먼저 JDK 가 설치된 디렉토리의 Home 디렉토리로 이동한다. `cd /Library/Java/JavaVirtualMachines/jdk1.8.0_231.jdk/Contents/Home`
  > 'jdk1.8.0_231.jdk' 는 자신이 설정한 Java 버전에 따라 다를 수 있다.

    ![screenshot5](./img/cdJava.png)

  - 다음 명령을 한다. vi 편집기를 사용해 .bash_profile 파일을 연다. `vi ~/.bash_profile`
    ![screenshot6](./img/vi.png)

  - 첫번째 그림과 같이 창이 변경된다.
  - 이 상태에서 입력(INSERT) 모드로 전환하기 위해 `i` 키를 누른다. 두번째 그림처럼 INSERT 모드로 변경되었다.
    ![screenshot7](./img/vi2.png)
    ![screenshot8](./img/viInsert.png)

  - java.exe 파일이 위치한 디렉토리 경로를 PATH 로 지정해주자. `export PATH=${PATH}:$JAVA_HOME/bin`
  > javac.exe 파일의 경로는 /Library/Java/JavaVirtualMachines/jdk1.8.0_231.jdk/Contents/Home/bin/javac 이다.
  > 기본으로 JAVA_HOME 변수가 설정되어 있지 않다면 JAVA_HOME 변수도 등록해야 한다. (아래 그림 참고)

  - `esc` 키를 눌러 INSERT 모드를 종료한다.
  - 이제 vi 편집기를 종료해야 한다. vi 명령어 중 \"변경사항 저장 후 종료\" 명령어인 `:wq` 를 입력하고 `enter`(return) 키를 누른다!
    ![screenshot9](./img/viPATH.png)

  - 원래의 창으로 돌아오면 편집한 .bash_profile 파일을 적용시키기 위해 다음 명령어를 입력한다. `source ~/.bash_profile`
  - 이제 설정이 끝났다.
    ![screenshot10](./img/source.png)

  - 설정이 잘 적용되었는지 확인해보자. 환경변수 $PATH 를 출력하는 명령어를 입력한다. `echo $PATH`
  -  \/Library\/Java\/JavaVirtualMachines\/jdk1.8.0_231.jdk\/Contents\/Home\/bin 경로가 추가되어 있으면 잘 적용된 것이다.
    ![screenshot11](./img/echoPATH.png)


<hr/>

    &#128519; 참고 문서
    [macOS Java 환경변수(PATH) 설정 방법](https://whitepaek.tistory.com/28)
    [\[컴퓨터 초보자를 위한 강좌\] 패스\(Path\)란 무엇인가](http://mwultong.blogspot.com/2006/03/path.html)
