<template>
  <div class="root">
    <div class="title">ระบบจัดการหลังร้าน</div>

    <div class="container">
      <form @submit.prevent="login" class="form-container">
        <input
          type= "email"
          placeholder="อีเมล"
          required
          v-model="email"
        />
        <input
          type="password"
          placeholder="รหัสผ่าน"
          required
          v-model="password"
        />

        <button type="submit" class="btn btn-success">เข้าสู่ระบบ</button>
      </form>
    </div>
  </div>
</template>

<script>
import { Jwt, StoreAuth } from '../libs/sessionStorage'

export default {
      head() {
      return {
        title: "เข้าสู่ระบบ",
      }
    },
  data() {
    return {
      email: '',
      password: '',
    }
  },
  methods: {
    async login() {
      if (!this.email && !this.password) return

      try {
         const url = "https://test-eshop-api.herokuapp.com/api/v1"
        const { data } = await this.$axios.post(
          url+'/store/admin/login',
          {
            email: this.email,
            password: this.password,
          }
        )

        // console.log(data)

        if (data) {
          this.$router.push('/dashboard')
          Jwt.setJwtToken(data.token)
          StoreAuth.setStoreAuth(data.data)
        }
      } catch (err) {
        console.error(err)
      }
    },
  },
}
</script>

<style scoped>
.root {
  min-height: 100vh;
  padding: 20px;

  gap: 20px;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

}


.form-container {
  width: auto;
  height: auto;
  padding: 20px;

  gap: 12px;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  @apply bg-white rounded shadow-lg;
}

.form-container input {
  @apply border-2 border-dodger-blue-500 px-3 py-2 rounded;
}

.form-container .btn-login {
  width: 100%;
  height: 100%;

  @apply transition-all bg-dodger-blue-500 px-3 py-1 rounded text-white font-medium;
  @apply hover:bg-dodger-blue-700;
}

.form-container .btn-signup {
  width: 100%;
  height: 100%;

  @apply transition-all bg-transparent px-2 py-1 rounded text-black text-sm font-normal;
  @apply text-center hover:underline;
}
</style>