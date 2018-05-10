# flask

## 목차

- Static 파일 읽기
- Threading


## Static 파일 읽기

### 템플릿에서 읽기
`url_for()` 를 활용한다.

```
<script src="{{ url_for('static', filename='js/main.js') }}"></script>
<img src="{{ url_for('static', filename=scene.image) }}" />
```

### 코드에서 읽기
`app.static_folder()` 로 절대 경로를 구해야 한다.

```
from flask import current_app

def load():
    path = os.path.join(current_app.static_folder, 'blah.yml')
    with open(path, 'r') as f:
        return yaml.load(f)
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
