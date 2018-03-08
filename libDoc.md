## moment.js

```js
//es6 import引入
import moment from 'moment' 
//nodejs 引入
var moment = require('moment')

//把时间对象格式化为文本 2018-01-09 12:14:27
moment(new Date).format('YYYY-MM-DD hh:mm:ss')

//把文本解析为date对象   
moment('2018-01-09 12:14:27')
```

