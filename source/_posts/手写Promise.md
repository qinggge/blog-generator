---
title: 手写Promise
date: 2019-06-10 21:53:47
tags: [JavaScript,Promise] 
---
直接上代码
```javascript
function Promise(executor) {
  let self = this;
  
  let status = "pending"; // 等待态
  let value = undefined;
  let reason = undefined;
  //成功执行
  function resolve(value) {
    if (status == 'pending') {
      self.value = value;
      self.status = "resolve";
    }
  }
  //执行失败
  function reject(reason) {
    if (status == 'pending') {
      self.reason = reason;
      self.status = "reject";
    }
  }
  
  try {
    executor(resolve, reject);
  } catch (e) {
    reject(e); // 捕获异常
  }
  
  // then方法
  Promise.prototype.then = function(reject, resolve) {
    let self = this;
    if (this.status == 'resolve') {
      reject(self.value);
    }
    if (this.status == 'reject') {
      resolve(self.reason);
    }
  }
}
```