<template>
  <!-- <Navbar @isauthenticated="isAuth()"/> -->
  <router-view @isauthenticated="isAuth()" />

  <!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">{{signup_bool ? 'Sign Up' : 'Login'}}</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        <button type="button" ref="closeModal" hidden data-bs-dismiss="modal" aria-label="Close"></button>

      </div>
      <div class="modal-body">
        <form v-show="!signup_bool">
          <div class="">
            <label for="Email" class="form-label">Email</label>
            <input type="email" class="form-control form-control-sm" v-model="email" id="loginemail"/>
          </div>
          <div class="" v-show="!forgotPass_bool">
              <label for="password" class="form-label" >Password</label>
              <input type="password" class="form-control form-control-sm" v-model="password" id="loginpassword" />
          </div>
          <a href="#" class="mx-2" v-if="!message" @click="forgotPass_bool = true, modalfx = 'forgotPassword' ">Forgot Password</a>
          <div class="" v-show="loading">
            <div class="spinner-border text-primary spinner-border-sm" role="status">
              <span class="visually-hidden">Loading...</span>
            </div>
            <div class="spinner-grow text-warning spinner-grow-sm ms-2" role="status">
              <span class="visually-hidden">Loading...</span>
            </div>
          </div>
          <span class="text-warning  fw-lighter"><i class="fw-lighter bi bi-chat-square-dots me-2" v-if="message"></i>{{message}}</span>
          <p v-if="!signup_bool" class="text-success mx-2 float-end">
            Don't have an account?
            <a href="#" class="text-success" @click="signup_bool = true, modalfx = 'signup'">
              Sign Up
            </a>
          </p>           
        </form>
        <form  v-show="signup_bool">
          <div class="">
            <label for="name" class="form-label">Name</label>
            <input type="text" class="form-control form-control-sm" v-model="name" id="name" />
          </div>
          <div class="">
            <label for="Email" class="form-label">Email</label>
            <input type="email" class="form-control form-control-sm" v-model="email" id="email" />
          </div>
          <div class="">
              <label for="password" class="form-label">Password</label>
              <input type="password" class="form-control form-control-sm" v-model="password" id="password" />
          </div>
          <div class="">
              <label for="password_confirmation" class="form-label">Confirm Password</label>
              <input type="password" class="form-control form-control-sm" v-model="password_confirmation" id="password_confirmation" />
          </div>
          <p v-if="signup_bool" class=" text-success mx-2 float-end">
            I already have an account
            <a href="#" class="text-success" @click="signup_bool = false, modalfx = 'login', forgotPass_bool = false">
              Login
            </a>
          </p>
        </form>
      </div>
      <div class="modal-footer">
            <p class="text-success fs-6 float-start mx-auto" v-show="responseMsg"><i class="bi bi-chat-square-text-fill me-1"></i>Your process was Successful</p>
            <p class="text-danger fs-6 float-start mx-auto" v-show="errorMsg"><i class="bi bi-exclamation-triangle-fill me-1"></i>{{error}}</p>
            
        <button type="button"  ref="closeModal2" class="btn btn-sm btn-danger" data-bs-dismiss="modal">Close</button>
        <button type="button" v-if="modalfx == 'login'" @click="loginFx" class="btn btn-sm btn-primary">Login</button>
        <button type="button" v-else-if="modalfx == 'forgotPassword'" @click="forgotPassword" class="btn btn-sm btn-primary">Forgot Password</button>
        <button type="button" v-else @click="signupFx" class="btn btn-sm btn-primary">Sign Up</button>
      </div>
    </div>
  </div>
</div>

<!-- <button type="button" class="btn btn-sm btn-primary" @click="toast" id="liveToastBtn">Show live toast</button> -->



</template>


<script>
// @ is an alias to /src

import Navbar from "@/components/Navbar.vue";
import axios from 'axios';



export default {
  components: {
    Navbar,
  },
  data() {
    return {
      email:'',
      name:'',
      password:'',
      password_confirmation:'',
      modalfx: 'login',
      signup_bool:false,
      forgotPass_bool:false,
      loading:false,
      myModal:null,
      route: '',    
      message:null,
        errorMsg:false,
        error:null,
        responseMsg:false,
    }
  },
  methods: {
    isAuth(){
      setTimeout(() => {
        console.log(this.$refs.closeModal);
        this.$refs.closeModal.click()
      }, 500);
    },
    loginFx(){
      axios.post('http://127.0.0.1:8000/api/login', 
      {
        'email':this.email,
        'password':this.password
      }
        ).then(response =>  {
          console.log(response.data)
          window.localStorage.setItem('token', response.data.token); 
          this.responseMsg = true
          setTimeout(() => {
            this.$refs.closeModal.click();
            this.$router.push('/registration')
            this.responseMsg = false
          }, 1000);
        }).catch(error => {
          console.log(error);
          this.errorMsg = true
          this.error = error.response.data.message
        })
    },
    signupFx(){
      axios.post('http://127.0.0.1:8000/api/register', 
      {
        'name':this.name,
        'email':this.email,
        'password':this.password,
        'password_confirmation': this.password_confirmation
      }
        ).then(response => {
            console.log(response.data)
            window.localStorage.setItem('token', response.data.token); 
            this.$refs.closeModal.click();
            this.$router.push('/registration')
          }).catch(error => {
            console.log(error);
          })
    },
    forgotPassword(){
      this.loading = true
      axios.post('http://127.0.0.1:8000/api/forgotpassword', 
      {'email':this.email}
      ).then(response =>  {
        console.log(response.data)
        this.message = response.data.status
        this.loading = false
      }).catch(error => {
        console.log(error.response.data.message);
        this.message = error.response.data.message
        this.loading = false
      })
    }, 
  },
}


</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@400&display=swap');


p, h1, h2, h3,h4,h5, li, button, .btn, label,span,input, a{
    font-family: 'Raleway', sans-serif;
} 

body,html{
  height: 100%;
  overflow: auto;
  overflow-x: hidden;
}
  body.home {
    /* background: rgb(27, 26, 26); */
    background-image: linear-gradient(30deg, rgba(255,0,0,0)30%, rgb(34, 34, 34) );
    object-fit: fill;
  }
  body.about {
    /*background: rgb(150, 150, 150); */
    background-image: url('./assets/bgimage.png');
    min-height: fit-content;
  }
  body.index {
    /*background: rgb(150, 150, 150); */
    background-image: linear-gradient(30deg, rgba(255,0,0,0)30%, rgb(34, 34, 34) );
    min-height: fit-content;
  }
</style>
