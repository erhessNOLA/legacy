<template>
  <b-jumbotron header="Sign Up Page">
    <b-form>
      <b-form-group
        id="fieldset1"
        label="Email address"
        :feedback="emailFeedback"
        :state="email"
      >
        <b-form-input
          v-model.trim="signUpForm.email"
          type="email"
          placeholder="E-mail Address"
          required
        />
      </b-form-group>
      <b-form-group
        label="Name"
        :feedback="nameFeedback"
        :state="name"
      >
        <b-form-input
          v-model.trim="signUpForm.name"
          type="text"
          placeholder="Name"
          required
        />
      </b-form-group>

      <b-form-group
        label="Password"
        :feedback="passFeedback"
        :state="pass"
      >
        <b-form-input
          v-model.trim="signUpForm.password"
          type="password"
          placeholder="Password"
          required
        />
      </b-form-group>

      <b-form-group
        label="Current City"
        :feedback="cityFeedback"
        :state="city"
      >
        <b-form-input
          v-model.trim="signUpForm.city"
          type="text"
          placeholder="Current City"
          required
        />
      </b-form-group>

      <b-form-group
        label="Date of Birth"
        :feedback="dobFeedback"
        :state="dob"
      >
        <b-form-input
          v-model="signUpForm.dob"
          type="date"
          placeholder="DOB"
          required
        />
      </b-form-group>
      <b-form-select v-model="signUpForm.gender">
        <option>male</option>
        <option>female</option>
      </b-form-select>
      <b-button @click.prevent="signUp">Sign Up</b-button>
    </b-form>
  </b-jumbotron>
</template>

<script>
// Imports

export default {
  data() {
    return {
      signUpForm: {
        name: '',
        password: '',
        city: '',
        email: '',
        dob: '',
        Image: '',
      },
    };
  },
  computed: {
    emailFeedback() {
      return this.signUpForm.email.length > 0 ? '' : 'Please enter your email';
    },
    nameFeedback() {
      return this.signUpForm.name.length > 0 ? '' : 'Please enter your Name';
    },
    passFeedback() {
      return this.signUpForm.name.length > 0 ? '' : 'Please enter your Password';
    },
    dobFeedback() {
      return this.signUpForm.dob.length > 0 ? '' : 'Please enter your DOB';
    },
    cityFeedback() {
      return this.signUpForm.city.length > 0 ? '' : 'Please enter your Current City';
    },
    email() {
      const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(this.signUpForm.email) ? 'valid' : 'invalid';
    },
    name() {
      return this.signUpForm.name.length > 0 ? 'valid' : 'invalid';
    },
    pass() {
      return this.signUpForm.password.length > 6 ? 'valid' : 'invalid';
    },
    dob() {
      const re = /^((0?[13578]|10|12)(-|\/)(([1-9])|(0[1-9])|([12])([0-9]?)|(3[01]?))(-|\/)((19)([2-9])(\d{1})|(20)([01])(\d{1})|([8901])(\d{1}))|(0?[2469]|11)(-|\/)(([1-9])|(0[1-9])|([12])([0-9]?)|(3[0]?))(-|\/)((19)([2-9])(\d{1})|(20)([01])(\d{1})|([8901])(\d{1})))$/;
      return re.test(this.signUpForm.dob) ? 'valid' : 'invalid';
    },
    city() {
      return this.signUpForm.password.length > 6 ? 'valid' : 'invalid';
    },
  },
  methods: {
    signUp() {
      this.$http.get('https://tinyfac.es/api/users')
        .then((response) => {
          for (let i = 0; i < response.body.length; i++) {
            if (this.signUpForm.gender === response.body[i].gender) {
              this.signUpForm.Image = response.body[i].avatars[1].url;
              console.log(this.signUpForm.Image);
              return;
            }
          }
        })
        .then(() => {
          this.$http.post('/signup', {
            name: this.signUpForm.name,
            password: this.signUpForm.password,
            city: this.signUpForm.city,
            email: this.signUpForm.email,
            dob: this.signUpForm.dob,
            Image: this.signUpForm.Image,
          }).then((data, status, request) => {
            this.signUpForm.name = '';
            this.signUpForm.password = '';
            this.signUpForm.city = '';
            this.signUpForm.email = '';
          });
        });
    },
  },
};
</script>


<style scoped>
.body {
  background: url(../assets/Walkthrough_03@3x.png);
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
