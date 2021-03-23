# Git_Instruction
- 깃을 사용할 때 필요한 명령어들 정리

### Index
- [git 파일 업로드](https://github.com/KimUJin3359/Git_Instruction#%EC%83%88%EB%A1%9C%EC%9A%B4-%ED%8C%8C%EC%9D%BC-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8)
- [git pull](https://github.com/KimUJin3359/Git_Instruction#git-gui%EB%A5%BC-%ED%86%B5%ED%95%B4-%EC%88%98%EC%A0%95%EC%9D%84-%ED%96%88%EC%9D%84-%EA%B2%BD%EC%9A%B0)
- [git .ignore 반영해서 다시 업로드](https://github.com/KimUJin3359/Git_Instruction#gitignore%EB%A5%BC-%EB%B0%98%EC%98%81%ED%95%98%EC%A7%80-%EB%AA%BB%ED%96%88%EC%9D%84-%EB%95%8C)
- [git 작업(add, commit, pull, merge) 취소](https://github.com/KimUJin3359/Git_Instruction#git-%EC%9E%91%EC%97%85-%EC%B7%A8%EC%86%8C)

---

### Git을 혼자 사용한다면
- Branch를 사용할 필요가 없음
- 별 다른 기능을 사용하지 않아도 됨

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

### Git GUI를 통해 수정을 했을 경우
- **git pull [로컬 저장소명] [원격 저장소명]**
  - ex) git pull origin master

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

---

### Git을 같이 사용한다면
   ![다운로드](https://user-images.githubusercontent.com/50474972/112145243-76ccb700-8c1d-11eb-8018-875fc9aea51a.png)
- 이상적인 branch 관리
- branch를 사용하여 각자의 환경에서 작업
- 다른 사람의 repository를 가져와서 작업하는 것이라면 fork -> pull request
- 같이 공유된 repository를 작업하는 것이라면 'fetch, merge' or 'pull' 

#### 공부 후 추가 예정

