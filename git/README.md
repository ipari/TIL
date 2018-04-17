# git

## 목차

- 초기 설정
- rebase
  - resolve conflict
  - interactive
- reset

## 초기 설정

```
$ git config --global user.name "my name"
$ git config --global user.email my@name.com
$ git config --global core.editor vim
```

## rebase

### resolve conflict

충돌난 파일의 정책을 먼저 정한다.

```
git checkout --ours <path>  // local
git checkout --theirs <path>  // remote
// or edit file
```

이후 파일을 추가하고 리베이스를 계속한다.

```
git add <path>
git rebase --continue
```

### interactive
기존 커밋에 대하여 다양한 작업을 할 수 있다.

```
git rebase -i <commit>^
```

## reset

커밋 하나 삭제
```
git reset --hard HEAD~1
```

`--amend` 커밋 취소
```
git reset --soft HEAD@{1}
```
