<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useAuthStore } from '@/stores/auth'

const authStore = useAuthStore()
const router = useRouter()

const email = ref('')
const password = ref('')
const errorMessage = ref('')

const handleLogin = async () => {
  try {
    const success = await authStore.login(email.value, password.value)

    if (success) {
      router.push('/')
    } else {
      errorMessage.value = 'メールアドレスまたはパスワードが違います。'
    }
  } catch (error) {
    console.error('Login error:', error)
    errorMessage.value = 'ログイン中にエラーが発生しました。'
  }
}
</script>

<template>
  <div class="min-h-screen flex items-center justify-center bg-google-gray-100 font-google">
    <div class="w-full max-w-md p-8 space-y-6 bg-white rounded-google shadow-google">
      <div class="text-center">
        <h2 class="text-2xl font-medium text-google-gray-800">ログイン</h2>
        <p class="mt-2 text-sm text-google-gray-600">アカウントにサインインしてください</p>
      </div>
      <form class="space-y-6" @submit.prevent="handleLogin">
        <div>
          <label for="email" class="block text-sm font-medium text-google-gray-700 mb-2">
            メールアドレス
          </label>
          <input
            id="email"
            v-model="email"
            type="email"
            required
            class="w-full px-4 py-3 border border-google-gray-300 rounded-google focus:outline-none focus:ring-2 focus:ring-google-blue-500 focus:border-transparent transition-colors"
            placeholder="email@example.com"
          />
        </div>
        <div>
          <label for="password" class="block text-sm font-medium text-google-gray-700 mb-2">
            パスワード
          </label>
          <input
            id="password"
            v-model="password"
            type="password"
            required
            class="w-full px-4 py-3 border border-google-gray-300 rounded-google focus:outline-none focus:ring-2 focus:ring-google-blue-500 focus:border-transparent transition-colors"
            placeholder="パスワードを入力"
          />
        </div>
        <div v-if="errorMessage" class="p-3 bg-red-50 border border-red-200 rounded-google">
          <p class="text-sm text-red-600">{{ errorMessage }}</p>
        </div>
        <div>
          <button
            type="submit"
            class="w-full px-4 py-3 font-medium text-white bg-google-blue-500 rounded-google hover:bg-google-blue-600 focus:outline-none focus:ring-2 focus:ring-google-blue-500 focus:ring-offset-2 shadow-sm transition-colors"
          >
            ログイン
          </button>
        </div>
      </form>
      <div class="text-center">
        <p class="text-sm text-google-gray-600">
          アカウントをお持ちでない方は
          <router-link to="/signup" class="text-google-blue-500 hover:text-google-blue-600 font-medium">
            新規登録
          </router-link>
        </p>
      </div>
    </div>
  </div>
</template>
