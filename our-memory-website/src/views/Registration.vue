<template>
  <div class="registration" style="text-align: center;">
    <h1>Registration</h1>
    <div style="width: 50%; left: 25%; display: inline-block">
      <b-form @submit="onSubmit" @reset="onReset" v-if="show">
        <b-form-group
          id="emailLabel"
          label="Email address:"
          label-for="emailInput"
          description="We'll never share your email with anyone else."
        >
          <b-form-input
            id="emailInput"
            v-model="model.email"
            type="email"
            required
            placeholder="Enter email"
          ></b-form-input>
        </b-form-group>

        <b-form-group id="name.Label" label="Your Name:" label-for="nameInput">
          <b-form-input
            id="nameInput"
            v-model="model.name"
            required
            placeholder="Enter name"
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="passwordLabel"
          label="password:"
          label-for="passwordInput"
        >
          <b-form-input
            id="passwordInput"
            v-model="model.password"
            required
            placeholder="Enter password"
            type="password"
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="confirmpasswordLabel"
          label="Confirm password:"
          label-for="input-3"
          ><b-form-input
            id="confirmpasswordInput"
            v-model="model.confirmPassword"
            required
            placeholder="Confirm password"
            type="password"
          ></b-form-input>
        </b-form-group>

        <b-button type="submit" variant="primary">Submit</b-button>
        <b-button type="reset" variant="danger">Reset</b-button>
      </b-form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ApiService from "..\\components\\ApiService.vue";

export default {
  data() {
    return {
      model: {
        email: "",
        name: "",
        confirmPassword: "",
        password: ""
      },
      show: true
    };
  },
  methods: {
    async onSubmit(evt) {
      evt.preventDefault();

      var result = await ApiService.post("\\Account\\Register", {
        "model.email": this.model.email,
        "model.name": this.model.name,
        "model.confirmPassword": this.model.confirmPassword,
        "model.password": this.model.password
      });

      if (result.status == 200) {
        console.log("success: " + result.data);
      } else {
        console.log("failure:" + result.status + " | " + result.data);
      }

      alert(JSON.stringify(this.model));
    },
    onReset(evt) {
      evt.preventDefault();
      // Reset our form values
      this.model.email = "";
      this.model.name = "";
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    }
  }
};
</script>
