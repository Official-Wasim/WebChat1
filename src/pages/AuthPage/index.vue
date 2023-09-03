<template>
  <div class="container" :class="{ 'sign-up-mode': isSignUpMode }">
    <div class="forms-container">
      <div class="signin-signup">
        <form action="#" @submit.prevent="login" class="sign-in-form">
          <h2 class="title">Sign in</h2>
          <div class="input-field">
            <i class="fas fa-user"></i>
            <input type="text" placeholder="Username" v-model="username" />
          </div>
          <div class="input-field">
            <i class="fas fa-lock"></i>
            <input type="password" placeholder="Password" v-model="password" />
          </div>
          <input type="submit" value="Login" class="btn solid" />
          <p class="social-text">Or Sign in with social platforms</p>
          <div class="social-media">
            <a href="#" class="social-icon">
              <i class="fab fa-facebook-f"></i>
            </a>
            <a href="#" class="social-icon">
              <i class="fab fa-twitter"></i>
            </a>
            <a href="#" class="social-icon">
              <i class="fab fa-google"></i>
            </a>
            <a href="#" class="social-icon">
              <i class="fab fa-linkedin-in"></i>
            </a>
          </div>
        </form>
        <form action="#" @submit.prevent="signup" class="sign-up-form" @click="switchToSignUpMode" >
          <h2 class="title">Sign up</h2>

          <div class="input-field">
            <i class="fas fa-user"></i>
            <input type="text" placeholder="Username" v-model="username" />
          </div>
          <div class="input-field">
            <i class="fas fa-envelope"></i>
            <input type="email" placeholder="Email" v-model="email" />
          </div>
          <div class="input-field">
            <i class="fas fa-lock"></i>
            <input type="password" placeholder="Password" v-model="password" />
          </div>
          <div class="input-field">
            <i class="fas fa-lock"></i>
            <input placeholder="First name" type="text" v-model="first_name" />
          </div>
          <div class="input-field">
            <i class="fas fa-lock"></i>
            <input placeholder="Last name" type="text" v-model="last_name" />
          </div>
          <input type="submit" class="btn" value="Sign up" />
          <p class="social-text">Or Sign up with social platforms</p>
          <div class="social-media">
            <a href="#" class="social-icon">
              <i class="fab fa-facebook-f"></i>
            </a>
            <a href="#" class="social-icon">
              <i class="fab fa-twitter"></i>
            </a>
            <a href="#" class="social-icon">
              <i class="fab fa-google"></i>
            </a>
            <a href="#" class="social-icon">
              <i class="fab fa-linkedin-in"></i>
            </a>
          </div>
        </form>
      </div>
    </div>

    <div class="panels-container">
      <div class="panel left-panel">
        <div class="content">
          <h3>New here ?</h3>
          <p>
            Ready to chat? Create your account now!
          </p>
          <button class="btn transparent" id="sign-up-btn" @click="toggleSignUpMode(true)">
            Sign up
          </button>
        </div>
       
      </div>
      <div class="panel right-panel">
        <div class="content">
          <h3>One of us ?</h3>
          <button class="btn transparent" id="sign-in-btn"  @click="toggleSignUpMode(false)" >
            Sign in
          </button>
        </div>
     
      </div>
    </div>
    <div v-if="wrongPassword" class="wrong-password-message">
      Incorrect username or password. Please try again.
    </div>    
  </div>
</template>
<script>
   import '@fortawesome/fontawesome-free/css/all.css'
   import { loginRest, signupRest } from "./api";
  
  export default {
    data() {
      return {
        username: "",
        password: "",
        email: "",
        first_name: "",
        last_name: "",
        wrongPassword: false,
      };
    },
    methods: {
      login() {
    loginRest(this.username, this.password)
      .then((response) => {
        if (response.data && response.data.success) {
          // Replace the condition above with the appropriate logic
          this.$emit("onAuth", { ...response.data, secret: this.password });
        } else {
          this.wrongPassword = true; // Show wrong password message
        }
      })
      .catch((error) => console.log("Login error", error));
      },
      signup() {
         signupRest(
           this.username,
           this.password,
           this.email,
           this.first_name,
           this.last_name
         )
           .then((response) =>
             this.$emit("onAuth", { ...response.data, secret: this.password })
           )
           .catch((error) => console.log("Sign up error", error));
      },
      toggleSignUpMode() {
        this.isSignUpMode = !this.isSignUpMode;
      },

    },
  };
</script>