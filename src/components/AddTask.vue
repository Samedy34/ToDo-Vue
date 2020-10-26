<template>
  <div class="addTodo">
    <div class="close" @click="closeWindow">
      x
    </div>
    <label>
      <input v-model="name"
             @keyup.enter="addTask"
             type="text"
             placeholder="Введите название папки"
             ref="input"
      />
    </label>
    <div class="circles">
      <i v-for="(color, index) in colors"
         :key="index"
         :style="{ backgroundColor: color }"
         @click="chooseColor(index)"
         :class="[activeColor === index ? 'active' : '' ]"
      ></i>
    </div>
    <button @click="addTask">Добавить</button>
  </div>
</template>

<script>



export default {
  data() {
    return {
      name: '',
      activeColor: 0,
      popupItem: ''
    }
  },
  methods: {
    addTask() {
      if (this.name !== '') {
        this.$emit('add-task', this.name, this.activeColor);
      } else {
        alert('Введи че нить че как лох')
      }
      this.name = '';
    },
    chooseColor(index){
      this.activeColor = index;
    },
    closeWindow(){
      this.$emit('close-window')
    }
  },
  mounted() {
    this.popupItem = this.$el
    this.$refs.input.focus();
  },
  directives: {

  },
  props: ['colors']
}
</script>


<style lang="scss"
       scoped
>
.addTodo {
  box-sizing: border-box;
  cursor: auto;
  padding: 18px;
  background: #FFFFFF;
  box-shadow: 0px 6px 10px rgba(0, 0, 0, 0.30);
  border-radius: 10px;
  position: absolute;
  top: 30px;
  left: 20px;
  display: flex;
  flex-direction: column;
  width: 240px;
}

input {
  margin-bottom: 10px;
}

.circles{
  display: flex;
  justify-content: space-between;
}

i {
  width: 10px;
  height: 10px;
  background-color: #000;
  border-radius: 50%;
  margin-bottom: 10px;
  cursor: pointer;
  border: 3px solid transparent;
  display: inline-block;

  &.active {
    border: 3px solid #767676;
  }
}

.close{
  cursor: pointer;
  position: absolute;
  width: 20px;
  height: 20px;
  background-color: #5C5C5C;
  top: -8px;
  right: -8px;
  color: #fff;
  text-align: center;
  border-radius: 50%;
}
</style>