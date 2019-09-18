<template>
  <div class="login">
    <el-card class="box-card">
      <div class="title">
        <img src="../../assets/img/logo_index.png" alt />
      </div>
      <el-form :model="ruleForm" :rules="rules" ref="ruleForm" class="ruleForm">
        <el-form-item prop="mobile">
          <el-input v-model="ruleForm.mobile" placeholder="请输入手机号"></el-input>
        </el-form-item>
        <el-form-item prop="code">
          <el-input v-model="ruleForm.code" placeholder="请输入验证码" class="code"></el-input>
          <el-button style="float:right" type="primary">获取验证码</el-button>
        </el-form-item>
        <el-form-item prop="agree">
          <el-checkbox v-model="ruleForm.agree" style="font-size:10px" class="checkbox">
            我已阅读并同意
            <span>用户协议</span>和
            <span>用户条款</span>
          </el-checkbox>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" class="loginbutton" @click="login">登录</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>
export default {
  data () {
    let validator = function (rule, value, callback) {
      value ? callback() : callback(new Error('您需要同意用户协议后才能登陆'))
    }
    return {
      ruleForm: {
        mobile: '',
        code: '',
        agree: false
      },
      rules: {
        mobile: [
          {
            required: true,
            message: '手机号不能为空'
          },
          {
            pattern: /^1[3456789]\d{9}$/,
            message: '请输入正确手机号'
          }
        ],
        code: [
          {
            required: true,
            message: '验证码不能为空'
          },
          {
            pattern: /^\d{6}$/,
            message: '验证码输入错误'
          }
        ],
        agree: [{ validator }]
      }
    }
  },
  methods: {
    login () {
      this.$refs.ruleForm.validate(valid => {
        if (valid) {
          this.$axios({
            methods: 'post',
            url: '/authorizations',
            data: this.ruleForm
          })
            .then(result => {
              window.localStorage.setItem('user-token', result.data.data.token)
            })
            .catch(() => {
              this.$message({
                type: 'warning',
                message: '手机号或者验证码输入错误'
              })
            })
        }
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang='less' scoped>
.login {
  background-image: url("../../assets/img/login_bg.jpg");
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  .box-card {
    width: 440px;
    height: 330px;
    .title {
      text-align: center;
      img {
        height: 45px;
      }
    }
    .checkbox {
      span {
        color: cornflowerblue;
      }
    }
    .code {
      width: 70%;
    }
    .loginbutton {
      width: 100%;
    }
  }
}
</style>
