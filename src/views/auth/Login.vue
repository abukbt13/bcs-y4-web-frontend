
<script setup>
import {onMounted, ref} from "vue";
import axios from "axios";
import {useRouter} from "vue-router";
const router = useRouter()
const email = ref('')
const password = ref('')

import {auth} from "@/compossables/auth";
const {base_url,authHeader} = auth()


const   authUser = async () => {
  try {
    const res= await axios.get('http://127.0.0.1:8000/api/auth/user', authHeader);
    if(res){
      localStorage.setItem('status', 'logged_in');
      await router.push('/dashboard');
    }
  } catch (error) {
  }
};
const login =async () => {
  if(email.value=='' && password.value==''){
    regerror.value = 'Please fill all fields'
  }
  const formData = new FormData();
  formData.append('email', email.value)
  formData.append('password', password.value)
  const res = await axios.post(base_url.value+'auth/login',formData)
  if(res.status=== 200) {
    if (res.data.status === 'success') {
      localStorage.setItem('token', res.data.token);
      await router.push('/dashboard');
    }
    else if(res.data.status === 'failed') {
      regerror.value = res.data.message;
    }

  }else{
    regerror.value ="Error in network"
  }

}

onMounted(()=>{
  authUser()
})
</script>

<template>
  <section class="vh-100">
    <div class="row px-3 d-flex justify-content-center align-items-center h-100">

      <div class="col-12 col-md-8 col-lg-6 col-xl-5">
        <div class="error text-center bg-danger text-white text-uppercase">{{regerror}}</div>
        <p>Dont have an account <a href="register">Create Account here</a></p>
        <form @submit.prevent="login">
          <div class="mb-3">
            <label for="exampleFormControlInput1" class="form-label">Email address</label>
            <input type="email" class="form-control" v-model="email" placeholder="name@example.com">
          </div>
          <div class="mb-3">
            <label for="exampleFormControlTextarea1" class="form-label">Password</label>
            <input type="password" class="form-control" v-model="password" placeholder="password">
          </div>

          <div class="">
            <button type="submit" class="w-100 btn btn-primary btn-block">Login</button>
          </div>
        </form>
      </div>

    </div>

  </section>
</template>

<style scoped>

</style>

