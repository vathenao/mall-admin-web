<template>
    <el-card class="register-form-layout">
        <!-- 根标签 -->
        <el-form
        :model="form"
        status-icon
        :rules="rules"
        ref="form"
        label-width="100px"
        class="demo-ruleForm"
        >

        <el-form-item label="用户名" prop="username">
            <el-input v-model.username="form.username"></el-input>
        </el-form-item>

        <el-form-item label="邮箱" prop="email">
            <el-input type="email" v-model="form.email" auto-complete="on"></el-input>
        </el-form-item>

        <el-form-item label="密码" prop="password">
            <el-input type="password" v-model="form.password" auto-complete="on"></el-input>
        </el-form-item>

        <el-form-item label="确认密码" prop="checkPass">
            <el-input type="password" v-model="form.checkPass" auto-complete="on"> </el-input>
        </el-form-item>
        
        <el-form-item label="昵称" prop="nickname">
            <el-input v-model.nickname="form.nickname"></el-input>
        </el-form-item>
        
        <el-form-item>
            <el-button type="primary" @click.native.prevent="handleSummit">提交</el-button>
            <!--el-button  @click.native.prevent="handleReset">重置</el-button-->
        </el-form-item>
        </el-form>
    </el-card>
</template>
   
  <script>
  import {isvalidUsername} from '@/utils/validate';
  export default {
    data () {
        const validateUsername = (rule, value, callback) => {
        if (!isvalidUsername(value)) {
          callback(new Error('请输入正确的用户名'))
        } else {
          callback()
        }
      };
      const validatePass = (rule, value, callback) => {
        if (value.length < 3) {
          callback(new Error('密码不能小于3位'))
        } else {
          callback()
        }
      };
      const validateChkPass = (rule, value, callback) => {
        if (value === this.form.password) {
          callback()
        } else {
          callback(new Error('两次输入的密码不一致'))
        }
      };

        return {
            form:{
                username:'',
                nickname:'',
                password:'',
                checkPass:''
            },
            rules:{
                username: [{required: true, trigger: 'blur', validator: validateUsername}],
                password: [{required: true, trigger: 'blur', validator: validatePass}],
                checkPass: [{required: true, trigger: 'blur', validator: validateChkPass}]
            }
        }
    },
    methods:{
        handleSummit(){
            console.log(this.form)
            this.$refs.form.validate(valid => {
                if (valid) {
                    this.loading = true;
                    this.$store.dispatch('Register', this.form).then(() => {
                        this.loading = false;
                    }).catch(() => {
                        this.loading = false
                    })
                } else {
                    console.log('参数验证不合法！');
                    return false
                }
            })
        }
    }
  };
  </script>
   
  <style scoped>
    .register-form-layout {
        position: absolute;
        left: 0;
        right: 0;
        width: 720px;
        margin: 140px auto;
        border-top: 10px solid #409EFF;
    }
  </style>