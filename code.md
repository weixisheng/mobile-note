### 实用代码段

- 回到顶部

  ```javascript
  const scrollTop = () => {
    const t = document.documentElement.scrollTop || document.body.scrollTop;
    const rAF = window.requestAnimationFrame || window.webkitRequestAnimationFrame 
    				|| window.mozRequestAnimationFrame;
    if(t>0){
      rAF(scrollTop);
      window.scrollTo(0,t-t/5);
    }
  }

  scrollTop();
  ```

- 柯里化Promise函数

  ```javascript
  const promisify = func => (...args) =>
      new Promise((resolve, reject) => 
          func(...args, (err, res) => 
              (err ? reject(err) : resolve(res))
              )
          );

  const delay = promisify((d, cb) => setTimeout(cb, d));
  delay(2000).then(() => console.log('print after 2s!'));
  ```

- 统计数组x出现次数

  ```javascript
  const countX = (arr,val) => arr.reduce((a,v)=>(v===val?a+1:a+0),0)

  countX([1,2,3,1,2,1],1) //3
  ```

- 交并差

  ```javascript
  const union = (a,b) => [...new Set([...a,...b])] //并集
  const intersect = (a,b) => {const s = new Set(b);return a.filter(x => s.has(x)) } //交集
  const difference = (a,b) =>{const s = new Set(b);return a.filter(x => !s.has(x))} //差集
  const symmertricDifference = (a,b) => {
    const sA = new Set(a),sB = new Set(b);
    return [...a.filter(x => !sB.has(x)),...b.filter(x => !sA.has(x))]
  }
  ```

- 数组去重

  ```javascript
  const dedupe = arr => [...new Set(arr)]
  const dedupe = arr => Array.from(new Set(arr))
  ```

- 数组平铺到指定深度

  ```javascript
  const flatten = (arr,depth=1) =>
  	depth !==1 
       ? arr.reduce((acc,val)=>acc.concat(Array.isArray(val)?flatten(val,depth-1):val),[])
  	 : arr.reduce((acc,val) => acc.concat(val),[])

  flatten([1, [2], 3, 4]); // [1, 2, 3, 4]
  flatten([1, [2, [3, [4, 5], 6], 7], 8], 2); // [1, 2, 3, [4, 5], 6, 7, 8]
  ```

- pick提取

  ```javascript
  const pick = (obj,arr) => 
  	arr.reduce((acc,curr)=>(acc in obj && (acc[curr] = obj[curr]）,acc),{}) 
  //逗号操作符：处理之后最后表达式(acc)被返回

  pick({a:1,b:2,c:3},['a','b']) //{a:1,b:2}        
  ```

- 复制到剪贴板

  ```javascript
  const copyToClipboard = str => {
    //创建一个新的 <textarea> 元素，用提供的数据填充它，并将其添加到 HTML 文档中
      const el = document.createElement('textarea');
      el.value = str;
      el.setAttribute('readonly', '');
      el.style.position = 'absolute';
      el.style.left = '-9999px';
      document.body.appendChild(el);
    //存储选择的范围
      const selected =
          document.getSelection().rangeCount > 0 ? document.getSelection().getRangeAt(0) : false;
      el.select();
      document.execCommand('copy');//复制到剪贴板
      document.body.removeChild(el);
      if (selected) {
          document.getSelection().removeAllRanges();
          document.getSelection().addRange(selected);//恢复原始选择范围
      }
  };

  copyToClipboard('test text')
  ```

- 生成UUID

  ```javascript
  const UUIDGeneratorBrowser = () =>
    ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, c =>
      (c ^ (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))).toString(16)
    );

  UUIDGeneratorBrowser(); //"36d8c9ab-0e84-45b7-846f-fa5ee06a47e0"
  ```

  ​