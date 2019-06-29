<template>
  <el-row type="flex" class="row-bg" justify="center" align="middle">
    <el-col :span="8">
      <el-form
        ref="loginform"
        :model="form"
        :rules="rules"
        label-position="top"
        label-width="80px"
        class="login-form"
      >
        <el-form-item label="用户名" prop="username" required>
          <el-input v-model="form.username"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password" required>
          <el-input v-model="form.password"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('loginform')">登录</el-button>
          <el-button @click="resetForm('loginform')">重置</el-button>
        </el-form-item>
      </el-form>
    </el-col>
  </el-row>
  <!-- <h1>登录页面</h1> -->
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      form: {
        username: "admin",
        password: "123456"
      },
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          { min: 5, max: 12, message: "长度在 5 到 12 个字符", trigger: "blur" }
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 5, max: 12, message: "长度在 6 到 12 个字符", trigger: "blur" }
        ]
      }
    };
  },

  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          axios({
            url: "http://localhost:8888/api/private/v1/login",
            method: "post",
            data: this.form
          }).then(res => {
            if (res.data.meta.status === 200) {
              console.log(res.data.meta.msg);
            }
          });
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  }
};
</script>











<style>
.el-form .el-form--label-top {
  margin: 0 auto;
}
.row-bg {
  height: 100%;
  background-color: #2d434c;
}
.login-form {
  background-color: #fff;
  padding: 30px 20px;
  border-radius: 10px;
  min-width: 400px;
}
</style>

