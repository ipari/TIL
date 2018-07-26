# Javascript

## 목차

- jQuery 벗어나기
  - $(document).ready
  
## jQuery 벗어나기

### $(document).ready
98% 이상의 브라우저에서 작동하는 방법 (IE8에서는 작동 안함).  
https://stackoverflow.com/questions/799981/document-ready-equivalent-without-jquery

```
document.addEventListener("DOMContentLoaded", function(event) { 
  //do work
});
```
