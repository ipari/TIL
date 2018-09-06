# flask

## 목차

- 파일 읽기
  - Static 폴더 내의 파일
  - 일반 파일
- Threading
- mod_wsgi 한글 문제


## 파일 읽기

### Static 폴더 내의 파일

#### 템플릿에서 읽기
`url_for()` 를 활용한다.

```
<script src="{{ url_for('static', filename='js/main.js') }}"></script>
<img src="{{ url_for('static', filename=scene.image) }}" />
```

#### 코드에서 읽기
`app.static_folder()` 로 절대 경로를 구해야 한다.

```
from flask import current_app

def load():
    path = os.path.join(current_app.static_folder, 'blah.yml')
    with open(path, 'r') as f:
        return yaml.load(f)
```

### 일반 파일
`app.open_resource()`로 아무 파일을 읽을 수 있다.  
http://flask.pocoo.org/docs/1.0/api/#flask.Flask.open_resource

```
with app.open_resource('schema.sql') as f:
    contents = f.read()
    do_something_with(contents)
```


## Threading

백그라운드 작업을 하려면 아래와 같이 `threading` 을 사용하면 된다.

※ 주의점 : Flask 디버그 모드를 켜면 스레드가 두 번씩 실행된다.

```
app = Flask(__name__)


@app.route("/")
def hello():
  return "Hello World!"


def loop():
  while True:
    print("yay!")
    time.sleep(1)


if __name__ == "__main__":
  thread = threading.Thread(target=loop, args=())
  thread.daemon = True
  thread.start()
  app.run(host="0.0.0.0, threaded=True)
```


## mod_wsgi

### 디버깅
공개 서버에서 디버그 모드를 다는 건 좋지 않지만 필요한 경우가 있다.  
https://stackoverflow.com/questions/36208236/why-isnt-flask-giving-me-an-interactive-debugger

아래 내용을 적당한 곳에 추가한다.
```
from werkzeug.debug import DebuggedApplication

app.debug = True
app.wsgi_app = DebuggedApplication(app.wsgi_app, evalex=True)
```

mod_wsgi로 Flask 앱을 실행하면 디버깅 핀을 입력하기 난감하다.
아래와 같이 wsgi 파일에 핀을 미리 환경변수로 지정하여 해결할 수 있다.

```
import os
os.environ['WERKZEUG_DEBUG_PIN'] = '1111'
```

### 한글 인코딩 에러
Python3 에서 Flask로 만든 앱을 바로 실행할 때는 한글 문제가 없다가, `libapache2-mod-wsgi-py3` 으로 아파치 위에서 돌릴 때 한글 문제가 발생하였다. 주소에 있는 파라미터를 utf-8이 아닌 ascii로 받아 코드에서 `UnicodeEncodeError`가 발생하던 것.

구글링하여 아래의 방법을 시도해보았으나 답이 아니었다.
- conf 파일의 WSGIDaemonProcess에 `lang`과 `locale` 지정하기
  - https://modwsgi.readthedocs.io/en/develop/configuration-directives/WSGIDaemonProcess.html
- wsgi 파일에 `os.enviorn['LANG']` 설정하기

그러다가 아래 방법을 사용하니 해결되었다. (위의 내용은 안해도 되었다)

`$ sudo vi /etc/apache2/envvars` 에 들어가서 아래 부분의 주석을 푼다.

```
## Uncomment the following line to use the system default locale instead:
# . /etc/default/locale
```
