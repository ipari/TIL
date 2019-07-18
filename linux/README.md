# Linux

## 목차

- SSH Key
  - 기존 SSH Key 추가
  - 사이트 별 다른 키 사용


## SSH Key

### 기존 SSH Key 추가

```
$ eval $(ssh-agent)
$ ssh-add ~/.ssh/id_rsa
```

그냥 추가하면 퍼미션 에러가 난다.  
directory, public key, priviate key 여부에 따라 아래와 같이 설정해야 한다.  
https://superuser.com/questions/215504/permissions-on-private-key-in-ssh-folder

```
$ chmod 700 ~/.ssh
$ chmod 644 ~/.ssh/id_rsa.pub
$ chmod 600 ~/.ssh/id_rsa
```

권한 설정 후에는 잘 된다.

```
$ ssh-add ~/.ssh/id_rsa
```

### 사이트 별 다른 키 사용

`~/.ssh/config` 파일을 아래와 같이 편집한다.

```
Host github.com
  IdentityFile ~/.ssh/id_rsa_1

Host gitlab.com
  IdentityFile ~/.ssh/id_rsa_2
```

`~/.ssh/config` 파일의 권한이 맞지 않으면 에러가 발생한다.

```
$ git push origin develop
Bad owner or permissions on /home/user_id/.ssh/config

$ chmod 600 ~/.ssh/config
```

## Etc.
### `sudo apt-get update` 안될 때
https://askubuntu.com/questions/760574/sudo-apt-get-update-failes-due-to-hash-sum-mismatch
```
sudo apt-get clean
sudo rm -r /var/lib/apt/lists/*
```
