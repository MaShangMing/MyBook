![image-20200312200800644](C:\Users\马睿序\AppData\Roaming\Typora\typora-user-images\image-20200312200800644.png)

```vue
父组件：
<apply-form-dialog v-if="showApplyDialog" :xeguid='xeguid' :showApplyDialog.sync="showApplyDialog"></apply-form-dialog>
```

```js
子组件：
closeDialog () {
    this.$emit('update:showApplyDialog', false);
}
```

通过update修饰符，更新父组件数据，以达到子->父组件的通信。