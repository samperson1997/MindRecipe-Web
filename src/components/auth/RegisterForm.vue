<template>
  <div class="auth-wrapper">

    <div class="left-form-wrapper">
      <h1>Sign up</h1>

      <el-form :model="registerForm" :rules="rules" ref="registerForm" labelPosition="top">

        <el-form-item label="User name" prop="username">
          <el-input v-model="registerForm.username"></el-input>
        </el-form-item>

        <!--<el-form-item label="E-mail" prop="email">-->
        <!--<el-input v-model="registerForm.email"></el-input>-->
        <!--</el-form-item>-->

        <!--<el-form-item label="Phone number" prop="phone">-->
        <!--<el-input v-model="registerForm.phone"></el-input>-->
        <!--</el-form-item>-->

        <el-form-item label="Password" prop="password">
          <el-input type="password" v-model="registerForm.password" auto-complete="off"></el-input>
        </el-form-item>

        <el-form-item label="Confirm password" prop="confirmPassword">
          <el-input type="password" v-model="registerForm.confirmPassword" auto-complete="off"></el-input>
        </el-form-item>

        <el-form-item>
          <div class="go-sign-in">
            <span>Had Account？</span>
            <el-button
              type="text"
              @click="goToLoginPage"
            >
              Go to sign in
            </el-button>
          </div>
          <div class="sign-in-button-wrapper">
            <el-button type="text" @click="submitForm('registerForm')">Sign up</el-button>
          </div>
        </el-form-item>
      </el-form>
    </div>

    <div class="right-wrapper">
      <img src="../../assets/img/login2.jpeg" height="100%"/>
    </div>
  </div>
</template>

<script>
  import {Button, Input, Form, FormItem, Radio, InputNumber, Select, Option, Tag, Cascader, Message} from 'element-ui'
  import {router} from '../../main'
  import {mapActions} from 'vuex'

  export default {
    name: 'register-form-page',
    components: {
      elButton: Button,
      elInput: Input,
      elForm: Form,
      elFormItem: FormItem,
      elRadio: Radio,
      elInputNumber: InputNumber,
      elSelect: Select,
      elOption: Option,
      elTag: Tag,
      elCascader: Cascader
    },
    data() {
      let checkUsername = (rule, value, callback) => {
        if (!value || value.length === 0) {
          return callback(new Error('Please enter user name'))
        } else {
          callback()
        }
      };
      let checkEmail = (rule, value, callback) => {
        if (!value || value.length === 0) {
          return callback(new Error('Please enter e-mail'))
        } else if (!/^([\w-_]+(?:\.[\w-_]+)*)@((?:[a-z0-9]+(?:-[a-zA-Z0-9]+)*)+\.[a-z]{2,6})$/i.test(value)) {
          return callback(new Error('Please enter correct e-mail format'))
        } else {
          callback()
        }
      };
      let checkPhone = (rule, value, callback) => {
        if (!value || value.length === 0) {
          return callback(new Error('Please enter phone number'))
        } else if (!/^1[34578]\d{9}$/.test(value)) {
          return callback(new Error('Please enter correct phone number format'))
        } else {
          callback()
        }
      };
      let validatePassword = (rule, value, callback) => {
        if (!value || value.length === 0) {
          callback(new Error('Please enter password'))
        } else {
          callback()
        }
      };
      let validateConfirmPassword = (rule, value, callback) => {
        if (!value || value.length === 0) {
          callback(new Error('Please enter password again'))
        } else if (value !== this.registerForm.password) {
          callback(new Error('Password not consistent!'))
        } else {
          callback()
        }
      };

      return {
        rules: {
          password: [
            {required: true, validator: validatePassword, trigger: 'blur'}
          ],
          phone: [
            {required: true, validator: checkPhone, trigger: 'blur'}
          ],
          username: [
            {required: true, validator: checkUsername, trigger: 'blur'}
          ],
          email: [
            {required: true, validator: checkEmail, trigger: 'blur'}
          ],
          confirmPassword: [
            {required: true, validator: validateConfirmPassword, trigger: 'blur'}
          ]
        },
        registerForm: {
          username: '',
          password: '',
          confirmPassword: '',
        },
      }
    },
    methods: {
      goToLoginPage() {
        router.push({name: 'LoginPage'})
      },
      submitForm(data) {
        this.$refs[data].validate((valid) => {
          if (valid) {
            Message.success('Successfully registered. Please sign in')
            router.push({name: 'LoginPage'})
          } else {
            Message.error('Please enter correct information')
          }
        })
      }
    }
  }
</script>

<style scoped src="./RegisterForm.css"></style>
