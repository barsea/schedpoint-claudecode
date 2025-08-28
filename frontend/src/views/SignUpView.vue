<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useAuthStore } from '@/stores/auth'

const authStore = useAuthStore()
const router = useRouter()

const name = ref('')
const email = ref('')
const password = ref('')
const errorMessage = ref('')

const handleSignup = async () => {
  errorMessage.value = ''
  try {
    const result = await authStore.signup(name.value, email.value, password.value)

    if (result === true) {
      router.push('/login')
    } else {
      errorMessage.value = result
    }
  } catch (error) {
    console.error('Signup error:', error)
    errorMessage.value = '新規登録中にエラーが発生しました。'
  }
}
</script>

<template>
  <div class="min-h-screen flex items-center justify-center bg-google-gray-100 font-google">
    <div class="w-full max-w-md p-8 space-y-6 bg-white rounded-google shadow-google">
      <div class="text-center">
        <h2 class="text-2xl font-medium text-google-gray-800">新規登録</h2>
        <p class="mt-2 text-sm text-google-gray-600">アカウントを作成してください</p>
      </div>
      <form class="space-y-6" @submit.prevent="handleSignup">
        <div>
          <label for="name" class="block text-sm font-medium text-google-gray-700 mb-2">
            名前
          </label>
          <input
            id="name"
            v-model="name"
            type="text"
            required
            class="w-full px-4 py-3 border border-google-gray-300 rounded-google focus:outline-none focus:ring-2 focus:ring-google-blue-500 focus:border-transparent transition-colors"
            placeholder="山田 太郎"
          />
        </div>
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
            placeholder="6文字以上で入力してください"
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
            登録する
          </button>
        </div>
      </form>
      <div class="text-center">
        <p class="text-sm text-google-gray-600">
          すでにアカウントをお持ちの方は
          <router-link to="/login" class="text-google-blue-500 hover:text-google-blue-600 font-medium">
            ログイン
          </router-link>
        </p>
      </div>
    </div>
  </div>
</template>
