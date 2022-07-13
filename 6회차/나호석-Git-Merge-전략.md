# Git Merge 전략

## Merge 기본 개념

- 2개의 다른 branch를 하나로 병합(merge)

### Fast-Foward (ff)

![ff](https://git-scm.com/book/en/v2/images/basic-branching-4.png)

- 빨리 감기(fast-foward).
- 대상 branch가 기존 branch의 commit 이력을 모두 같고 있을 때 가능. (그림에서 master와 hotfix)
- 분기한 브랜치의 commit history가 기존 branch의 commit history를 포함하고 있는가.
- 별도의 merge commit이 생기지 않고 head만 끝으로 이동.
- `git merge {branch name}`

![ff merge](https://git-scm.com/book/en/v2/images/basic-branching-5.png)

### Fast-Foward가 아닐 때 (no-ff)

![no-ff](https://git-scm.com/book/en/v2/images/basic-merging-1.png)

- 분기한 branch의 commit history가 기존 branch의 commit history를 포함 못할 때.
- 두 branch의 commit history를 모두 포함할 새로운 commit이 필요.(merge commit)
- 각 branch의 head + 공통 조상 하나 = 총 commit 3개 -> 3-way Merge
- 3-way Merge의 결과가 merge commit
- `git merge --no-ff {branch name}`

![no-ff merge](https://git-scm.com/book/en/v2/images/basic-merging-2.png)

### Conflict

- 3-way Merge가 실패.
- 두 branch에서 같은 파일의 한 부분을 동시에 수정하고 Merge 하면 Git은 어떻게 해야 할 지 모름.
- 개발자가 직접 수동으로 수정.
- conflict가 생긴 파일은 git의 status가 `both modified`로 바뀜.
- Git은 충돌이 난 부분을 표준 형식에 따라 표시.

```
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> feature/test:index.html
```

- 위, 아래 중에 선택 혹은 새로 작성하여 충돌 해결한 후 commit(<<<, ====, >>> 열을 삭제)

## Merge 전략

![Merge 전략](https://user-images.githubusercontent.com/16220817/178788450-bc0be2fd-5178-446f-88a8-0567aaceb1e0.png)

### Merge Commit

![Merge Commit](https://inmoonlight.github.io/assets/images/git-merge.png?style=centerme)

- 일반적인 merge 전략.
- 어떤 브랜치에서 어떻게 merge가 되었는지 파악이 가능.
- 자잘한 commit들이 있을 때 그런 commit들의 기록조차 남아서 가독성이 떨어질 수 있음.
- commit log는 commit을 행한 순서대로 기록되고, merge log는 merge가 된 순서대로 기록.
- commit log의 순서가 merge 순서와 다르기 때문에 history 관리 및 이해가 어려움.

### Squash and Merge

![Squash and Merge](https://inmoonlight.github.io/assets/images/git-squash-merge.png?style=centerme)

- 짓누르다(squash)
- 여러 개의 commit을 하나의 commit으로 짓눌러서 합침.
- merge된 branch의 변경 내역을 하나의 commit으로 합쳐서 병합시켰기 때문에 자잘한 commit으로 가독성이 떨어질 일이 없음.
- 새로운 commit의 제목은 PR 제목이 되고, 합쳐진 commit의 제목은 새로운 commit의 상세 내용이 됨.
- 반대로 하나의 commit만 남기 때문에 어떠한 과정으로 변경해 왔는지 추적이 불가. (atomic commit level로 rollback 하는 것은 불가)

### Rebase and Merge

![Rebase and Merge](https://inmoonlight.github.io/assets/images/git-rebase-merge.png?style=centerme)

- Rebase 기능을 사용해 브랜치를 merge.
- 기준 branch를 다시 base 삼아 commit을 쌓는 느낌.
- commit 히스토리가 단순해짐. (하나의 branch에서 작업한 것 처럼)
- 작업 내역을 그대로 살려서 타겟 branch에 병합시키기 때문에 진행 과정을 확인할 수 있음.
- merge commit이 남지 않기 때문에 병합시킨 브랜치가 언제 병합되었는지의 시점을 알 수 없음.
- 다른 PR이 먼저 merge되는 경우, rebase 작업이 필요할 수 있고 이 때 발생할 수 있는 conflict를 잘 해결해야 한다.

## 출처

- [Git Merge의 기초](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EC%99%80-Merge-%EC%9D%98-%EA%B8%B0%EC%B4%88)
- [git merge 한번에 정리하기](https://kotlinworld.com/277)
- [Git Merge에서 Fast Forward 관계 이해하기](https://otzslayer.github.io/git/2021/12/05/git-merge-fast-forward.html)
- [Git Merge 전략](https://velog.io/@yujuck/Git-Merge-%EC%A0%84%EB%9E%B5)
- [Git의 다양한 머지 전략 비교](https://inmoonlight.github.io/2021/07/11/Git-merge-strategy/)
