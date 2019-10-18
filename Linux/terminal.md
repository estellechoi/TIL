## 리눅스 기본 명령어
 #### clear
 - clear : 사용중인 터미널 화면을 깨끗하게 지워줌

 #### pwd (Print Working Directory)
  - pwd : 현재 위치한 디렉토리 경로 출력

 #### ls (List)
 - ls : 현재 디렉토리의 파일 목록
 - ls \/home\/estellechoi : 해당 디렉토리의 파일/디렉토리 목록
 - ls -a : 현재 디렉토리의 목록 (숨김파일 포함)
 - ls -l : 현재 디렉토리의 목록 상세 (권한, 생성일자 등)
 - ls -al : ls -a 와 ls -l
 - ls \*.txt : 확장자가 .txt 인 목록
 - ls -l \/home\/estellechoi\/t* : 해당 디렉토리에서 첫글자가 t인 목록

 #### cd (Change Directory)
 - cd : 사용자의 home 디렉토리로 이동
 - cd .. : 상위 디렉토리로 이동
 - cd . : 현재 디렉토리로 이동 (제자리)
 - cd \/home\/estellechoi : 해당 디렉토리로 이동
 - cd ..\/estellechoi : 현재 위치에서 상위 디렉토리로 이동한 후 다시 해당 디렉토리로 이동

 #### mkdir (Make Directory)
 - mkdir test : 현재 디렉토리에 \/test 디렉토리 생성

 #### rmdir (Remove Directory)
 - rmdir estellechoi : 해당 디렉토리 삭제

 #### rm (Remove)
 - rm t.txt : 해당 파일 삭제
 - rm -i t.txt : 해당 파일 삭제시 정말 삭제할지 확인하는 메시지 나옴
 - rm -f t.txt : 해당 파일 바로 삭제 (f : force)
 - rm -r estellechoi : 해당 디렉토리 삭제 (r : recursive)

 #### cp (Copy)
 - cp a.txt a2.txt : a.txt 파일을 복사하여 현재 디렉토리에 a2.txt 이름의 파일로 저장
 - cp a.txt test\/a2.txt : a.txt 파일을 복사하여 test 디렉토리에 a2.txt 이름의 파일로 저장
 - cp ..\/a.txt . : 상위 디렉토리의 a.txt 파일을 복사하여 현재 디렉토리에 저장

 #### mv (Move)
 - mv a.txt \/home\/estellechoi\/ : a.txt 파일을 해당 경로로 이동
 - mv a.txt b.txt : a.txt 파일의 이름을 b.txt로 변경하여 이동 (?)

 #### vi
 - vi a.txt : a.txt 라는 파일을 생성하고 vi 편집기로 편집 시작
    - i : insert 모드 전환 (종료하기 : esc 키)
    - :q! : 저장하지 않고 vi 편집기 종료
    - :wq : 저장하고 vi 편집기 종료
    - :q : 변경사항 없을 때 vi 편집기 종료

 #### cat (Concatenate)
 - cat a.txt : a.txt 파일의 내용을 보여줌
 - cat a.txt b.txt : a.txt 와 b.txt 파일의 내용을 연결해서 보여줌
