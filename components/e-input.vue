<template>
  <div>
    <input type="text" @input="handleInput" :value="value">
  </div>
</template>

<script>
export default {
  props: {
    value: {},
  },
  methods: {
  /* 1. 双向数据绑定
     2. 对输入数据进行校验,触发父级让父级进行校验 

  */
    handleInput(e){
      this.$emit('input',e.target.value)
      // 找到父级form-item
      let parent = this.$parent
      while(parent){
        if(parent.$options.name === 'e-form-item'){
          parent.$emit('validator')
          return
        }else{
          parent = parent.$parent
        }
      }
    }
  }
}
</script>

<style>

</style>