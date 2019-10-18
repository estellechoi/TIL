## 리눅스 슈퍼유저(root) 명령어
 #### useradd
 - useradd estellechoi : estellechoi 라는 사용자 추가
 - useradd -u 1111 estellechoi : estellechoi 사용자를 추가하고 사용자 ID를 1111로 지정
 - useradd -g agroup estellechoi : estellechoi 사용자를 추가하고 agroup 그룹에 포함시킴
 - passwd estellechoi : estellechoi 사용자의 비밀번호 지정/변경

 #### usermod (사용자 속성)
 - usermod -g root estellechoi : estellechoi 사용자의 그룹을 root 그룹으로 변경

 #### userdel
 - userdel estellechoi : estellechoi 사용자를 삭제
 - userdel -r estellechoi : estellechoi 사용자를 삭제하면서 홈 디렉토리까지 삭제

 #### groups
 - groups estellechoi : estellechoi 사용자가 소속된 그룹을 보여줌

 #### groupadd
 - groupadd agroup : agroup 라는 그룹을 추가

 #### groupmod (그룹 속성)
 - groupmod -n agroup bgroup : agroup 이름을 bgroup으로 변경

 #### groupdel
 - groupdel agroup : agroup 그룹을 삭제

 #### gpasswd
 - gpasswd agroup : agroup 그룹의 비밀번호 지정
 - gpasswd -a estellechoi agroup : estellechoi 사용자를 agroup 그룹에 추가
 - gpasswd -d estellechoi agroup : estellechoi 사용자를 agroup 그룹에서 제거
 - gpasswd -A estellechoi agroup : estellechoi 사용자를 agroup 그룹의 관리자로 지정
