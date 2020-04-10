# 表格导出Excel

```js
let blob = new Blob([res], { type: '.csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel.csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel' });
let objectUrl = window.URL.createObjectURL(blob);
var link = document.createElement('a');
link.href = objectUrl;
link.download = '文件名';
link.click();
window.URL.revokeObjectURL(link.href);
```

![image-20200408101939198](C:\Users\马睿序\AppData\Roaming\Typora\typora-user-images\image-20200408101939198.png)

![image-20200408102156071](C:\Users\马睿序\AppData\Roaming\Typora\typora-user-images\image-20200408102156071.png)

```js
// .xlsx格式
application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;charset=utf-8

// .xls格式
.csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel.csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel
```

