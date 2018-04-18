# windows

## 목차

## 원노트
Windows 10 에서는 쓰지도 않는 cortana가 원노트의 캡쳐 단축키 `Win+S` 를 먹어버린다. 레지스트리 수정을 통해 해당 단축키를 원노트로 다시 되돌릴 수 있다.

```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced]
"DisabledHotkeys"="S"
```
