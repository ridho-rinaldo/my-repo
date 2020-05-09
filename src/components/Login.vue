<template>
  <div class="background">
    <div class="box">
      <p class="text-login">Login</p>
      <div class="field">
        <md-field>
          <md-icon>email</md-icon>
          <label>Email</label>
          <md-input v-model="login.email" type="text"></md-input>
        </md-field>

        <md-field>
          <md-icon>vpn_key</md-icon>
          <label>Password</label>
          <md-input v-model="login.password" type="password"></md-input>
        </md-field>

        <md-button class="md-raised md-primary" style="width: 100%" v-on:click="btnLogin()">LOGIN</md-button>

        <div class="sign-up">
          <p class="sign-up-text">Don't have a Business account yet?</p>
          <p class="sign-up-button" v-on:click="show()">Sign up</p>
        </div>
      </div>
    </div>

    <modal name="sign-up" height="auto" style="background: rgba(0, 0, 0, 0.4)">
      <p class="text-login">Registration</p>
      <div class="field" style="padding: 3% 8%">
        <md-field style="margin: 0px">
          <md-icon>email</md-icon>
          <label>Email</label>
          <md-input v-model="reg.email" type="text"></md-input>
        </md-field>

        <md-field style="margin: 0px">
          <md-icon>vpn_key</md-icon>
          <label>Password</label>
          <md-input v-model="reg.password" type="password"></md-input>
        </md-field>

        <md-field style="margin: 0px">
          <md-icon>vpn_key</md-icon>
          <label>Re-submission Password</label>
          <md-input v-model="reg.resubmitPassword" type="password"></md-input>
        </md-field>

        <md-button class="md-raised md-primary btn-reg" v-on:click="registration()">Sign up</md-button>
      </div>
    </modal>

    <modal
      name="loading"
      width="80px"
      height="auto"
      style="background: rgba(0, 0, 0, 0.4); padding: 10px"
    >
      <md-progress-spinner md-mode="indeterminate"></md-progress-spinner>
    </modal>

    <md-snackbar
      :md-position="'center'"
      :md-duration="5000"
      :md-active.sync="showSnackbar"
      md-persistent
    >
      <span>{{ message }}</span>
      <md-button class="md-raised md-primary" @click="showSnackbar = false">Done</md-button>
    </md-snackbar>
  </div>
</template>

<script>
import axios from "axios";
import router from "../router/index";

export default {
  name: "Login",
  data() {
    return {
      signUp: false,
      showSnackbar: false,
      login: {
        email: "",
        password: ""
      },
      reg: {
        email: "",
        password: "",
        resubmitPassword: ""
      },
      message: "",
      response: {}
    };
  },
  mounted() {},
  methods: {
    btnLogin() {
      this.$modal.show("loading");
      axios({
        method: "POST",
        url: "https://reqres.in/api/login",
        data: this.login,
        headers: {
          "content-type": "application/json"
        }
      })
        .then(result => {
          this.$modal.hide("loading");
          let token = result.data.token;
          router.push({ name: "Dashboard" });
        })
        .catch(err => {
          this.$modal.hide("loading");
          this.message = err;
          this.showSnackbar = true;
        });
    },
    show() {
      this.$modal.show("sign-up");
    },
    registration() {
      this.$modal.show("loading");
      if (this.reg.password !== this.reg.resubmitPassword) {
        this.message = "Password did not same!";
        this.showSnackbar = true;
        this.$modal.hide("loading");
        return false;
      }

      let registration = {
        email: this.reg.email,
        password: this.reg.password
      };
      axios({
        method: "POST",
        url: "https://reqres.in/api/register",
        data: registration,
        headers: {
          "content-type": "application/json"
        }
      }).then(
        result => {
          this.$modal.hide("loading");
          this.message = "Sign up Successfull.";
          this.showSnackbar = true;
          this.$modal.hide("sign-up");

          this.reg = {
            email: "",
            password: "",
            resubmitPassword: ""
          };
        },
        error => {
          this.$modal.hide("loading");
          this.message = error;
          this.showSnackbar = true;
        }
      );
    },
    movePage() {
      router.push({ name: "Dashboard" });
    }
  }
};
</script>

<style lang="scss" scoped>
.background {
  background-image: url("../assets/background-desk.jpg");
  background-size: contain;
  height: 100vh;
  width: auto;
  display: flex;
  padding: 10% 25%;
}
.box {
  height: 100%;
  width: 100%;
  background-color: white;
  border-radius: 20px;
}
.text-login {
  font-family: sans-serif;
  font-size: 30px;
  font-weight: 600;
  margin: 5% 0px 0px 0px;
}
.field {
  padding: 5% 8%;
}
.sign-up {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  margin-top: 5%;
}
.sign-up-text {
  font-family: sans-serif;
  font-size: 15px;
  margin: 0px 2% 0px 0px;
}
.sign-up-button {
  font-family: sans-serif;
  font-size: 15px;
  color: blueviolet;
  margin: 0px;
}
p.sign-up-button:hover {
  text-decoration: underline;
  cursor: pointer;
}
.my-dialog {
  transform: translate(0%, 0%) scale(1);
}
.err-msg {
  margin: 5px 0px 0px;
  text-align: left;
  color: red;
}
.btn-reg {
  width: 100%;
  margin-top: 3%;
}

//-----------
.left {
  height: 100%;
  width: 65%;
}
.right {
  height: 100%;
  width: 35%;
  background-color: #1f2530;
}
.login-container {
  padding: 10% 10%;
}
.text-title {
  text-align: left;
  font-family: sans-serif;
  font-size: 30px;
  color: white;
  margin: 0px;
}
.text-note {
  text-align: left;
  font-family: sans-serif;
  font-size: 14px;
  color: white;
  padding: 2% 0%;
}
</style>