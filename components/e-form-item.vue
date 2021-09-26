<template>
  <div>
    <label for="" v-if="label">{{ label }}:</label>
    <div class="slot">
      <slot></slot>
      {{ message }}
    </div>
  </div>
</template>

<script>
import Schema from "async-validator";
export default {
  inject: ["eform"],
  name: "e-form-item",
  props: {
    label: String,
    prop: String,
  },
  data() {
    return {
      message: "",
    };
  },
  mounted() {
    /*  父级对传来的这个值进行校验
    1. 要用到父级e-form的rules => 直接用provide 和inject来获取
    2. rules和prop进行匹配,对val进行校验 => 用第三方库来校验
     */
    this.$on("validator", function () {
      this.validate()
      // console.log(this.validate(val))
    });
  },
  methods: {
    // 校验数据
    validate() {
      /* 
      1. 获取规则
      2. 获取当前数据,可以直接拿$emit中的value,也可以直接拿form中的数据,反正他们已经双向绑定了,拿谁都可以 
      3. 后面发现拿$emit中的value不方便,数据来源太单一.
      */
      let rules = this.eform.rules[this.prop];
      let val = this.eform.model[this.prop]
      // console.log({ rules });
      //  定义属性描述规则
      let descriptor = {
        //  rules这里可以是一个数组,也可以是一个对象
        [this.prop]: rules,
      };
      // descriptor分配给schema，创建一个validator
      let validator = new Schema(descriptor);
      // 参数一：要验证的对象、参数二：回调函数,返回值是一个promise
      return validator.validate({ [this.prop]: val }, (errors) => {
        if (errors) {
          this.message = errors[0].message;
        } else {
          this.message = "";
        }
      });
    },
  },
};
</script>

<style>
.slot {
  display: inline-block;
}
</style>