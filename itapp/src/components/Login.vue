<template>
  <div class="login_container">
    <h2>电商后台管理系统</h2>
    <div class="login_box">
      <!-- 头像 -->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt />
      </div>
      <!-- 表单 -->
      <!-- 
        rules ===> 校验规则对象
        :model ==>数据绑定
        ref ==>引用 表单实例
      -->
      <el-form
        ref="LoginFormRef"
        :model="loginForm"
        label-width="0"
        :rules="LoginFormRules"
        class="login_form"
      >
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="el-icon-lock" type="password"></el-input>
        </el-form-item>

        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import Cookies from 'js-cookie'
export default {
  data() {
    return {
      // 这是登陆表单数据绑定对象
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      //表单验证规则对象
      LoginFormRules: {
        // 验证用户名是否合法
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在3到10个字符', trigger: 'blur' }
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在6到15个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    //点击重置按钮，重置表单
    resetLoginForm() {
      this.$refs.LoginFormRef.resetFields()
    },
    login() {
      this.$refs['LoginFormRef'].validate(async valid => {
        if (valid) {
          const { data: res } = await this.$http.post('login', this.loginForm)
          if (res.meta.status == 200) {
            Cookies.set('username', this.loginForm.username)
            this.$message({
              message: '登录成功',
              type: 'success'
            })
            window.sessionStorage.setItem('token', res.data.token)
            this.$router.push('/home')
          } else {
            this.$message({
              message: res.meta.msg,
              type: 'error'
            })
          }
        } else {
          return false
        }
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container {
  background-image: url('../assets/fonts/bg.jpg');
  height: 100%;
}
h2 {
  color: #fff;
  position: absolute;
  left: 50%;
  top: 10%;
  transform: translate(-50%);
}
.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.avatar_box {
  width: 130px;
  height: 130px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
}
.avatar_box img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: #eee;
}
.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 20px;
  box-sizing: border-box;
}

.btns {
  display: flex;
  justify-content: flex-end;
}
</style>>

