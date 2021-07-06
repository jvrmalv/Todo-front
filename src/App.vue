<template>
  <v-app>
    <v-main>
      <v-container class="py-0 fill-height" align-center fluid>
        <v-row class="" justify="center">
          <v-col cols="12" md="6">
            <todo-list
              v-on:delete-todo="deleteTodo"
              v-on:create-todo="createTodo($event.name, $event.description)"
              v-on:edit-todo="editTodo"
              class=""
              :todolist="todosTest"
            />
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import TodoList from "./components/TodoList.vue";
import axios from "axios";

export default {
  name: "App",

  components: {
    TodoList,
  },

  data: () => ({
    todosTest: [],
  }),
  mounted() {
    axios.get("http://localhost:3000/todo/").then((response) => {
      console.log(response);
      this.todosTest = response.data;
    });
  },
  methods: {
    editTodo({ id, name, description }) {
      console.log(id, name, description);
      const refreshUpdatedTodo = (todo) => (acc, v) => {
        if (v.id === id) return [...acc, todo];
        return [...acc, v];
      };
      axios
        .put(`http://localhost:3000/todo/${id}`, { name, description })
        .then((res) => {
          const todo = res.data;
          console.log("Update todo:", todo);
          this.todosTest = this.todosTest.reduce(refreshUpdatedTodo(todo), []);
          console.log("Todoslist:", this.todosTest);
        });
    },
    deleteTodo: function (id) {
      console.log("Listened id is:", id);
      axios
        .delete(`http://localhost:3000/todo/${id}`)
        .then(
          () =>
            (this.todosTest = this.todosTest.filter((todo) => todo.id !== id))
        );
    },
    createTodo: function (name, description) {
      const todo = { name, description };
      axios
        .post("http://localhost:3000/todo/", todo)
        .then((res) =>
          this.todosTest.push({
            id: res.data.id,
            name: todo.name,
            description: todo.description,
          })
        );
    },
  },
};
</script>
