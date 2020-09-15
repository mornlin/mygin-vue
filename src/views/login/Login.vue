<template>
  <div class="register">
    <b-row class="mt-5">
      <b-col
        md="8"
        offset-md="2"
        lg="6"
        offset-lg="3"
      >
        <b-card title="登录">
          <b-form>
            <b-form-group label="手机号码">
              <b-form-input
                v-model="$v.user.telephone.$model"
                type="text"
                placeholder="请输入手机号码"
                :state="validateState('telephone')"
              ></b-form-input>
              <b-form-invalid-feedback :state="validateState('telephone')">
                手机号码必须为112位
              </b-form-invalid-feedback>
            </b-form-group>
            <b-form-group label="密码">
              <b-form-input
                v-model="$v.user.password.$model"
                type="password"
                placeholder="请输入密码"
                :state="validateState('password')"
              ></b-form-input>
              <b-form-invalid-feedback :state="validateState('password')">
                密码长度必须大于等于6位
              </b-form-invalid-feedback>
            </b-form-group>
            <b-form-group>
              <b-button
                variant="outline-primary"
                @click="login"
                block
              >登录</b-button>
            </b-form-group>
          </b-form>
        </b-card>
      </b-col>
    </b-row>
  </div>
</template>
<script>
import { required, minLength, maxLength } from "vuelidate/lib/validators";

export default {
  data() {
    return {
      user: {
        telephone: "",
        password: "",
      },
      validation: null,
    };
  },
  validations: {
    user: {
      telephone: {
        required,
        minLength: minLength(11),
        maxLength: maxLength(11),
      },
      password: {
        required,
        minLength: minLength(6),
      },
    },
  },
  methods: {
    validateState(name) {
      const { $dirty, $error } = this.$v.user[name];
      return $dirty ? !$error : null;
    },
    login() {
      const api = "http://localhost:8088/api/auth/login";
      this.axios.post(api, { ...this.user }).then((res) => {
        console.log(res.data);
      }).catch((err) => {
        console.log("err:", err.response.data.msg);
      });
    },
  },
};
</script>
<style lang="scss" scoped>
</style>
