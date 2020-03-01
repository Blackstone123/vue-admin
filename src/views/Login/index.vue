<template>
    <div id="login">
        <div class="login-wrap">
            <ul class="menu-tab">
                <li  v-for="item in menuTab" :key="item.id" :class="{'current': item.current}"
                 @click="toggleMenu(item)">
                    {{item.txt}}
                </li>
            </ul>
            <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm"  class="login-form" size="medium">
                <el-form-item prop="username" class="item-form">
                  <label> 邮箱 </label>
                  <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item prop="password" class="item-form">
                  <label> 密码 </label>
                  <el-input type="text" v-model="ruleForm.password" autocomplete="off" minlength="6" maxlength="20"></el-input>
                </el-form-item>
                <el-form-item prop="password2" class="item-form" v-if="model === 'register'">
                  <label> 再次输入密码 </label>
                  <el-input type="text" v-model="ruleForm.password2" autocomplete="off" minlength="6" maxlength="20"></el-input>
                </el-form-item>
                <el-form-item prop="code" class="item-form">
                  <label> 验证码 </label>
                  <el-row :gutter="11">
                    <el-col :span="15"><el-input v-model.number="ruleForm.code" minlength="6" maxlength="6"></el-input></el-col>
                    <el-col :span="9">
                      <el-button type="success" class="block">获取验证码</el-button>
                    </el-col>
                  </el-row>
                </el-form-item>
                <el-form-item class="item-form">
                  <el-button type="danger" @click="submitForm('ruleForm')" class="login-btn block">提交</el-button>
                </el-form-item>
            </el-form>
        </div>
          
    </div>
</template>

<script>
import { stripscript, validateEmail, validatePass, validateVCode} from '@/utils/validate'
export default {
   name: "login",
   data() {
      var validateUsername = (rule, value, callback) => {
        let reg =/^[a-zA-Z0-9_](\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
        if (value === '') {
          callback(new Error('请输入用户名'));
        } else if(!validateEmail(value)) {
          callback(new Error('用户名格式有误'));
        } else {
          callback();
        }
      };
      var validatePassword = (rule, value, callback) => {
        value=stripscript(value)
        this.ruleForm.password = value
        if (value === '') {
          callback(new Error('请输入密码'));
        } else if(!validatePass(value)) {
          callback(new Error('密码格式有误, 密码为6至20位的数字加字母'));
        } else {
          callback();
        }
      };
      var validatePassword2 = (rule, value, callback) => {
        this.ruleForm.password2 = value
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if(value !== this.ruleForm.password) {
          callback(new Error('和上次输入的密码不一致'));
        } else {
          callback();
        }
      };
      var validateCode = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入验证码'));
        } else if (!validateVCode(value)) {
          callback(new Error('请输入六位验证码'));
        } else {
          callback();
        }
       };
       return {
           menuTab: [
               {txt: '登录', current: true, type: 'login'},
               {txt: '注册', current: false, type: 'register'}
           ],
           ruleForm: {
               username: '',
               password: '',
               code: ''
           },
           model: 'login',
           rules: {
               username: [
                 { validator: validateUsername, trigger: 'blur' }
               ],
               password: [
                 { validator: validatePassword, trigger: 'blur' }
               ],
               password2: [
                 { validator: validatePassword2, trigger: 'blur' }
               ],
               code: [
                 { validator: validateCode, trigger: 'blur' }
               ]
           }
      };
   },
   created() {
       
   },
   // 挂载完成自动执行的
   mounted() {
   },
   methods: {
       toggleMenu(data) {
           this.menuTab.forEach(elem => {
               elem.current = false
           });
           console.log(data);
           data.current = true;
           this.model = data.type;
       },
       submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
       }
   }
}
</script>

<style lang="scss" scoped>
#login {
    height: 100vh;
    background-color: #344a5f;
}
.login-wrap {
    width: 330px;
    margin: auto;
}
.menu-tab {
    text-align: center;
    li {
        display: inline-block;
        width: 88px;
        line-height: 36px;
        font-size: 14px;
        color: #fff;
        border-radius: 2px;
        cursor: pointer;
    }
    .current {
        background-color: rgba(0, 0, 0, 0.1)
    }
}
.login-form {
    margin-top: 29px;
    label {
      display: block;
      font-size: 14px;
      color: white;
      margin-bottom: 3px;
    }
}
.item-form {
  margin-bottom: 13px;
}
.block {
  display: block;
  width: 100%;
}
.login-btn {
  margin-top: 19px;
}
</style>