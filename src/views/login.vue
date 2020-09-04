<template>
  <div class="vue-tempalte  main-ontainer">
    <div class="card text-center">
      <div class="card-header">BEER REWARDS</div>
      <div class="card-body">
        <Guest />
        <div style="margin:5px;font-weight:600;">OR</div>
        <Register />
      </div>
      <div class="card-footer privacy-container">
        <div>
          By submitting this form,you agree to the
          <a href="">Privacy Policy</a>of Beer Reward's website
        </div>
        <div style="font-weight:800;">Get the facts</div>
      </div>
    </div>
  </div>
</template>

<script>
import Guest from "@/components/Guest.vue";
import Register from "@/components/Register.vue";

export default {
  name: "Login",
  components: {
    Guest,
    Register,
  },
  beforeRouteEnter(to, from, next) {
    if (localStorage.getItem("accessToken")) {
      localStorage.setItem("accessType", "registeredUser");
      next("/home");
    } else {
      next();
    }
  },
};
</script>
<style scoped>
.card {
  height: 70%;
  width: 60%;
}
.card-header {
  background-color: white;
  border: none;
  font-weight: 600;
  font-size: large;
}
.card-body {
  display: flex;
  justify-content: center;
  align-items: center;
  flex: none;
  height: 75%;
  padding: 0px 10px;
}
.card-footer {
  background-color: white;
  border: none;
}

.main-ontainer {
  display: flex;
  justify-content: center;
  align-items: center;
}
.privacy-container {
  font-size: 10px;
}
</style>
