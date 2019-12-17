### 상황 1. fast-foward

> fast-foward는 feature 브랜치 생성된 이후 master 브랜치에 변경 사항이 없는 상황

1. feature/test branch 생성 및 이동	

   ```bash
   $ git checkout -b feature/test
   Switched to a new branch 'feature/test'
   ```

2. 작업 완료 후 commit

   ```bash
   $ touch test.txt
   
   $ git add test.txt
   
   $ git commit -m 'complete test'
   [feature/test 3d2e0ea] complete test
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 test.txt
   ```


3. master 이동

   ```bash
   (feature/test) $ git checkout master
   Switched to branch 'master'
   
   (master) $ 
   ```


4. master에 병합

   ```bash
   (master) $ git merge feature/test
   
   Updating 1292e45..3d2e0ea
   Fast-forward
    test.txt | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 test.txt
   ```


5. 결과 -> fast-foward (단순히 HEAD를 이동)

   ```bash
   Updating 1292e45..3d2e0ea
   Fast-forward
    test.txt | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 test.txt
   ```

6. branch 삭제

   ```bash
   $ git branch -d feature/test
   error: Cannot delete branch 'feature/test' checked out at 'C:/Users/최시영/Desktop/네이버'
   ```
   
7. 중간중간 이력 확인

   ```bash
   $ git log --oneline
   3d2e0ea (HEAD -> master, feature/test) complete test
   1292e45 complete index page
   ed79212 add readme
   ```

   * --online은 한줄로 보겠다는 뜻

   

---

### 상황 2. merge commit

> 서로 다른 이력(commit)을 병합(merge)하는 과정에서 다른 파일이 수정되어 있는 상황
>
> git이 auto merging을 진행하고, commit이 발생된다.

1. feature/signout branch 생성 및 이동

   ```bash
   # $ git branch feature/signout
   # 생성
   # $ git checkout feature/signout
   # 이동
   $ git checkout -b feature/signout
   # 생성 및 이동(합한 명령어)
   ```

2. 작업 완료 후 commit

   ```bash
   $ touch signout.html
   $ git add .
   $ git commit -m 'complete signout'
   [feature/signout c99ef6c] complete signout
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 signout.html
   ```

3. master 이동

   ```bash
   $ git checkout master
   Switched to branch 'master'
   ```

4. *master에 추가 commit 이 발생시키기!!*

   * **다른 파일을 수정 혹은 생성하세요!**(+중간중간 log 확인)

   ```bash
   $ git log --oneline
   3d2e0ea (HEAD -> master, feature/test) complete test
   1292e45 complete index page
   ed79212 add readme
   
   $ touch hotfix.txt
   $ git add .
   $ git commit -m 'hotfix on master'
   [master ea47e6d] hotfix on master
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 hotfix.txt
   
   $ git log --oneline
   ea47e6d (HEAD -> master) hotfix on master
   3d2e0ea (feature/test) complete test
   1292e45 complete index page
   ed79212 add readme
   ```

5. master에 병합

   ```bash
   (master) $ git merge feature/signoutgit 
   Merge made by the 'recursive' strategy.
    signout.html | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 signout.html
   ```

6. 결과 -> 자동으로 *merge commit 발생*

   * vim 편집기 화면이 나타납니다.
   
   * 자동으로 작성된 커밋 메시지를 확인하고, `esc`를 누른 후 `:wq`를 입력하여 저장 및 종료를 합니다.
      * `w` : write
      * `q` : quit
      
   * 커밋이  확인 해봅시다.
   
7. 그래프 확인하기

   ```bash
   $ git log --oneline --graph
   *   6726c47 (HEAD -> master) Merge branch 'feature/signout'
   |\
   | * c99ef6c (feature/signout) complete signout
   * | ea47e6d hotfix on master
   |/
   * 3d2e0ea (feature/test) complete test
   * 1292e45 complete index page
   * ed79212 add readme
   
   $ git log --oneline
   6726c47 (HEAD -> master) Merge branch 'feature/signout' (master)
   ea47e6d hotfix on master                                (master)
   c99ef6c (feature/signout) complete signout              (branch)
   3d2e0ea (feature/test) complete test
   1292e45 complete index page
   ed79212 add readme
   ```

   * feature을 먼저 밑에 깔고 합친다.

8. branch 삭제

   ```bash
   $ git branch -d feature/board
   ```
   
   

---

### 상황 3. merge commit 충돌

> 서로 다른 이력(commit)을 병합(merge)하는 과정에서 동일 파일이 수정되어 있는 상황
>
> git이 auto merging을 하지 못하고, 해당 파일의 위치에 라벨링을 해준다.
>
> 원하는 형태의 코드로 직접 수정을 하고 merge commit을 발생 시켜야 한다.

1. feature/board branch 생성 및 이동

   ```bash
   $ git checkout -b feature/board
   ```

2. 작업 완료 후 commit

   ```bash
   $ touch board.html
   # README.md 수정
   
   $ git status
   On branch feature/board
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   README.md
   
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           board.html
   
   no changes added to commit (use "git add" and/or "git commit -a")
   
   $ git add .
   $ git commit -m 'complete board & update README'
   [feature/board 05bc5b5] complete board & update readme
    2 files changed, 8 insertions(+)
    create mode 100644 board.html
   
   $ git log --oneline
   05bc5b5 (HEAD -> feature/board) complete board & update readme
   6726c47 (master) Merge branch 'feature/signout'
   ea47e6d hotfix on master
   c99ef6c complete signout
   3d2e0ea (feature/test) complete test
   1292e45 complete index page
   ed79212 add readme
   ```


3. master 이동

   ```bash
   $ git checkout master
   Switched to branch 'master'
   ```


4. *master에 추가 commit 이 발생시키기!!*

   * **동일 파일을 수정 혹은 생성하세요!**

   ```bash
   # README.md 수정(README메모장 켜서 임으로 수정(오류 발생 시키려고))
   
   # 수정 후
   (master) $ git add .
   $ git commit -m ' Update README on master'
   [master 96b5398]  Update README on master
    1 file changed, 5 insertions(+), 1 deletion(-)
   
   $ git log --oneline
   96b5398 (HEAD -> master)  Update README on master
   6726c47 Merge branch 'feature/signout'
   ea47e6d hotfix on master
   c99ef6c complete signout
   3d2e0ea (feature/test) complete test
   1292e45 complete index page
   ed79212 add readme
   ```

5. master에 병합

   ```bash
   $ git merge feature/board
   Auto-merging README.md
   CONFLICT (content): Merge conflict in README.md
   Automatic merge failed; fix conflicts and then commit the result.
   ```
   
   *병합하다가 충돌이 발생하여 자동병합이 실패했고, 충돌을 고친 다음에 commit을 해줘라
   
   


6. 결과 -> *merge conflict발생*

   ```bash
   $ git status
   On branch master
   You have unmerged paths.
     (fix conflicts and run "git commit")
     (use "git merge --abort" to abort the merge)
   
   Changes to be committed:
           new file:   board.html
   
   Unmerged paths:
     (use "git add <file>..." to mark resolution)
           both modified:   README.md
   ```

   


7. 충돌 확인 및 해결( open with code에서 수정 )

   ```bash
   # open with code -> README -> 수정
   
   <<<<<<< HEAD
   # 네이버 프로젝트
   master에서 추가함!
   
   =======
   
   ㅁㄴㅇㄹ
   >>>>>>> feature/board
   ```
   
   * HEAD(현재 상황), 아래에 feature/board 변화 내역들이 각각 표기 되어 있다.
   
   * 원하는 형태로 코드를 수정한다.
   
     


8. merge commit 진행

    ```bash
    $ git add .
    $ git commit
[master 32743fb] Merge branch 'feature/board'
   ( vim화면 뜸: esc + : + w+ q + 엔터 하면 자동으로 병합 )
   ```
   
   * vim 편집기 화면이 나타납니다.
   
   * 자동으로 작성된 커밋 메시지를 확인하고, `esc`를 누른 후 `:wq`를 입력하여 저장 및 종료를 합니다.
      * `w` : write
      * `q` : quit
      
   * 커밋이  확인 해봅시다.
   
9. 그래프 확인하기

   ```bash
   $ git log --oneline --graph
   *   32743fb (HEAD -> master) Merge branch 'feature/board'
   |\
   | * 05bc5b5 (feature/board) complete board & update readme
   * | 96b5398  Update README on master
   |/
   *   6726c47 Merge branch 'feature/signout'
   |\
   | * c99ef6c complete signout
   * | ea47e6d hotfix on master
   |/
   * 3d2e0ea (feature/test) complete test
   * 1292e45 complete index page
   * ed79212 add readme
   ```
   
   


10. branch 삭제

    ```bash
    $ git branch -d feature/board
    Deleted branch feature/board (was 05bc5b5).
    ```
    
    
