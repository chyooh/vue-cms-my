<template>
  <div class="login-wrap">
    <div class="ms-login">
      <div class="ms-title">后台管理系统</div>
      <el-form :model="param" :rules="rules" ref="login" label-width="0px" class="ms-content">
        <el-form-item prop="username">
          用户名：
          <el-input v-model="param.username" placeholder="username" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          密码：
          <el-input
            type="password"
            placeholder="password"
            v-model="param.password"
            auto-complete="off"
          ></el-input>
        </el-form-item>
        <div class="login-btn">
          <el-button :loading="loading" type="primary" @click="submitForm()">登录</el-button>
        </div>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  data: function() {
    return {
      param: {
        username: "admin",
        password: "123456"
      },
      rules: {
        username: [{ required: true, message: "请输入用户名", trigger: "blur" }],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }]
      },
      loading: false,
      redirect: "/dashboard"
    };
  },
  methods: {
    submitForm() {
      this.$refs.login.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('user/login', this.param).then(() => {
            this.$router.push({ path: this.redirect })
            localStorage.setItem("lang", "cn")
            localStorage.setItem("username", this.param.username)
            this.loading = false
          }).catch(() => {
            this.loading = false
          })
        } else {
          console.log('提交错误!!')
          return false;
        }
      })
    }
  }
};
</script>

<style lang="scss" scoped>
$color:#fff;
.login-wrap {
  position: relative;
  width: 100%;
  height: 100%;
  background-image: url(../../assets/img/login-bg.jpg);
  background-size: 100%;
}
.ms-title {
  width: 100%;
  line-height: 50px;
  text-align: center;
  font-size: 24px;
  color: $color;
  font-weight: bolder;
  border-bottom: 1px solid #ddd;
}
.ms-login {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 350px;
  margin: -190px 0 0 -175px;
  border-radius: 5px;
  background: rgba(255, 255, 255, 0.3);
  overflow: hidden;
}
.ms-content {
  padding: 30px 30px;
}
.login-btn {
  text-align: center;
}
.login-btn button {
  width: 100%;
  height: 36px;
  margin-bottom: 10px;
}
.login-tips {
  font-size: 12px;
  line-height: 30px;
  color: #fff;
}
</style>