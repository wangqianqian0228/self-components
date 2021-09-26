<template>
  <!-- 阻止表单默认提交 -->
  <form action="">
    <slot></slot>
  </form>
</template>

<script>
export default {
  provide() {
    return {
      eform: this,
    };
  },
  props: {
    model: {
      type: Object,
      default: () => ({}),
    },
    rules: {
      type: Object,
      default: () => ({}),
    },
  },
  methods: {
    async validate(callback) {
      /* 
      1. 找到父级form下面的所有的form-item
      2. 看所有的form-item是否符合规范?怎么看 
          让所有的form-item都校验完(执行validate()),全部通过返回true,否则,打印出错误信息.
      */

      let children = this.$children; // 集合
      let arr = []; //创建一个空数组,来存放找到的所有的form-item
      function findFormItem(children) {
        children.forEach((child) => {
          // 找到需要校验的e-form-item
          if (child.$options.name === "e-form-item" && child.prop) {
            arr.push(child);
          }
          if (child.$children) {
            findFormItem(child.$children);
          }
        });
      }
      findFormItem(children);
      try {
        //等待这3个Promise都执行完
        // brr: [ promise, promise, promise]
        let brr = arr.map((e) => e.validate());
        console.log(brr); // 是一个promise
        await Promise.all(brr); // 等待所有的promise都执行完之后,再去执行下面的代码
        callback(true); //执行传过来的回调
      } catch {
        // 其中有一个promise有错误
        callback(false);
      }
    },
  },
};
</script>

<style></style>
