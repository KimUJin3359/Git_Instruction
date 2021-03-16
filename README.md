# Git_Instruction
- 깃을 사용할 때 필요한 명령어들 정리

### Git을 혼자 사용한다면
- Branch를 사용할 필요가 없음
- 별 다른 기능을 사용하지 않아도 됨

#### 새로운 파일 업데이트
>> **git init**

>> **git remote add [저장소명] [주소]**
>> - ex) git remote add origin https://github.~

>> **git add [변경된 파일]**
>> - ex) git add . (변경된 모든 파일들)

>> **git commit -m [메시지 : 로그 관리를 위해 필요]**
>> - ex) git commit -m "v1.0_init"

>> **git push [로컬 저장소명] [원격 저장소명]**
>> - ex) git push origin master

#### Git GUI를 통해 수정을 했을 경우
>> git pull [로컬 저장소명] [원격 저장소명]
>> - ex) git pull origin master

#### .gitignore를 반영하지 못했을 때 
>> git rm -r --cached .

>> git add .

>> git commit -m "v1.1_apply gitigrnoe"

#### git 작업 취소
>> **git reset --mixed HEAD [파일명]**
>> - add를 취소(mixed 옵션은 default)
>> - 파일명 추가 시, 특정 파일 add 취소

>> **git reset --soft HEAD^**
>> - commit 취소 및 해당 파일들(staged) 워킹 디렉터리에 보존
>> - 수정한 파일들 존재 및 add O
>> - HEAD == @

>> **git reset --mixed HEAD^**
>> - commit 취소 및 해당 파일들(unstaged) 워킹 디렉터리에 보존
>> - 수정한 파일들은 존재하는 상태, add X

>> **git reset --hard HEAD^**
>> - commit 취소 및 해당 파일들(unstaged) 워킹 디렉터리에 보존 X
>> - 마지막 커밋 이후의 워킹 디렉토리 상태로 변경

>> **git reset [commit id]**
>> - 특정 커밋으로 되돌림
>> - id 보는 방법은 git log -g or git refolg

>> **git commit --amend**
>> - git commit message 변경

>> **git reset --hard ORIG_HEAD**
>> - pull을 취소

>> **git reset --merge ORIGI_HEAD**
>> - merge를 취소
>> - pull이나 merge를 하는 경우에 ORIG_HEAD를 남김
>> - 이를 이용하여 되돌림

### Git을 같이 사용한다면
- 이상적인 branch 관리
  ![total-branch](https://user-images.githubusercontent.com/50474972/111345204-63bb6380-86c0-11eb-83d2-438ae763811d.png)

- 

