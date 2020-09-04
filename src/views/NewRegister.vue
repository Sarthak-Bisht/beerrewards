<template>
  <div class="vue-tempalte  main-ontainer">
    <div class="card text-center">
      <div class="card-header">
        <div class="main-heading">CREATE ACCOUNT</div>
        <div class="sub-heading">Complete your account below</div>
      </div>
      <div class="card-body">
        <form class="needs-validation" @submit="register">
          <div class="form-group " data-tip="This is the text of the tooltip2">
            <input
              type="email"
              class="form-control"
              id="inputEmail4"
              placeholder="Email"
              v-model="userData.email"
              required
            />
          </div>
          <div class="form-row">
            <div class="form-group col-md-5">
              <select
                class="form-control"
                v-model="userData.title"
                placeholder="TITLE"
                required
                id="title"
              >
                <option>Mr.</option>
                <option>Mrs.</option>
                <option>Ms.</option>
                <option>Miss.</option>
              </select>
            </div>
          </div>
          <div class="form-row">
            <div class="form-group col-md-6">
              <input
                type="text"
                class="form-control"
                id="firstname"
                placeholder="FIRST NAME"
                v-model="userData.firstName"
                required
              />
            </div>
            <div class="form-group col-md-6">
              <input
                type="text"
                class="form-control"
                id="lastName"
                placeholder="LAST NAME"
                v-model="userData.lastName"
              />
            </div>
          </div>
          <div class="form-group">
            <input
              type="text"
              class="form-control"
              id="mobile"
              placeholder="MOBILE"
              v-model="userData.mobile"
            />
          </div>
          <div class="form-row">
            <div class="form-group col-md-6">
              <input
                type="text"
                class="form-control"
                id="postCode"
                placeholder="POSTCODE"
                v-model="userData.postCode"
              />
            </div>
            <div class="form-group col-md-6">
              <input
                type="date"
                class="form-control"
                id="date"
                v-model="userData.dob"
                required
              />
            </div>
          </div>

          <div class="input-group mb-3">
            <input
              :type="passwordButtonStatus ? 'text' : 'password'"
              class="form-control"
              id="inputPasswordRegister"
              placeholder="PASSWORD"
              required
              v-model="userData.password"
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
          <div class="set-link">
            <div class="form-group">
              <div class="form-check">
                <input
                  class="form-check-input"
                  type="checkbox"
                  v-model="userData.agree"
                  id="gridCheck"
                  required
                />
                <label class="form-check-label" for="gridCheck">
                  I agree to <a href="">terms of use </a>
                </label>
              </div>
            </div>
          </div>
          <div class="set-button">
            <button type="submit" class="btn btn-dark register">
              REGISTER
            </button>
          </div>
        </form>
      </div>
      <div class="card-footer privacy-container">
        <div style="font-weight:800;">Get the facts</div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "NewRegister",
  data() {
    return {
      userData: {
        email: null,
        title: null,
        firstName: null,
        lastName: null,
        mobile: null,
        postCode: null,
        dob: null,
        password: null,
        agree: null,
      },
      error: {
        date: "required",
      },
      passwordButtonStatus: false,
    };
  },
  components: {},
  methods: {
    async register(e) {
      e.preventDefault();
      if (!this.underAgeValidate(this.userData.dob)) {
        alert("age must me 18 or over");
      } else {
        try {
          const {
            email,
            title,
            firstName,
            lastName,
            mobile,
            postCode,
            dob,
            password,
            agree,
          } = this.userData;

          const result = await axios({
            method: "POST",
            url:
              "http://beerrewardsadmin.demos.classicinformatics.com/api/register",
            headers: { Accept: "application/json" },
            data: {
              title: title,
              first_name: firstName,
              last_name: lastName,
              email: email,
              password: password,
              dob: dob,
              phone: mobile,
              postalcode: postCode,
              terms: agree ? "1" : "0",
            },
          });

          if (result.data.error === undefined) {
            localStorage.setItem("data", result.data.data);
            localStorage.setItem("accessToken", result.data.access_token);
            localStorage.setItem("tokenType", result.data.token_type);
            localStorage.setItem("accessType", "registeredUser");
            window.location.href = "/home";
          } else {
            alert(result.data.message);
          }
        } catch (error) {
          console.error(error);
        }
      }
    },
    underAgeValidate(birthday) {
      const optimizedBirthday = birthday.replace(/-/g, "/");
      const myBirthday = new Date(optimizedBirthday);
      const currentDate = new Date().toJSON().slice(0, 10) + " 01:00:00";
      const myAge = ~~((Date.now(currentDate) - myBirthday) / 31557600000);
      if (myAge < 18) {
        return false;
      } else {
        return true;
      }
    },
    toggleButtonStatus() {
      this.passwordButtonStatus = !this.passwordButtonStatus;
    },
  },
};
</script>
<style scoped>
form {
  width: 55%;
}
.card {
  height: 70%;
  width: 60%;
}
.card-header {
  background-color: white;
  border: none;
}
.main-heading {
  font-weight: 600;
  font-size: large;
}
.sub-heading {
  font-weight: light;
  font-size: small;
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

.register {
  font-size: 10px;
  border-radius: 15px;
  margin-bottom: 15px;
  padding: 4px 10px;
}

.set-link {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
}
.set-button {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-end;
}
</style>
