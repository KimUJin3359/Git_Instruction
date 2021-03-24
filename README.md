# Git_Instruction
- 깃을 사용할 때 필요한 명령어들 정리

# FETCH 및 그 이후부분 20210325 수정 예정

### Index
- [용어정리](https://github.com/KimUJin3359/Git_Instruction/blob/master/README.md#%EC%9A%A9%EC%96%B4-%EC%A0%95%EB%A6%AC)
- [git 명령어](https://github.com/KimUJin3359/Git_Instruction/blob/master/README.md#git-%EB%AA%85%EB%A0%B9%EC%96%B4)
- [git 명령어(branch 관련)](https://github.com/KimUJin3359/Git_Instruction/blob/master/README.md#git-%EB%B8%8C%EB%9E%9C%EC%B9%98%EA%B4%80%EB%A0%A8-%EB%AA%85%EB%A0%B9%EC%96%B4)
- 명령어 활용
  - [git 파일 업로드](https://github.com/KimUJin3359/Git_Instruction#%EC%83%88%EB%A1%9C%EC%9A%B4-%ED%8C%8C%EC%9D%BC-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8)
  - [git .ignore 반영해서 다시 업로드](https://github.com/KimUJin3359/Git_Instruction#gitignore%EB%A5%BC-%EB%B0%98%EC%98%81%ED%95%98%EC%A7%80-%EB%AA%BB%ED%96%88%EC%9D%84-%EB%95%8C)
  - [git 작업(add, commit, pull, merge) 취소](https://github.com/KimUJin3359/Git_Instruction#git-%EC%9E%91%EC%97%85-%EC%B7%A8%EC%86%8C)

## Git 정리

### 용어 정리
#### Git : 분산형 버전 관리 시스템
- 파일이 **변경 이력 별로 구분**되어 저장

#### 저장소
- **원격 저장소** : 전용 **서버에서 관리**되는 여러 사람과 함께 공유하는 저장소
- **로컬 저장소** : 내 **PC에 저장**되는 개인 전용 저장소

#### 로컬 저장소
- **작업 트리** : 작업하는 폴더
- **인덱스** : 작업 트리와 Local 저장소 사이의 공간
- staging : 인덱스에 파일 상태를 기록

#### 브랜치(branch)
- 개념 : 동일한 소스코드로 **여러 작업(버그 픽스/기능 추가 등)을 수행**하기 위해 만들어진 기능
  - 각자 독립적인 작업 영역에서 소스코드 변경
- **master 브랜치**
  - 저장소를 처음 만들 때 생성되는 브랜치
  - 새로운 브랜치를 만들지 않는 이상 모든 작업은 master 브랜치에서 수행
- **integration(통합) 브랜치**
  - 언제든지 배포할 수 있는 버전을 가진 브랜치(메인)
  - 안정성이 유지되어야 함
- **topic 브랜치**
  - 수정/기능 추가 등을 해야할 때 만들어내는 브랜치
- **HEAD**
  - 현재 사용 중인 **브랜치의 선두 부분**
    - 일반적으로 master의 선두 부분
  - HEAD 이동 시 사용하는 브랜치가 변경
    - **HEAD를 기준으로 브랜치 이동**이 용이 
    - ~ : 해당 커밋의 조상 브랜치 가리킴
    - ^ : 해당 커밋의 부모를 가리킴 -> 원본이 여러개 있는 경우 몇 번째 원본인지를 가리키는 용도로 사용 
  ```
            HEAD~2   HEAD~1
  HEAD~3      or       or      HEAD
           HEAD~1^1   HEAD^
     o -----> o -----> o -----> o
      \-----> o -----/
           HEAD~1^2
  - HEAD~1^1 : HEAD~1의 부모 중 첫번째(^1)         
  ```
- **stash**
  - 파일의 **변경 내용을 일시적으로 기록**해두는 영역
  - 작업트리와 인댁스 내에서 아직 커밋하지 않은 변경 사항을 일시적으로 저장
- **merge**
  - topic 브랜치를 만든 후, 수정 사항을 저장할 때 master 브랜치의 변경 내역이 없을 때
    - 바로 topic 브랜치로 master 브랜치를 이동(fast-forward)
    - fast-forward를 할 수 있지만 non-fast-forward 병합 옵션을 지정가능(실행한 작업 확인 및 브랜치 관리면에서 유용)
  - topic 브랜치를 만든 후, 수정 사항을 저장할 때 master 브랜치의 변경 내역이 있을 때
    - master 브랜치와 topic 브랜치의 수정사항을 확인 후 하나로 통합
- **rebase**
  - topic 브랜치를 만든 후, 수정 사항을 저장할 때 master 브랜치의 변경 내역이 있을 때
    - topic 브랜치를 master 브랜치에 rebase하면, topic 브랜치의 이력이 master 브랜치 뒤로 이동
    - 하나의 줄기처럼 보임
  ```
  - merge
     A -----> B -----> C -----> D
      \-----> X -----/
  - rebase
     A -----> B -----> C -----> D -----> X'      
  ```
  
  | merge | rebase |
  | --- | --- |
  | 변경 내용 이력이 모두 그대로 남음 | 변경 내용 이력이 그대로 남지 않음 |
  | 이력이 복잡해짐 | 이력이 단순해짐 |
  | 정확한 이력 정보를 가짐 |
   
#### 브랜치 모델
- Main Branch
  - master 브랜치와 develop 브랜치 사용
  - master : 배포 가능한 상태만을 관리함
    - 태그를 사용하여 배포 번호를 기록
  - develop : 통합 브랜치의 역할, 평소에는 이 브랜치를 기반으로 개발
- Feature Branch
  - 토픽 브랜치의 역할을 담당
  - 작업이 필요할 때에 'develop' 브랜치로 부터 분기
    - 개발이 완료되면 'develop' 브랜치로 병합하여 공유    
- Release Branch
  - 모든 기능이 정상적으로 동작하는지 확인
    - 기능을 점검하며 발견한 버그 수정 사항은 'develop' 브랜치에도 적용 
  - 관례적으로 브랜치 이름 앞에 'release-'를 붙임
  - 릴리즈를 위한 최종적인 버그 수정 등의 개발을 수행
    - 배포 가능한 상태가 되면 'master' 브랜치로 병합 및 병합한 커밋에 릴리즈 번호 태그 추가  
- Hotfix Branch
  - 긴급하게 수정을 해야 할 필요가 있을 경우 'master' 브랜치로부터 분기하는 브랜치
  - 관례적으로 브랜치 이름 앞에 'hotfix-'를 붙임
  - 'develop' 브랜치 개발 중 발견한 큰 문제점이 있을 때, 빠르게 'master' 브랜치로부터 직접 브랜치를 만들어 필요한 부분만을 수정
  - 변경사항은 'develop' 브랜치에도 병합하여 문제  
---

### Git 명령어
#### 사용자 등록
- git config --global user.name "Git ID"
- git config --global user.email "email"

#### 깃 환경 초기화
- git init

#### 작업 트리, 인덱스 상태 확인
- git status

#### 파일을 인덱스에 등록
- git add "File/Folder name"

#### 인덱스에 등록되어 있는 파일/폴더를 로컬 저장소에 등록
- git commit -m "commit message"

#### 커밋 이력 확인
- git log
- git log --graph --oneline
  - -graph 옵션 : 그림 형태로 이력 흐름 표시
  - -oneline 옵션 : 한 줄로 커밋 정보 표시

#### 원격 저장소의 주소 등록
- git remote add "name" "url"
  - 원격 저장소의 주소(url)를 이름(name)으로 등록
  - push나 pull 실행 시 원격 저장소명을 생략하면, origin이라는 이름의 원격 저장소를 사용 -> origin 이름을 붙이는게 일반적이 됨 

#### 로컬 저장소에 변경된 이력을 원격 저장소에 공유
- git push "name" "branch"
  - 경로의 주소(name)로 브랜치를 공유
  - -u 옵션 : 사용 시 이후에 브랜치명 지정 생략 가능

#### 원격 저장소의 내용을 PC로 받아오기
- git clone "url" "directory"
  - 경로의 주소(url)를 해당 디렉토리로 복제

#### 원격 저장소의 내용이 변경되었을 때 업데이트 하기
- git pull "name" "branch"
  - 경로의 주소(name)에 있는 파일의 변경사항을 branch로 받아옴
  - 같은 부분을 수정하지 않았다면, 자동적으로 파일을 병합(merge) 해줌
  - 같은 부분을 수정하였다면, Git에서 충돌이 발생한 곳을 표시해줌 -> 수정 필요
  ```
  <<<<<<< HEAD
  printf("%d", a + 2);
  =======
  printf("%d", a);
  >>>>>>> 
  - 로컬 저장소 내용
  - ==== 
  - 원격 저장소 내용
  ```
  
---

### Git 브랜치관련 명령어
#### 브랜치 생성
- git branch "name"

#### 브랜치 이동
- git checkout "name"
  - 해당 브랜치로 이동
  - 기존에 존재하던 브랜치라면 브랜치 안에 있는 **마지막 커밋 내용이 작업 트리에 보여짐**
  - **커밋하지 않은 변경 사항이 인덱스와 작업 트리에 남아 있는 채로 다른 브랜치로 전환(checkout)하면, 변경 사항은 기존 브랜치가 아닌 전환된 브랜치에서 커밋 가능**
    - 커밋 가능한 변경 내용 중 전환된 브랜치에서도 변경이 존재하는 경우에는 전환(checkout)에 실패할 수 있음
    - stash를 이용하여 충돌을 피하게 한 뒤 전환   
- git checkout -b "name"
   - 해당 브랜치 생성 및 이동 

#### 브랜치 수정 내역 master 브랜치 반영(merge)
1) git checkout master
2) git merge "name"
3) git commit -m "message"
- fast-forward 병합
   ```
   o -----> o -----> o
   ```
- non-fast-forward 병합
   ```   
   o -----> o -----> o
    \-----> o -----/
   ```

#### 브랜치 수정 내역 master 브랜치 반영(rebase)
1) git checkout "name"
// rebase 부분
2) git rebase master
3) git rebase --continue
  - rebase 취소 : git rebase --abort
// rebase 한 곳으로 이동하여 merge해야 HEAD가 이동함
4) git checkout master
5) git merge "name"

#### 브랜치 삭제
- git branch -d "name"

---

### 새로운 파일 업데이트
1) **git init**

2) **git remote add [저장소명] [주소]**
   - ex) git remote add origin https://github.~

3) **git add [변경된 파일]**
   - ex) git add . (변경된 모든 파일들)

4) **git commit -m [메시지 : 로그 관리를 위해 필요]**
   - ex) git commit -m "v1.0_init"

5) **git push [로컬 저장소명] [원격 저장소명]**
   - ex) git push origin master

---

### .gitignore를 반영하지 못했을 때 
1) git rm -r --cached .

2) git add .

3) git commit -m "v1.1_apply gitigrnoe"

---

### git 작업 취소
- **git reset --mixed HEAD [파일명]**
  - **add를 취소**(mixed 옵션은 default)
  - 파일명 추가 시, 특정 파일 add 취소

- **git reset --soft HEAD^**
  - commit 취소 및 해당 파일들(staged) 워킹 디렉터리에 보존
  - 수정한 파일들 존재 및 add O
  - HEAD == @

- **git reset --mixed HEAD^**
  - commit 취소 및 해당 파일들(unstaged) 워킹 디렉터리에 보존
  - 수정한 파일들은 존재하는 상태, add X

- **git reset --hard HEAD^**
  - commit 취소 및 해당 파일들(unstaged) 워킹 디렉터리에 보존 X
  - 마지막 커밋 이후의 워킹 디렉토리 상태로 변경

- **git reset [commit id]**
  - **특정 커밋**으로 되돌림
  - id 보는 방법은 **git log -g or git reflog**

- **git commit --amend**
  - git commit message 변경

- **git reset --hard ORIG_HEAD**
  - **pull을 취소**

- **git reset --merge ORIGI_HEAD**
  - **merge를 취소**
  - pull이나 merge를 하는 경우에 ORIG_HEAD를 남김
  - 이를 이용하여 되돌림

- **push 취소**
  1) commit 취소
  2) commit
  3) push -f (강제 푸쉬)
