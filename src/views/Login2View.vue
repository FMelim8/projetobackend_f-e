<template>
    <div class="login-wrap">
      <router-link :to="`/`">
        <button style="position: absolute; top: 70px; left: 70px; color: red; font-size: 30px; font-weight: 900;">
          X (backend)
        </button>
      </router-link>
      <div class="login-html">
        <input id="tab-1" type="radio" name="tab" class="sign-in" checked>
        <label for="tab-1" class="tab">Sign In</label>
        <input id="tab-2" type="radio" name="tab" class="sign-up">
        <label for="tab-2" class="tab">Sign Up</label>
        <div class="login-form">
          <div class="sign-in-htm">
            <div class="group">
              <label for="user" class="label">Email</label>
              <input id="user" type="text" class="input" v-model="email">
            </div>
            <div class="group">
              <label for="pass" class="label">Password</label>
              <input id="pass" type="password" class="input" data-type="password" v-model="password">
            </div>
            <div class="group">
              <input type="submit" class="button" value="Sign In" @click="loginUser">
            </div>
            <div class="hr"></div>
            <div class="foot-lnk">
              <h1 style="color: red;">{{ errMsg }}</h1>
              <a>Forgot Password? then sorry</a>
            </div>
          </div>
          <div class="sign-up-htm">
            <div class="group">
              <label for="user" class="label">Username</label>
              <input id="user" type="text" class="input" v-model="userName">
            </div>
            <div class="group">
              <label for="pass" class="label">Password</label>
              <input id="pass" type="password" class="input" data-type="password" v-model="password">
            </div>
            <div class="group">
              <label for="pass" class="label">Repeat Password</label>
              <input id="pass" type="password" class="input" data-type="password" v-model="confirmPassword">
            </div>
            <div class="group">
              <label for="pass" class="label">Email Address</label>
              <input id="pass" type="text" class="input" v-model="email">
            </div>
            <div class="group">
              <label for="pass" class="label">Url of picture profile (not necessary)</label>
              <input id="pass" type="text" class="input" v-model="pictureProfileSignIn">
            </div>
            <div class="group">
              <input type="submit" class="button" value="Sign Up" @click="register">
            </div>
            <div class="hr"></div>
            <div class="foot-lnk">
              <h1 style="color: red;">{{ errMsg }}</h1>
              <label for="tab-1">Already Member?</label>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        userName: "",
        email: "",
        password: "",
        confirmPassword: "",
        pictureProfileSignIn: "",
        errMsg: ""
      };
    },
    methods: {
      async loginUser() {
        localStorage.removeItem("reloaded");
        try {
          const response = await axios.post("http://localhost:3000/login", {
            email: this.email,
            password: this.password,
            username: this.userName
          });
          console.log(response.data);
          localStorage.setItem("token", response.data.token);
          localStorage.setItem('isLoggedIn', true);
          this.$router.push({
            name: "home",
            params: {
              user: response.data.user
            }
          });
        } catch (error) {
          if (error.response && error.response.data) {
            this.errMsg = error.response.data.message || "Login failed. Please check your credentials.";
          } else {
            this.errMsg = "Login failed due to an unknown error.";
          }
          console.error(error);
        }
      },
      async register() {
        localStorage.removeItem("reloaded");
        if (this.password !== this.confirmPassword) {
          this.errMsg = "Passwords do not match.";
          return;
        }
        try {
          const response = await axios.post("http://localhost:3000/signup", {
            email: this.email,
            password: this.password,
            username: this.userName
          });
          console.log(response.data);
          localStorage.setItem("token", response.data.token);
          localStorage.setItem('isLoggedIn', true);
        } catch (error) {
          if (error.response && error.response.data) {
            this.errMsg = error.response.data.message || "Signup failed.";
          } else {
            this.errMsg = "Signup failed due to an unknown error.";
          }
          console.error(error);
        }
      }
    }
  };
  </script>
  
  
    <style scoped>
   body{
      margin:0;
      color:#6a6f8c;
      background:#c8c8c8;
      font:600 16px/18px 'Open Sans',sans-serif;
  }
  *,:after,:before{box-sizing:border-box}
  .clearfix:after,.clearfix:before{content:'';display:table}
  .clearfix:after{clear:both;display:block}
  a{color:inherit;text-decoration:none}
  
  .login-wrap{
      width:100%;
      height:100vh; 
      margin:auto;
      position:relative;
      background:linear-gradient(rgba(0, 0, 0, 0.919), rgba(0, 0, 0, 0.698)), url(https://images.unsplash.com/photo-1637825891028-564f672aa42c?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D) no-repeat center;
      box-shadow:0 12px 15px 0 rgba(0,0,0,.24),0 17px 50px 0 rgb(92, 0, 93);
      display: flex;
      align-items: center;
      justify-content: center;
  }
  
  .login-html{
      width:100%;
      max-width: 600px;
      height:100%; 
      position:absolute;
      padding:90px 70px 50px 70px;
  }
  
  .login-html .sign-in-htm,
  .login-html .sign-up-htm{
      top:0;
      left:0;
      right:0;
      bottom:0;
      position:absolute;
      transform:rotateY(180deg);
      backface-visibility:hidden;
      transition:all .4s linear;
  }
  .login-html .sign-in,
  .login-html .sign-up,
  .login-form .group .check{
      display:none;
  }
  .login-html .tab,
  .login-form .group .label,
  .login-form .group .button{
      text-transform:uppercase;
  }
  .login-html .tab{
      font-size:22px;
      margin-right:15px;
      padding-bottom:5px;
      margin:0 15px 10px 0;
      display:inline-block;
      border-bottom:2px solid transparent;
  }
  .login-html .sign-in:checked + .tab,
  .login-html .sign-up:checked + .tab{
      color:#fff;
      border-color:#1161ee;
  }
  .login-form{
      min-height:345px;
      position:relative;
      perspective:1000px;
      transform-style:preserve-3d;
  }
  .login-form .group{
      margin-bottom:15px;
  }
  .login-form .group .label,
  .login-form .group .input,
  .login-form .group .button{
      width:100%;
      color:#fff;
      display:block;
  }
  .login-form .group .input,
  .login-form .group .button{
      border:none;
      padding:15px 20px;
      border-radius:25px;
      background:rgba(255,255,255,.1);
  }
  .login-form .group input[data-type="password"]{
      -webkit-text-security:circle;
  }
  .login-form .group .label{
      color:#aaa;
      font-size:12px;
  }
  .login-form .group .button{
      background:#1161ee;
  }
  .login-form .group label .icon{
      width:15px;
      height:15px;
      border-radius:2px;
      position:relative;
      display:inline-block;
      background:rgba(255,255,255,.1);
  }
  .login-form .group label .icon:before,
  .login-form .group label .icon:after{
      content:'';
      width:10px;
      height:2px;
      background:#fff;
      position:absolute;
      transition:all .2s ease-in-out 0s;
  }
  .login-form .group label .icon:before{
      left:3px;
      width:5px;
      bottom:6px;
      transform:scale(0) rotate(0);
  }
  .login-form .group label .icon:after{
      top:6px;
      right:0;
      transform:scale(0) rotate(0);
  }
  .login-form .group .check:checked + label{
      color:#fff;
  }
  .login-form .group .check:checked + label .icon{
      background:#1161ee;
  }
  .login-form .group .check:checked + label .icon:before{
      transform:scale(1) rotate(45deg);
  }
  .login-form .group .check:checked + label .icon:after{
      transform:scale(1) rotate(-45deg);
  }
  .login-html .sign-in:checked + .tab + .sign-up + .tab + .login-form .sign-in-htm{
      transform:rotate(0);
  }
  .login-html .sign-up:checked + .tab + .login-form .sign-up-htm{
      transform:rotate(0);
  }
  
  .hr{
      height:2px;
      margin:60px 0 50px 0;
      background:rgba(255,255,255,.2);
  }
  .foot-lnk{
      text-align:center;
  }
    </style>