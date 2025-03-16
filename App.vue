<template>
  <div class="guide">
    <button class="sign in" @click="islogin">sign in</button>
    <button class="sign up" @click="isloginde">sign up</button>
    <button class="home" @click="ishome">home</button>
  </div>
  
  <!-- <div v-if="islogin1">
   <other></other>
  </div>
    --><login v-if="islogin1==='A'"></login>
    <sign-in v-else-if="islogin1==='B'"></sign-in>
  <div id="app" v-else >
    
    <h1>To-Do List</h1>
    <to-do-form @todo-added="addToDo"></to-do-form>
    <h2 id="list-summary">{{ listSummary }}</h2>
    <ul aria-labelledby="list-summary" class="stack-large">
      <li v-for="item in ToDoItems" :key="item.id">
        <to-do-item
          :label="item.label"
          :done="item.done"
          :id="item.id"
          @checkbox-changed="updateDoneStatus(item.id)"
          @item-deleted="deleteToDo(item.id)"
          @item-edited="editToDo(item.id, $event)"
        ></to-do-item>
      </li>
    </ul>
  </div>
</template>

<script>
import ToDoItem from "./components/ToDoItem.vue";
import uniqueId from "lodash.uniqueid";
import ToDoForm from "./components/ToDoForm";
import Login from "./components/Login.vue";
// import Signln from "./components/SignIn.vue";
import SignIn from "./components/SignIn.vue";
//import { RouterView, RouterLink } from 'vue-router';
// import signln from "./components/other.vue";
export default {
  name: "App",
  components: {
    ToDoItem,
    ToDoForm,
    Login,
    SignIn,
    // Other,
  },
  data() {
    return {
       islogin1 :false ,
       islogin2:false,
      ToDoItems: []
    };
  },
  created() {
    this.loadToDoItems();
  },
  methods: {
    addToDo(toDoLabel) {
      this.ToDoItems.push({ id: uniqueId("todo-"), label: toDoLabel, done: false });
      this.saveToDoItems();
    },
    updateDoneStatus(toDoId) {
      const toDoToUpdate = this.ToDoItems.find(item => item.id === toDoId);
      toDoToUpdate.done =!toDoToUpdate.done;
      this.saveToDoItems();
    },
    deleteToDo(toDoId) {
      const itemIndex = this.ToDoItems.findIndex(item => item.id === toDoId);
      this.ToDoItems.splice(itemIndex, 1);
      this.saveToDoItems();
    },
    editToDo(toDoId, newLabel) {
      const toDoToEdit = this.ToDoItems.find(item => item.id === toDoId);
      toDoToEdit.label = newLabel;
      this.saveToDoItems();
    },
    loadToDoItems() {
      const storedItems = localStorage.getItem("ToDoItems");
      if (storedItems) {
        this.ToDoItems = JSON.parse(storedItems);
      }
    },
    saveToDoItems() {
      localStorage.setItem("ToDoItems", JSON.stringify(this.ToDoItems));
    },
    islogin(){
      
       this.islogin1='A';
    },
    isloginde(){
    this.islogin1="B";
  },
  ishome(){
this.islogin1='C'
  },
  },
  
  computed: {
    listSummary() {
      const numberFinishedItems = this.ToDoItems.filter(item => item.done).length;
      return `${numberFinishedItems} out of ${this.ToDoItems.length} items completed`;
    }
  }
};
</script>
<style>
.guide button{
  margin-left: 10px;
  border-width: 10px 10px 10px 10px;
  background-color: rgb(2, 2, 2);
    color: white;
    padding: 10px 20px 10px 10px;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}
.btn {
    padding: 0.8rem 1rem 0.7rem;
    border: 0.2rem solid #4d4d4d;
    cursor: pointer;
    text-transform: capitalize;
  }
  .btn__danger {
    color: #fff;
    background-color: #ca3c3c;
    border-color: #bd2130;
  }
  .btn__filter {
    border-color: lightgrey;
  }
  .btn__danger:focus {
    outline-color: #c82333;
  }
  .btn__primary {
    color: #fff;
    background-color: #000;
  }
  .btn-group {
    display: flex;
    justify-content: space-between;
  }
  .btn-group > * {
    flex: 1 1 auto;
  }
  .btn-group > * + * {
    margin-left: 0.8rem;
  }
  .label-wrapper {
    margin: 0;
    flex: 0 0 100%;
    text-align: center;
  }
  [class*="__lg"] {
    display: inline-block;
    width: 100%;
    font-size: 1.9rem;
  }
  [class*="__lg"]:not(:last-child) {
    margin-bottom: 1rem;
  }
  @media screen and (min-width: 620px) {
    [class*="__lg"] {
      font-size: 2.4rem;
    }
  }
  .visually-hidden {
    position: absolute;
    height: 1px;
    width: 1px;
    overflow: hidden;
    clip: rect(1px 1px 1px 1px);
    clip: rect(1px, 1px, 1px, 1px);
    clip-path: rect(1px, 1px, 1px, 1px);
    white-space: nowrap;
  }
  [class*="stack"] > * {
    margin-top: 0;
    margin-bottom: 0;
  }
  .stack-small > * + * {
    margin-top: 1.25rem;
  }
  .stack-large > * + * {
    margin-top: 2.5rem;
  }
  @media screen and (min-width: 550px) {
    .stack-small > * + * {
      margin-top: 1.4rem;
    }
    .stack-large > * + * {
      margin-top: 2.8rem;
    }
  }
  /* 全局样式结束 */
  #app {
    background: #fff;
    margin: 2rem 0 4rem 0;
    padding: 1rem;
    padding-top: 0;
    position: relative;
    box-shadow:
      0 2px 4px 0 rgba(0, 0, 0, 0.2),
      0 2.5rem 5rem 0 rgba(0, 0, 0, 0.1);
  }
  @media screen and (min-width: 550px) {
    #app {
      padding: 4rem;
    }
  }
  #app > * {
    max-width: 50rem;
    margin-left: auto;
    margin-right: auto;
  }
  #app > form {
    max-width: 100%;
  }
  #app h1 {
    display: block;
    min-width: 100%;
    width: 100%;
    text-align: center;
    margin: 0;
    margin-bottom: 1rem;
  }
</style>
