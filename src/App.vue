<script setup>
import { ref, onMounted, watch, nextTick } from 'vue'
const showForm = ref(false)
const newMemo = ref('')
const memos = ref([])
const memoInput = ref(null)
const errorMessage = ref('')

function addMemo() {
  if (!newMemo.value) {
    errorMessage.value = 'Memo can not be empty'
    return;
  }else {
    errorMessage.value = ''
  }
  memos.value.push({
    id: Date.now(),
    content: newMemo.value,
    date: new Date().toLocaleDateString(),
    backgroundColor: getRandomColor(),
  })
  newMemo.value = ''
  showForm.value = false
}

function getRandomColor() {
  return `#${Math.floor(Math.random() * 16777215).toString(16)}`
}

onMounted(() => {
  document.addEventListener('keydown', (event) => {
    if (event.ctrlKey && event.key === 'Enter') {
      console.log('Ctrl + Enter pressed!')
      showForm.value = true
    }
  })
})

function handleHeaderButtonClick() {
  showForm.value = true
  alert('u can click ctrl + enter to make a new memo')
}

watch(showForm, async (newValue) => {
  if (newValue) {
    console.log('showForm menjadi true')
    console.log('memoInput.value:', memoInput.value)
    await nextTick()
    if (memoInput.value) {
      memoInput.value.focus()
      console.log('Fokus diterapkan pada textarea setelah nextTick')
    } else {
      console.log('memoInput.value masih null atau undefined setelah nextTick')
    }
  }
})
</script>
<template>
  <main>
    <div class="container">
      <header>
        <h1 class="header-title">Memo</h1>
        <button @click="handleHeaderButtonClick" class="header-button">+</button>
      </header>

      <div class="card-container">
        <div
          v-for="memo in memos"
          v-bind:key="memo.id"
          class="card"
          :style="{ backgroundColor: memo.backgroundColor }"
        >
          <p class="card-content">{{ memo.content }}</p>
          <p class="card-date">{{ memo.date }}</p>
        </div>
      </div>
    </div>
    <div v-if="showForm" class="form-overlay">
      <div class="form-modal">
        <button @click="showForm = false" class="form-close-btn">&times;</button>
        <p v-if="errorMessage" class="form-error">{{ errorMessage }}</p>
        <textarea
          v-model="newMemo"
          @keypress.enter="addMemo"
          name="memo"
          id="memo"
          cols="30"
          rows="10"
          ref="memoInput"
        ></textarea>
        <button @click="addMemo" class="form-save-btn">Save</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  widows: 100vw;
}

.container {
  width: 900px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-title {
  font-size: 48px;
  font-weight: bold;
  margin-bottom: 25px;
  color: #495a7d;
}

.header-button {
  border: none;
  width: 50px;
  height: 50px;
  padding: 10px;
  border-radius: 100%;
  background-color: #ff6b6b;
  color: #fff;
  font-size: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  width: 225px;
  height: 225px;
  padding: 15px;
  border-radius: 10px;
  background-color: #ffa6c1;
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.form-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.form-modal {
  width: 420px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.form-save-btn {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: #495a7d;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  color: #fff;
}

.form-close-btn {
  position: absolute;
  top: 5px;
  right: 10px;
  width: 30px;
  height: 30px;
  background-color: transparent;
  border: none;
  font-size: 30px;
  cursor: pointer;
}

.form-error {
  color: red;
  text-align: center;
}
</style>
