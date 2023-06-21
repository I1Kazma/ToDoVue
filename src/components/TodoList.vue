<template>
  <div id="todo-list">
    <div class="list-item" v-for="n in todos" :key="n.id">
      <div class="list-item-holder" v-if="n.location == location" :data-status="n.completed">
        <div class="checkbox-items" :data-status="n.completed">
          <!-- Проверяем, активно ли редактирование задачи -->
          <input type="checkbox" :data-id="n.id" :id="n.id" @click="updateTodo" :checked="n.completed" />
          <template v-if="!n.editing">
            <div>
              <label :data-id="n.id" :for="n.id">{{ n.name }}</label>
            </div>
            <div class="item-options">
              <!-- Добавляем кнопку "Изменить" -->
              <div class="button-item" @click="startEditing(n)">Изменить</div>
              <div class="button-item" @click="deleteItem" :data-id="n.id">Удалить</div>
            </div>

          </template>
          <!-- Если редактирование задачи активно, отображаем инпут -->
          <template v-else>
            <input type="text" v-model="n.name" @keyup.enter="saveEditing(n)" />
            <div class="item-options">
              <!-- Добавляем кнопку "Сохранить" -->
              <div class="button-item" @click="saveEditing(n)">Сохранить</div>
            </div>
          </template>
        </div>
      </div>
    </div>
    <div id="new-todo-list-item">
      <input type="text" placeholder="Добавьте новую задачу" id="new-todo-list-item-input" @keyup="updateItemText" />
      <input type="submit" id="new-todo-list-item-submit" @click="newItem" value="Добавить" />
    </div>
  </div>
</template>

<script>
import { useStore } from 'vuex'
import { v4 as uuidv4 } from 'uuid'

export default {
  name: "TodoList",
  data() {
    return {
      newTodoItem: ''
    }
  },
  props: {
    location: String
  },
  setup() {
    const store = useStore()
    return {
      todos: store.getters.todos,
    }
  },
  methods: {
    updateItemText: function (e) {
      this.newTodoItem = e.currentTarget.value;

      if (e.keyCode === 13) {
        this.newItem();
      }

      return false;

    },
    updateTodo: function (e) {
      let newStatus = e.currentTarget.parentElement.getAttribute('data-status') == "true" ? false : true;
      this.$store.commit('updateTodo', {
        id: e.currentTarget.getAttribute('data-id'),
        completed: newStatus
      })
    },
    deleteItem: function (e) {
      this.$store.commit('deleteTodo', {
        id: e.currentTarget.getAttribute('data-id')
      })
    },
    newItem: function () {
      if (this.newTodoItem !== '') {
        this.$store.commit('addTodo', {
          id: uuidv4(),
          name: this.newTodoItem,
          completed: false
        })
      }
    },
    archiveItem: function (e) {
      this.$store.commit('moveTodoItem', {
        id: e.currentTarget.getAttribute('data-id'),
        location: 'archive'
      })
    },

    startEditing(todo) {
      // Устанавливаем флаг редактирования для выбранной задачи
      todo.editing = true;
    },

    saveEditing(todo) {
      // Сохраняем измененное значение задачи
      todo.editing = false;
      // Вызываем необходимые действия или мутации для сохранения изменения в хранилище
      this.$store.commit('updateTodo', {
        id: todo.id,
        name: todo.name,
        completed: todo.completed
      });
    }
  }

}
</script>
<style scoped>
#todo-list {
  border-radius: 14px;
  border: 2px solid #ddd;
  background: white;
}

.list-item-holder {
  display: flex;
  padding: 1rem 1rem;
  justify-content: space-between;
  border-bottom: 1px solid #ddd;
}

.item-options,
.item-checkbox {
  display: flex;
}

#new-todo-list-item {
  padding: 1rem;
}

#new-todo-list-item input[type="text"] {
  margin: 0 0 1rem 0;
}

.button-item {
  font-size: 0.875rem;
  background: #eee;
  margin: 0 0 0 0.5rem;
  height: 1rem;
  border-radius: 100px;
  transition: all 0.1s ease-out;
  color: rgba(0, 0, 0, 0.5);
  cursor: pointer;
  padding: 0.25rem 0.75rem;
}

.checkbox-items {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%
}

.button-item:hover {
  background: #ddd;
  color: black;
}

[data-status="false"] label {
  color: #0428ff;
  font-weight: 600;
}

[data-status="true"] label {
  text-decoration: line-through;
}
</style>