<template>
  <div class="listbox">
    <div class="header">
      <h1 class="todoHeader">Todo List</h1>
      <div class="inputWrapper">
        <input
          type="text"
          id="userInput"
          name="userInput"
          v-model="userInput"
          class="todoItemInput"
          v-on:keyup.enter="addItem"
        />
        <img src="../assets/add.png" class="addItem" @click="addItem" />
      </div>
    </div>
    <div class="itemsBox">
      <div class="item" v-for="(item, i) in list" :key="item.id">
        <div class="itemText">
          <h2 class="todoText">{{ item.title }}</h2>
          <h3>{{ item.date }}</h3>
        </div>
        <div class="itemButtons">
          <img src="../assets/edit.png" class="itemEdit" alt="Edit" @click="editItem(item, i)" />
          <img src="../assets/delete.png" class="itemDelete" alt="Del" @click="deleteItem(item, i)" />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: 'todoList',
  data() {
    return {
      userInput: '',
      list: [],
    }
  },
  mounted() {
    this.getTodos();
  },
  methods: {
    async getTodos() {
      const response = await axios.get("api/Todolist/");
      this.list = response.data;
    },
    async addItem() {
      if (this.userInput !== '') {
        const response = await axios.post("api/Todolist/", {
          title: this.userInput,
          date: this.getDate()
        });
        this.list.push(response.data)
        this.userInput = ''
      }
    },
    async deleteItem(item, i) {
      await axios.delete("api/Todolist/" + item._id,{
            title: "newtitle",
            date: this.getDate()
          });
      this.list.splice(i, 1)
    },
    async editItem(item, i) {
      const newItem = prompt('Edit todo', this.list[i].title)
      if (newItem != null && newItem != '') {
        try{
          await axios.put("api/Todolist/" + item._id, {
            title: newItem,
            date: this.getDate()
          }); 
          await this.getTodos();
        }
        catch(error){
          console.error(error)
        }
      }
    },
    getDate() {
      var curDate = new Date()
      return curDate.getMonth() + 1 + '/' + curDate.getDate() + '/' + curDate.getFullYear()
    },
  },
}
</script>
<style scoped>
.listbox {
  display: grid;
  grid-template-rows: 10rem 1fr;
  grid-template-columns: 3rem 1fr 3rem;
  justify-content: center;
  align-content: center;
  justify-self: center;
  align-self: center;
  height: 100%;
  width: 35%;
  border-radius: 6%;
  background-color: lavender;
  color: black;
}
@media (max-width: 1400px) {
  .listbox {
    width: 50%;
  }
}
h1 {
  font-size: 50px;
}
.todoHeader {
  grid-column: 1 / 3;
  align-self: center;
  justify-self: center;
}
.header {
  grid-column: 2;
  display: grid;
  grid-template-rows: 1fr;
  grid-template-columns: 1fr;
  justify-content: center;
  align-content: center;
}
.inputWrapper {
  align-self: center;
  justify-self: center;
  width: 18vw;
  display: flex;
  position: relative;
}
.todoItemInput {
  width: 18vw;
  height: 2.5rem;
  grid-column: 1;
  grid-row: 2;
  border-radius: 10px;
  justify-self: end;
  align-self: end;
}
.addItem {
  width: 2.5rem;
  height: 2.5rem;
  justify-self: end;
  position: absolute;
  right: 0;
  opacity: 30%;
}
.addItem:hover {
  cursor: pointer;
}
.itemsBox {
  width: 20vw;
  height: 50vh;
  grid-row: 2;
  grid-column: 2;
  align-self: start;
  justify-self: center;
  margin-top: 1rem;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
}
.item {
  display: grid;
  grid-template-columns: 1fr 0.2fr;
  grid-template-rows: 1fr;
  width: 18vw;
  margin-bottom: 0.5rem;
  background-color: lightblue;
  border: 1px solid black;
}
.itemText {
  margin-left: 1rem;
  grid-column: 1;
  display: flex;
  flex-direction: column;
}
.todoText {
  overflow: hidden;
  width: 13vw;
  flex-direction: row;
  word-wrap: break-word;
}
.itemButtons {
  grid-column: 2;
  grid-row: 1;
  display: flex;
  justify-content: space-around;
  margin-top: 0.5rem;
}
.itemDelete {
  height: 1.5rem;
  width: 1.5rem;
}
.itemDelete:hover {
  cursor: pointer;
}
.itemEdit {
  height: 1.5rem;
  width: 1.5rem;
}
.itemEdit:hover {
  cursor: pointer;
}
@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
