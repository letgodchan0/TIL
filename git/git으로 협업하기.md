# 🌱 Git으로 협업하기

1. GitHub Repository 생성
2. Repository - settings - Collaborators에서 초대
3. 팀원들 해당 `Repository` clone으로 받고 기능 별로 개발 시작
4. 기능 개발 시작 전 브랜치 생성 및 이동

```bash
# 브랜치 만들면서 이동
$ git switch -c feature/login
```

5. 특정 기능 개발 진행
6. 기능 개발 완료 후 commit, push

```bash
# 커밋
$ git commit -m "커밋 메세지"

# 푸쉬
$ git push origin feature/login
```

7. GitHub에서 내가 푸쉬한 내용 `merge request` 하기
8. 다른 팀원들이 `pull request` 확인 후 `merge`
9. GitHub에 반영 된 상태!!

10. 각 팀원들 로컬로 돌아와서 브랜치 `master`로 변경 후 GitHub에서 `pull` 하기

```bash
# 브랜치 변경
$ git switch master

# GitHub에서 프로젝트 변경사항 반영하기
$ git pull origin master
```

11. 로컬에 `pull` 하면서 프로젝트 변경사항이 반영이 되었음, 이제 기존의 개발 완료했던 브랜치 삭세

```bash
$ git branch -d feature/login

# master와 관계 없이 강제로 삭제하려면 
$ git branch -D feature/login
```



" 위 과정이 기능 하나를 개발 완료했을 때의 과정 "