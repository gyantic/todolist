<script setup lang="ts">
import { ref, computed } from 'vue'

interface Task {
  name: string
  isFinished: boolean
}

const tasks = ref<Task[]>([])
// 完了済みタスクの保持
const finishedTasks = computed(() => tasks.value.filter((task) => task.isFinished))

//未完了タスクの保持
const unfinishedTasks = computed(() => tasks.value.filter((task) => !task.isFinished))

const newTaskName = ref('')

const addTask = () => {
  // バリデーション通過でタスク追加する機能
  if (newTaskName.value === '') {
    alert('タスク名を入力してください')
    return
  }
  if (tasks.value.some((task) => task.name === newTaskName.value)) {
    alert('同じ名前のタスクが存在します')
    return
  }

  // タスクの追加処理
  tasks.value.push({ name: newTaskName.value, isFinished: false})
}

const finishTask = (taskName: string) => {
  //タスクの完了処理
  tasks.value = tasks.value.map((task) => {
    if (task.name === taskName) {
      return {
        ...task,
        isFinished: true
      }
    }
    return task
  })
}
</script>


<template>
  <div>
    <div>TaskList</div>
    <div>完了済みタスク一覧</div>
    <ul>
      <li v-for="task in finishedTasks" :key = "task.name">
        <div>{{ task.name }}</div>
      </li>
    </ul>


    <div>未完了タスク一覧</div>
    <ul>
      <li v-for="task in unfinishedTasks" :key = "task.name">
        <div>
          {{ task.name }}
          <button @click="finishTask(task.name)">完了する</button>
        </div>
      </li>
    </ul>

    <div>
      <label>
        名前
        <input v-model="newTaskName" type="text" />
      </label>
      <botton @click="addTask">追加</botton>
    </div>
  </div>
</template>
