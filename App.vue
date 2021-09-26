<template>
  <div>
    <e-form :model="ruleForm" :rules="rules" ref="ruleForm">
       <!-- prop是model中的某一个字段  -->
       <!-- 是在form-item对字段进行校验的 -->
      <e-form-item label="名称" prop="name">
        {{ruleForm}}
        <e-input v-model="ruleForm.name"></e-input>
      </e-form-item>
      <e-form-item>
        <button  type="button" @click="submitForm('ruleForm')">提交</button>
      </e-form-item>
    </e-form>
    
    
  </div>
</template>

<script>
import eInput from './components/Form/e-input.vue'
import eFormItem from './components/Form/e-form-item.vue'
import eForm from './components/Form/e-form.vue'


export default {
  data(){
    return {
      ruleForm: {
          name: '',
          password: ''
        },
        rules: {
          name: [
            { required: true, message: '请输入名称', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
          password: [
            { required: true, message: '请输入密码', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 10 个字符', trigger: 'blur' }
          ]
        }
    }
  },
  methods: {
    submitForm(formName) {
      // 引用form的validate方法,参数传的是一个回调方法.
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
  },
  components: {
    'e-input': eInput,
    'e-form-item': eFormItem,
    'e-form': eForm
  }
}
</script>

<style>

</style>