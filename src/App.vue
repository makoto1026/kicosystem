<template>
  <div class="wrapper">
    <h1>日付ごとの評価記録</h1>

    <div class="calendar">
      <input type="date" v-model="selectedDate" @change="loadFromStorage" />
    </div>

    <div class="mark-list">
      <div v-for="i in 10" :key="i" class="mark-row">
        <div class="number">{{ i }}</div>
        <div class="buttons">
          <button
            v-for="m in marks"
            :key="m"
            :class="{ active: data[i] === m }"
            @click="() => (data[i] = m)"
          >
            {{ m }}
          </button>
        </div>
      </div>
    </div>

    <button class="save-button" @click="save">保存する</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const marks = ['×', '△', '○']
const selectedDate = ref(new Date().toISOString().slice(0, 10))
const data = ref({})

// localStorage キー生成
const getKey = (date) => `score-${date}`

// データ読み込み
function loadFromStorage() {
  const raw = localStorage.getItem(getKey(selectedDate.value))
  data.value = raw ? JSON.parse(raw) : {}
}

// 保存処理
function save() {
  localStorage.setItem(getKey(selectedDate.value), JSON.stringify(data.value))
  alert('保存しました')
}

// 初期ロード
loadFromStorage()
</script>

<style scoped>
.wrapper {
  max-width: 480px;
  margin: 0 auto;
  padding: 16px;
  font-family: sans-serif;
}

h1 {
  font-size: 20px;
  margin-bottom: 16px;
  text-align: center;
}

.calendar {
  display: flex;
  justify-content: center;
  margin-bottom: 24px;
}

input[type="date"] {
  font-size: 16px;
  padding: 8px;
  width: 100%;
  max-width: 300px;
}

.mark-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-bottom: 32px;
}

.mark-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.number {
  font-size: 18px;
  width: 24px;
}

.buttons {
  display: flex;
  gap: 8px;
}

.buttons button {
  font-size: 18px;
  padding: 8px 14px;
  border-radius: 8px;
  border: 1px solid #ccc;
  background: #f9f9f9;
  transition: all 0.2s;
}

.buttons button.active {
  background-color: #d0ebff;
  font-weight: bold;
  border-color: #339af0;
}

.save-button {
  width: 100%;
  padding: 14px;
  font-size: 16px;
  border: none;
  background-color: #339af0;
  color: white;
  border-radius: 8px;
}
</style>
