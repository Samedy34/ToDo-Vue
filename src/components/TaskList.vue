<template>

  <div class="list">
    <div v-if="activeList">
      <h1 :style="{ color: activeList.color }">
        {{ activeList.name }}
      </h1>


      <ul>
        <li v-for="(list, index) in activeList.list"
            :key="index"
        ><input :checked="list.done"
                @change="$emit('toggle-checkbox', activeList, index)"
                type="checkbox"
                :id='index'
        > <label :for="index">{{ list.name }}</label>
        </li>
      </ul>

      <div @click="toggleAddNewTask"
           class="new-task"
           v-if="!newTaskWindow"
      >
        + Новая задача
      </div>
      <div v-if="newTaskWindow">
        <input @keyup.enter="addTask"
               v-model="newTask"
               type="text"
               placeholder="Введите задачу"
               ref="input"
        >
        <button @click="addTask">Добавить</button>
        <button @click="toggleAddNewTask">Отмена</button>
      </div>
    </div>
    <div v-else-if="!showAll">
      <h1>
        Нет выбранного списка задач!
      </h1>
    </div>
    <div v-if="showAll">
      <ul class="showAllUl">
        <li v-for="(todo, index) in todos"
            :key="index"
        >
          <h1 :style="{ color: todo.color }">{{ todo.name }}</h1>
          <ul>
            <li v-for="(list, index) in todo.list"
                :key="index"
            ><input :checked="list.done"
                    disabled
                    type="checkbox"
                    :id='index'
            >
              {{ list.name }}
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>

</template>

<script>


export default {
  data() {
    return {
      newTaskWindow: false,
      newTask: '',
    }
  },
  watch: {
    activeList: function () {
      this.newTaskWindow = false
    }

  },
  methods: {
    toggleAddNewTask() {
      this.newTaskWindow = !this.newTaskWindow;
    },
    addTask() {
      if (this.newTask !== '') {
        this.$emit('new-task', this.newTask);
        this.newTask = '';
      } else {
        alert('Ну введи ты таск епт!')
      }
    }
  },
  mounted() {

  },
  props: [
    'activeList',
    'todos',
    'showAll'
  ]
}
</script>

<style lang="scss"
       scoped
>
h1 {
  margin: 0px 0px 20px;
  padding: 0px 0px 20px;
  border-bottom: 1px solid #F2F2F2;
  display: block;
  width: 100%;
}

.list {
  box-sizing: border-box;
  padding: 50px;
  text-align: left;
  width: 70%;
  overflow: auto;
}

ul {
  margin: 0 0 20px;
  padding: 0px;

  li {
    list-style: none;
    margin-bottom: 20px;
  }

  li[checkbox] {

  }
}

.new-task {
  font-weight: bold;
  cursor: pointer;

  &:hover {
    color: #767676;
  }
}

.showAllUl {
  padding: 0;
}

button {
  margin-right: 10px;
  margin-top: 10px;
}

input[type="checkbox"] {
  width: auto;
}


</style>