<template>
  <div id="todoListPage" class="bg-half">
    <nav>
      <h1><a href="#">ONLINE TODO LIST</a></h1>
      <ul>
        <li class="todo_sm">
          <a href="#"><span>emma的代辦</span></a>
        </li>
        <li><a href="#" @click.prevent="handleLogout">登出</a></li>
      </ul>
    </nav>
    <div class="conatiner todoListPage vhContainer">
      <div class="todoList_Content">
        <TodoForm @add-todo="addTodo"></TodoForm>
        <TodoList v-if="todos.length" :todos="todos" @remove-todo="removeTodo"></TodoList>
        <div v-else class="empty-state">
          <p class="mt-10 text-center">目前尚無待辦事項</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import TodoForm from '@/components/TodoForm.vue'
import TodoList from '@/components/TodoList.vue'
import Swal from 'sweetalert2'
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const todos = ref([
  { id: 1, content: '拍大頭照', status: false },
  { id: 2, content: '修機車', status: true },
  { id: 3, content: '打電話請媽媽去拿機車', status: false },
  { id: 4, content: '買晚餐', status: true },
  { id: 5, content: '運動', status: false },
])

const addTodo = (content) => {
  if (!content || content.trim() === '') return

  todos.value.push({
    id: Date.now(),
    content: content.trim(),
    status: false,
  })
}

const removeTodo = (id) => {
  todos.value = todos.value.filter((t) => t.id !== id)
}

const router = useRouter()

const handleLogout = async () => {
  document.cookie = 'vue3-todolist-token=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/'
  await Swal.fire({
    icon: 'success',
    title: '您已成功登出',
    showConfirmButton: false,
    timer: 1500,
  })
  router.push('/login')
}
</script>
