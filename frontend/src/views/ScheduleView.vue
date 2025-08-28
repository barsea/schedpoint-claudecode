<script setup>
import { watch } from 'vue'
import TheHeader from '../components/TheHeader.vue'
import TimeAxis from '../components/TimeAxis.vue'
import ScheduleColumn from '../components/ScheduleColumn.vue'
import { usePlanStore } from '@/stores/plan'
import { useActualStore } from '@/stores/actual'
import { useAuthStore } from '@/stores/auth'
import PlanDetailModal from '../components/PlanDetailModal.vue'
import ActualDetailModal from '../components/ActualDetailModal.vue'

const planStore = usePlanStore()
const actualStore = useActualStore()
const authStore = useAuthStore()

watch(
  // 監視対象のデータ
  () => authStore.isLoggedIn,
  // 監視対象のデータが変化したときに実行される関数
  (isLoggedIn) => {
    // ログイン状態が true になった瞬間にデータを取得する
    if (isLoggedIn) {
      const dateToFetch = planStore.currentDate
      Promise.all([planStore.fetchPlans(dateToFetch), actualStore.fetchActuals(dateToFetch)])
    }
  },
  // { immediate: true } オプションにより、コンポーネントがマウントされた直後にも、watch内の処理が一度だけ実行される。
  // これにより、すでにログイン済みの状態でページをリロードした場合にも対応できます。
  { immediate: true },
)

/**
 * 「予定」ブロックがクリックされたときの処理
 * @param {object} plan - クリックされた予定オブジェクト
 */
const handlePlanClick = (plan) => {
  planStore.openPlanModal(plan)
}

/**
 * 「実績」ブロックがクリックされたときの処理
 * @param {object} actual - クリックされた実績オブジェクト
 */
const handleActualClick = (actual) => {
  actualStore.openActualModal(actual)
}
</script>

<template>
  <div class="flex flex-col h-screen bg-google-gray-100 font-google">
    <!-- ヘッダー -->
    <div class="w-full h-[80px] bg-white shadow-google flex-shrink-0">
      <TheHeader />
    </div>

    <!-- メインコンテンツ -->
    <div class="flex flex-grow overflow-hidden">
      <!-- タイムアクシス -->
      <div class="w-[100px] bg-white border-r border-google-gray-200 shadow-sm">
        <TimeAxis />
      </div>
      
      <!-- カレンダーコンテンツ -->
      <div class="flex flex-grow">
        <!-- 予定列 -->
        <div class="flex-1 bg-white border-r border-google-gray-200">
          <ScheduleColumn title="予定" :events="planStore.plans" @plan-click="handlePlanClick" />
        </div>
        
        <!-- 実績列 -->
        <div class="flex-1 bg-white">
          <ScheduleColumn
            title="実績"
            :events="actualStore.actuals"
            @actual-click="handleActualClick"
          />
        </div>
      </div>
    </div>
  </div>

  <!-- 予定詳細モーダル -->
  <PlanDetailModal
    v-if="planStore.isModalOpen && planStore.selectedPlan"
    :plan="planStore.selectedPlan"
  />

  <!-- 実績詳細モーダル -->
  <ActualDetailModal
    v-if="actualStore.isModalOpen && actualStore.selectedActual"
    :actual="actualStore.selectedActual"
  />
</template>
