<template>
  <div class="todo-list-container">
    <div class="add-todo-row">
      <input
        v-model="newTodo"
        placeholder="添加新任务"
        class="add-todo-input"
      />
      <button class="add-todo-btn" @click="addTodo">添加</button>
    </div>
    <ul>
      <li
        v-for="todo in todos"
        :key="todo.id"
        :class="['todo-item', { completed: todo.completed }]"
      >
        <input type="checkbox" v-model="todo.completed" @change="updateTodo(todo)" />
        <span :class="['todo-title', { completed: todo.completed }]">{{ todo.title }}</span>
        <button class="todo-delete" @click="deleteTodo(todo.id)">🗑</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      newTodo: ''
    }
  },
  mounted() {
    this.fetchTodos(); // 页面加载时获取所有事项
  },
  methods: {
    // 获取所有事项
    fetchTodos() {
      fetch('http://localhost:3000/api/todos')
        .then(res => res.json())
        .then(data => {
          this.todos = data;
        });
    },
    // 添加事项
    addTodo() {
      if (!this.newTodo.trim()) return;
      fetch('http://localhost:3000/api/todos', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ title: this.newTodo })
      })
        .then(res => res.json())
        .then(newTodo => {
          this.todos.push(newTodo);
          this.newTodo = '';
        });
    },
    // 删除事项
    deleteTodo(id) {
      fetch(`http://localhost:3000/api/todos/${id}`, {
        method: 'DELETE'
      })
        .then(res => res.json())
        .then(() => {
          this.todos = this.todos.filter(todo => todo.id !== id);
        });
    },
    // 修改事项（如完成状态）
    updateTodo(todo) {
      fetch(`http://localhost:3000/api/todos/${todo.id}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          title: todo.title,
          completed: todo.completed,
          tomatoCount: todo.tomatoCount
        })
      });
    }
  }
}
</script>

<style scoped>


.add-todo-row {
  display: flex;
  gap: 14px;
  margin-bottom: 36px;
}

.add-todo-input {
  flex: 1;
  background: rgba(255,255,255,0.95);
  border: none;
  border-radius: 12px;
  padding: 14px 18px;
  color: #333;
  font-size: 18px;
  outline: none;
  box-shadow: 0 2px 8px rgba(0,0,0,0.04);
}

.add-todo-btn {
  background: linear-gradient(90deg, #a1c4fd 0%, #c2e9fb 100%);
  border: none;
  border-radius: 12px;
  padding: 14px 32px;
  color: #333;
  font-size: 18px;
  cursor: pointer;
  transition: background 0.2s, transform 0.2s;
  font-weight: bold;
}
.add-todo-btn:hover {
  background: linear-gradient(90deg, #89f7fe 0%, #66a6ff 100%);
  transform: translateY(-2px) scale(1.04);
}

.todo-item {
  display: flex;
  align-items: center;
  background: rgba(255,255,255,0.85);
  margin-bottom: 18px;
  padding: 16px 14px;
  border-radius: 14px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.04);
  transition: background 0.3s, color 0.3s;
}

.todo-item.completed {
  background: rgba(220,220,220,0.5);
  color: #bbb;
}

.todo-title {
  flex: 1;
  font-size: 18px;
  margin-left: 10px;
  color: #333;
  transition: color 0.3s;
}

.todo-title.completed {
  text-decoration: line-through;
  color: #bbb;
  font-style: italic;
}

.todo-delete {
  background: none;
  border: none;
  color: #ffb3b3;
  font-size: 18px;
  cursor: pointer;
  margin-left: 10px;
  transition: color 0.2s;
}
.todo-delete:hover {
  color: #ff5252;
}

.timer-root {
  /* ... */
  border: 1px solid rgba(161,196,253,0.3); /* 更淡的分割线 */
  /* 或者直接去掉 border */
}
</style>