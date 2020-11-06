```js
// 检查文件夹子集中是否有检查
    handleCheckChildren (res) {
      // 如果文件夹中没有检查，且没有子文件夹
      let state = false;
      JSON.stringify(res, (key, value) => {
        if (value) {
          if (value.totalCount === 0) {
            state = false;
          } else if (value.totalCount > 0) {
            state = true;
          }
        }
        return value;
      });
      return state;
    },
```

遍历数组对象中的每一个对象和子对象的key，value