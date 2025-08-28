<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { useRouter } from 'vue-router'
import { useAuthStore } from '@/stores/auth'
import { usePlanStore } from '@/stores/plan'

const authStore = useAuthStore()
const planStore = usePlanStore()
const router = useRouter()

// --- ユーザーメニューのドロップダウン関連 ---
const isUserDropdownOpen = ref(false)
const userDropdown = ref(null)

const toggleUserDropdown = () => {
  isUserDropdownOpen.value = !isUserDropdownOpen.value
  // もう片方のドロップダウンが開いていたら閉じる
  isCreateDropdownOpen.value = false
}

// --- 「+作成」ボタンのドロップダウン関連 ---
const isCreateDropdownOpen = ref(false)
const createDropdown = ref(null) // テンプレート内の要素を参照するためのref
const toggleCreateDropdown = () => {
  isCreateDropdownOpen.value = !isCreateDropdownOpen.value
  // もう片方のドロップダウンが開いていたら閉じる
  isUserDropdownOpen.value = false
}

// メニューの外側をクリックしたかを判定する関数
const closeDropdowns = (event) => {
  if (userDropdown.value && !userDropdown.value.contains(event.target)) {
    isUserDropdownOpen.value = false
  }
  if (createDropdown.value && !createDropdown.value.contains(event.target)) {
    isCreateDropdownOpen.value = false
  }
}

// コンポーネントがマウントされた時にイベントリスナーを追加
onMounted(() => {
  window.addEventListener('click', closeDropdowns)
})

// コンポーネントがアンマウントされる時にイベントリスナーを削除
onUnmounted(() => {
  window.removeEventListener('click', closeDropdowns)
})

const handleLogout = () => {
  authStore.logout()
  router.push('/login')
}
</script>

<template>
  <div class="flex items-center justify-between h-full px-6 py-3 font-google">
    <div class="flex items-center">
      <div class="text-2xl font-medium mr-8 text-google-gray-800">{{ planStore.formattedCurrentDate }}</div>
      <button
        @click="planStore.resetToToday()"
        class="bg-google-blue-500 hover:bg-google-blue-600 text-white font-medium text-sm py-2 px-4 rounded-google mr-4 shadow-sm transition-colors"
      >
        今日
      </button>
      <div class="flex items-center bg-google-gray-100 rounded-google overflow-hidden mr-4">
        <button
          @click="planStore.changeDate(-1)"
          class="hover:bg-google-gray-200 text-google-gray-700 font-medium py-2 px-3 transition-colors"
        >
          ←
        </button>
        <div class="w-px h-8 bg-google-gray-300"></div>
        <button
          @click="planStore.changeDate(1)"
          class="hover:bg-google-gray-200 text-google-gray-700 font-medium py-2 px-3 transition-colors"
        >
          →
        </button>
      </div>
    </div>

    <div class="flex items-center space-x-4">
      <!-- 未ログイン時の表示 -->
      <div v-if="!authStore.isLoggedIn" class="space-x-3">
        <router-link
          to="/login"
          class="px-4 py-2 text-sm font-medium text-google-gray-700 rounded-google hover:bg-google-gray-100 transition-colors"
        >
          ログイン
        </router-link>
        <router-link
          to="/signup"
          class="bg-google-blue-500 hover:bg-google-blue-600 px-4 py-2 text-sm font-medium text-white rounded-google shadow-sm transition-colors"
        >
          新規登録
        </router-link>
      </div>
      <!-- ログイン時の表示 -->
      <div v-else class="flex items-center space-x-4">
        <!-- 「+ 作成」ボタンをドロップダウンに変更 -->
        <div class="relative" ref="createDropdown">
          <button
            @click.prevent="toggleCreateDropdown"
            class="flex items-center bg-google-blue-500 hover:bg-google-blue-600 px-4 py-2 text-sm font-medium text-white rounded-google shadow-sm transition-colors"
          >
            <svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"></path>
            </svg>
            <span>作成</span>
            <svg class="w-4 h-4 ml-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
            </svg>
          </button>
          <div
            v-if="isCreateDropdownOpen"
            class="absolute right-0 mt-2 w-36 bg-white rounded-google shadow-google py-2 z-20"
          >
            <RouterLink
              to="/events/new/plan"
              class="block px-4 py-2 text-sm text-google-gray-700 hover:bg-google-gray-100 transition-colors"
              @click="isCreateDropdownOpen = false"
            >
              予定
            </RouterLink>
            <RouterLink
              to="/events/new/actual"
              class="block px-4 py-2 text-sm text-google-gray-700 hover:bg-google-gray-100 transition-colors"
              @click="isCreateDropdownOpen = false"
            >
              実績
            </RouterLink>
          </div>
        </div>

        <!-- ユーザー名とプルダウンメニュー -->
        <div class="relative" ref="userDropdown">
          <button
            @click.prevent="toggleUserDropdown"
            class="flex items-center space-x-2 p-2 rounded-full hover:bg-google-gray-100 focus:outline-none transition-colors"
          >
            <div class="w-8 h-8 bg-google-blue-500 rounded-full flex items-center justify-center">
              <span class="text-white text-sm font-medium">
                {{ authStore.userName?.charAt(0).toUpperCase() }}
              </span>
            </div>
            <svg class="w-4 h-4 text-google-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
            </svg>
          </button>
          <div
            v-if="isUserDropdownOpen"
            class="absolute right-0 mt-2 w-48 bg-white rounded-google shadow-google py-2 z-20"
          >
            <div class="px-4 py-2 text-sm text-google-gray-900 font-medium border-b border-google-gray-200">
              {{ authStore.userName }}
            </div>
            <a
              href="#"
              @click.prevent="handleLogout"
              class="block px-4 py-2 text-sm text-google-gray-700 hover:bg-google-gray-100 transition-colors"
            >
              ログアウト
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
