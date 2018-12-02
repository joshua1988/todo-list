<template>
  <div id="app">
    <header class="header">
      <h1 class="title">Todo List</h1>
    </header>
    <section class="container">
      <div class="todo_box">
          <span class="ipt_tx">
            <input v-model="inputField" @keyup.enter="addTodo" placeholder="Todo Item" />
          </span>
          <button @click="addTodo" class="btn_add">Add Todo</button>
      </div>
      <div class="todo_list">
        <ul class="todo_list_group">
            <li class="todo_list_item" v-for="(todo, index) in todoList" :key="index">
              <div class="todo_list_bx">
                <input type="checkbox" @change="toggle(todo, index)" @checked="todo.complete" class="ipt_chk">
                <del v-if="todo.complete" class="todo_tx">
                    <span>{{ todo.name }}</span>
                </del>
                <span v-else class="todo_tx">
                    <span>{{ todo.name }}</span>
                </span>
                <span @click="deleteTodo(index)" class="todo_delete">X</span>
              </div>
            </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
var storage = {
  fetch: function() {
    return JSON.parse(localStorage.getItem('DAHYUN-TODO'));
  },
  save: function(todoList) {
    var list = JSON.stringify(todoList);
    localStorage.setItem('DAHYUN-TODO', list);
  },
};

export default {
  name: 'app',
  methods: {
    addTodo: function(todo) {
      todo = this.inputField;
      this.todoList.push({ name: todo, complete: false });
      this.inputField = '';
      console.log(this.todoList);
    },
    deleteTodo: function(index) {
      this.todoList.splice(index, 1);
      console.log(this.todoList);
    },
    toggle: function(todo, index) {
      todo.complete = !todo.complete;
      this.todoList.splice(index, 1, todo);
    }
  },
  data() {
    return {
      inputField: '',
      todoList: [],
    }
  },
  created: function() {
    this.todoList = storage.fetch();
  },
  watch: {
    todoList: function(value) {
      storage.save(value);
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

a {
  color: #42b983;
}
.title{font-size:2.5rem;font-weight:bold;color:#222;letter-spacing:-.03em}
.todo_box{display:flex;flex-wrap:wrap;margin-top:1.5rem;justify-content:center}
.ipt_tx{height:38px;margin-right:.25rem;border:1px solid #777}
.ipt_tx input{display:block;width:100%;height:100%;padding:0 10px;border:0;font-size:inherit;box-sizing:border-box}
.todo_box .btn_add{display:inline-block;height:38px;border:1px solid #222;border-radius:8px;background:#222;font-size:1rem;color:#fff}
.todo_list{display:flex;flex-direction:column;max-width:590px;margin:0 auto}
.todo_list_item{margin-top:.5rem;padding:10px 30px;border:1px solid #dee2e6;border-radius:.25rem}
.todo_list_bx{display:flex;flex-wrap:wrap;align-items:center}
.ipt_chk{flex:0 0 8.33%;max-width:8.33%}
.todo_tx{flex:0 0 66.67%;max-width:66.67%;text-align:left;padding:0 15px;font-size:1.25rem;font-weight:bold;line-height:1.2}
.todo_delete{flex:0 0 16.66%;max-width:16.66%;border:0;background:none;text-align:right;color:#777;cursor:pointer}
.todo_delete:hover{color:#000}
</style>
