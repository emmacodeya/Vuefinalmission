<template>
  <div id="loginPage" class="bg-yellow">
    <div class="conatiner loginPage vhContainer">
      <div class="side">
        <a href="#"
          ><img
            class="logoImg"
            src="https://raw.githubusercontent.com/hexschool/2022-web-layout-training/main/todolist/logo.png"
            alt=""
        /></a>
        <img
          class="d-m-n"
          src="https://raw.githubusercontent.com/hexschool/2022-web-layout-training/main/todolist/img.png"
          alt="workImg"
        />
      </div>
      <div>
        <form class="formControls" action="index.html">
          <h2 class="formControls_txt">最實用的線上代辦事項服務</h2>
          <label class="formControls_label" for="email">Email</label>
          <input
            class="formControls_input"
            type="text"
            id="email"
            name="email"
            placeholder="請輸入 email"
            v-model="email"
            required
          />
          <span v-if="submitted && !email.trim()">此欄位不可留空</span>
          <label class="formControls_label" for="pwd">密碼</label>
          <input
            class="formControls_input"
            type="password"
            name="pwd"
            id="pwd"
            placeholder="請輸入密碼"
            v-model="password"
            required
          />
          <input class="formControls_btnSubmit" type="button" @click="handleLogin" value="登入" />
          <router-link class="formControls_btnLink" to="register">註冊帳號</router-link>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { login } from '../utils/api.js'
import Swal from 'sweetalert2'

const router = useRouter()
const email = ref('')
const password = ref('')
const submitted = ref(false)

const handleLogin = async () => {
  submitted.value = true

  if (!email.value.trim() || !password.value.trim()) {
    return
  }

  try {
    const response = await login(email.value.trim(), password.value.trim())
    const { token, exp } = response.data
    document.cookie = `vue3-todolist-token=${token}; expires=${exp}`
    await Swal.fire({
      icon: 'success',
      title: '登入成功',
      showConfirmButton: false,
      timer: 1500,
    })
    router.push('/todolist')
  } catch (error) {
    Swal.fire({
      icon: 'error',
      title: '登入失敗',
      text: error?.response?.data?.message ?? '發生未知錯誤',
    })
  }
}
</script>
