# flask

## 목차

- Threading


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
