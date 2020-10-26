<template>
  <div class="application">
    <ToDoList @add-task="addNewTask"
              @active-task="getActiveTask"
              @remove-task="removeTask"
              :todos="todos"
              :toggleShowAll="showAll"
              :colors="colors"
              @show-all="showAllLists"
    />
    <TaskList :activeList="activeList"
              :todos="todos"
              :showAll="showAll"
              @new-task="addItem"
              @toggle-checkbox="toggleCheckbox"

    />
  </div>
</template>


<script>
import ToDoList from './components/ToDoList'
import TaskList from "@/components/TaskList";

export default {
  data() {
    return {
      todos: [],
      colors: ['#42B883', '#64C4ED', '#FFBBCC', '#B6E6BD', '#C9D1D3', '#C355F5', '#FF6464'],
      activeList: '',
      activeId: '',
      showAll: true
    }
  },
  components: {
    ToDoList,
    TaskList
  },
  watch: {
    todos: {
      handler() {
        console.log('Todos changed!');
        localStorage.setItem('todos', JSON.stringify(this.todos));
      },
      deep: true,
    }
  },
  methods: {
    toggleCheckbox(item, index) {
      console.log(item.id, item.list[index])
      const activeTodo = this.todos.find(todo => todo.id === item.id);
      activeTodo.list[index].done = !activeTodo.list[index].done
      console.log(this.todos)
    },
    addNewTask(task, color) {
      const newTask = {
        id: Date.now(),
        name: task,
        list: [],
        color: this.colors[color]
      }

      this.todos.push(newTask)
    },
    getActiveTask(id) {
      const activeTodo = this.todos.find(todo => todo.id === id);
      this.activeList = activeTodo;
      this.showAll = false;
      this.activeId = id;
    },
    addItem(item) {
      console.log(item)
      item = {
        name: item,
        done: false
      }
      const activeTodo = this.todos.find(todo => todo.id === this.activeId);
      activeTodo.list.push(item);
      console.log(activeTodo.list)
    },
    removeTask(id) {
      const activeTodo = this.todos.find(todo => todo.id === id);
      const index = this.todos.indexOf(activeTodo)
      this.todos.splice(index, 1)
    },
    showAllLists() {
      this.showAll = true;
      this.activeList = '';
    }
  },
  mounted() {
    console.log('App mounted!');
    if (localStorage.getItem('todos')) {
      this.todos = JSON.parse(localStorage.getItem('todos'))
    } else {
      this.todos = [
        {
          id: 1,
          name: 'Для Лехи',
          list: [{
            name: 'Написать Лехе',
            done: true
          }, {
            name: 'Позвонить Лехе',
            done: false
          }],
          color: '#42B883'
        },
        {
          id: 2,
          name: 'Для Соли',
          list: [{
            name: 'Куку Епта',
            done: true
          }, {
            name: 'Сделай это',
            done: false
          },
            {
              name: 'Не отдать хаосы',
              done: true
            }],
          color: '#FF6464'
        },
      ]
    }
    ;
  },
}
</script>


<style lang="scss">
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap');
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.application {
  width: 750px;
  height: 530px;
  border: 1px solid #2c3e50;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  margin: 0;
}

input {
  padding: 10px;
  border: 1px solid #EFEFEF;
  box-sizing: border-box;
  border-radius: 4px;
  width: 100%;
}

button {
  padding: 9px;
  background-color: #4DD599;
  border-radius: 4px;
  cursor: pointer;
  outline: none;
  border: 0px;
  color: #fff;
  font-weight: 600;

  &:hover {
    background-color: #6fe3b3;
  }
}


</style>
