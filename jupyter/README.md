# jupyter

## 목차
- 기본 설정
  - 프로파일 생성
  - 비밀번호 설정
  - CSS
  - nbextension
- 작업 팁
  - 다운로드 링크

## 기본 설정

### 프로파일 생성

```
$ jupyter notebook --generate-config
```

### 비밀번호 설정

패스워드 해시를 먼저 얻는다.

```
$ ipython
Python 3.5.2 (default, Nov 23 2017, 16:37:01)
Type 'copyright', 'credits' or 'license' for more information
IPython 6.2.1 -- An enhanced Interactive Python. Type '?' for help.

In [1]: from IPython.lib import passwd

In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:xxxxxxxxxxxxxxxx'
```

`~/.jupyter/jupyter_notebook_config.py` 를 열고 아래 부분 주석을 풀고 위에서 얻은 패스워드 해시를 입력한다.

```
c.NotebookApp.password = 'sha1:xxxxxxxxxxxxxxxx'
```

### CSS
`~/.jupyter/custom/custom.css` 에 내용을 추가한다.

https://gist.github.com/ipari/4b48e35c0ff9a943ba69e65b81334321

### nbextension

```
jupyter nbextension enable --py --sys-prefix widgetsnbextension
```

## 작업 팁

### 다운로드 링크
```
from IPython.display import FileLink
FileLink('path/to/download')
```

### Python2 유니코드 출력
Python3 에서는 이렇게 안해도 된다.

```
aa = [u'가', u'나']

# [u'\uac00', u'\ub098']
print aa

# [u'가', u'나']
print repr(aa).decode('unicode_escape')
```
