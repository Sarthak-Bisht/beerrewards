<template>
  <div>
    <form class="needs-validation" @submit="onContinue">
      <div class="form-group">
        <input
          type="email"
          class="form-control"
          placeholder="EMAIL"
          v-model="formValues.email"
          id="email"
          required
        />
      </div>
      <div class="input-group mb-3">
        <input
          :type="passwordButtonStatus ? 'text' : 'password'"
          class="form-control"
          placeholder="PASSWORD"
          v-model="formValues.password"
          id="password"
          required
        />
        <div class="input-group-append">
          <button
            class="btn btn-outline-dark btn-sm"
            type="button"
            id="button-addon2"
            :onclick="toggleButtonStatus"
          >
            {{ passwordButtonStatus ? "Hide" : "Show" }}
          </button>
        </div>
      </div>
      <div class="footer-container">
        <a href="" class="forgot-password">
          Forgot password?
        </a>
        <button class="btn btn-dark continue" type="submit">
          CONTINUE
        </button>
      </div>
      <router-link to="/register" class="register"
        >Register for a new account</router-link
      >
    </form>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "EmailPassword",
  data() {
    return {
      formValues: {
        email: null,
        password: null,
      },
      passwordButtonStatus: false,
    };
  },
  mounted() {},
  components: {},
  methods: {
    async onContinue(e) {
      e.preventDefault();
      try {
        const result = await axios({
          method: "POST",
          url: "http://beerrewardsadmin.demos.classicinformatics.com/api/login",
          headers: { Accept: "application/json" },
          data: this.formValues,
        });

        if (result.data.error === undefined) {
          localStorage.setItem("data", result.data.data);
          localStorage.setItem("accessToken", result.data.access_token);
          localStorage.setItem("accessType", "registeredUser");
          localStorage.setItem("tokenType", result.data.token_type);
          window.location.href = "/home";
        } else {
          alert(result.data.message);
        }
      } catch (error) {
        console.error(error);
      }
    },
    toggleButtonStatus() {
      this.passwordButtonStatus = !this.passwordButtonStatus;
    },
  },
};
</script>
<style scoped>
.form-container {
  margin: 20px 5px;
}
.continue {
  font-size: 10px;
  border-radius: 15px;
  padding: 4px 10px;
}
.footer-container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  /* padding: 0px 65px; */
}
.button-center {
  justify-content: center;
}
.forgot-password {
  font-size: x-small;
  font-weight: 700;
  margin-right: 30px;
  color: black;
  font-weight: 600;
}
.register {
  color: #c7cbd0;
  font-size: x-small;
}
</style>
