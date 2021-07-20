### Vue에서 공통 헤더 만드는 법

```
import axios from 'axios';

// axios 객체 생성
export default axios.create({
  baseURL: 'http://localhost:9999/vue/api',
  headers: {
    'Content-type': 'application/json',
  },
});
```
