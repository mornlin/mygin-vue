<template>
  <div class="register">
    <b-row class="mt-5">
      <b-col
        md="8"
        offset-md="2"
        lg="6"
        offset-lg="3"
      >
        <b-card title="注册">
          <b-form>
            <b-form-group label="姓名">
              <b-form-input
                v-model="$v.user.name.$model"
                type="text"
                placeholder="请输入名称"
              ></b-form-input>
            </b-form-group>
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
                @click="register"
                block
              >注册</b-button>
            </b-form-group>
          </b-form>
        </b-card>
      </b-col>
    </b-row>
  </div>
</template>
<script>
import { required, minLength, maxLength } from "vuelidate/lib/validators";
import storageService from "@/service/storageService";
import userService from "@/service/userService";

export default {
  data() {
    return {
      user: {
        name: "",
        telephone: "",
        password: "",
      },
      validation: null,
    };
  },
  validations: {
    user: {
      name: {

      },
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
    register() {
      userService.register(this.user).then((res) => {
        storageService.set(storageService.USER_TOKEN, res.data.data.token);
        userService.info().then((response) => {
          storageService.set(storageService.USER_INFO, JSON.stringify(response.data.data.user));
          this.$router.replace({ name: "Home" });
        });
      }).catch((err) => {
        // console.log("err:", err.response.data.msg);
        this.$bvToast.toast(err.response.data.msg, {
          title: "数据验证错误",
          variant: "danger",
          solid: true,
        });
      });
    },
  },
};
</script>
<style lang="scss" scoped>
</style>
