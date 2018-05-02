# git

## 목차

- 초기 설정
- rebase
  - resolve conflict
  - interactive
- reset
- 문제 해결

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

## 문제 해결

### git 한글 파일명

한글 파일명을 사용하면 아래와 같이 나오는 문제가 있다.

```
new file:   "_documents/\354\240\234\354\225\210\354\204\234.md"
```

경로가 영문이 아닌 경우 escape 하기 때문에 발생하는 문제로, 아래 명령어로 해당 기능을 끄면 된다.

```
$ git config --global core.quotepath false
```
