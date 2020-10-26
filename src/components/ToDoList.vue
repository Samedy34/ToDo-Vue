<template>
  <div class="tasks">
    <div @click="showAll"
         class="all-tasks"
         :class="[toggleShowAll ? 'active' : '']"
    >Все задачи
    </div>
    <ul>
      <li v-for="todo in todos"
          :key="todo.id"
          @click="makeActive(todo.id)"
          :class="[activeFolder === todo.id && !toggleShowAll ? 'active' : '']"
      >
        <div class="li-wrap"><span class="circle" :style="{ backgroundColor: todo.color }"></span><span>{{ todo.name }}</span></div>
        <span @click="removeTodo(todo.id)">x</span>
      </li>
    </ul>
    <div class="add-task">
      <div @click="toggleShowAddTask">
        + Добавить папку
      </div>
      <AddTask v-if="showAddTask"
               @add-task="addTask"
               :colors="colors"
               class="add-task-window"
               @close-window="toggleShowAddTask"
               @blur="toggleShowAddTask"
      />
    </div>
  </div>
</template>

<script>
import AddTask from './AddTask'

export default {
  data() {
    return {
      activeFolder: '',
      showAddTask: false
    }
  },
  methods: {
    toggleShowAddTask() {
      this.showAddTask = !this.showAddTask
    },
    addTask(task, color) {
      this.$emit('add-task', task, color)
    },
    removeTodo(id) {
      this.$emit('remove-task', id);
    },
    makeActive(id) {
      this.activeFolder = id;
      this.$emit('active-task', id);
    },
    showAll() {
      this.$emit('show-all');
    }
  },
  components: {
    AddTask
  },
  props: ['todos', 'toggleShowAll', 'colors']
}
</script>

<style lang="scss"
       scoped
>

.circle {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  margin-right: 10px;
}

.li-wrap {
  display: flex;
  align-items: center;
}

.tasks {
  box-sizing: border-box;
  display: flex;
  flex-direction: column;

  text-align: left;
  width: 30%;
  padding: 50px 20px;
  margin: 0px;
  background-color: #dedede;
}

.add-task {
  cursor: pointer;
  color: #000000;
  font-weight: bold;
  margin-top: 80px;
  position: relative;

  &:hover {
    color: #2c3e50;
  }

  &-window {

  }
}

.all-tasks {
  text-align: left;
  cursor: pointer;
  list-style: none;
  text-align: left;
  padding: 10px;
  border-radius: 4px;
  font-size: 16px;
  font-weight: 400;
  margin-bottom: 5px;
  display: flex;
  justify-content: space-between;
}

ul {
  padding: 0;

  li {
    cursor: pointer;
    list-style: none;
    text-align: left;
    padding: 10px;
    border-radius: 4px;
    font-size: 16px;
    font-weight: 400;
    margin-bottom: 5px;
    display: flex;
    justify-content: space-between;

    &:hover {
      background-color: #fff;
      box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.05);
    }
  }
}

.active {
  background-color: #fff;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.05);
}


</style>