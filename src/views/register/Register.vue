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
                v-model="user.name"
                type="text"
                placeholder="请输入名称"
              ></b-form-input>
            </b-form-group>
            <b-form-group label="手机号码">
              <b-form-input
                v-model="user.telepone"
                type="text"
                placeholder="请输入手机号码"
              ></b-form-input>
            </b-form-group>
            <b-form-group label="密码">
              <b-form-input
                v-model="user.passwrod"
                type="password"
                placeholder="请输入密码"
              ></b-form-input>
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
  methods: {
    register() {
      const api = "http://localhost:8088/api/auth/register";
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
