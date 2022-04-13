<template>
  <section class="gradient-custom-2">
    <div class="container py-5 h-100">
      <div class="row d-flex justify-content-center align-items-center h-100">
        <div class="col col-xl-10">
          <div class="card rounded">
            <div class="card-body p-5">
              <h3 class="mb-3 fw-bold">
                Todo List
                <span class="badge bg-primary rounded-pill ms-3 fs-6 fw-normal"
                  >{{ totalTodo }} task</span
                >
              </h3>

              <div class="row">
                <div class="col-10">
                  <div class="form-floating mb-3">
                    <input
                      type="text"
                      class="form-control fs-4"
                      id="floatingInput"
                      placeholder="Todos"
                      v-model="todo"
                      @keyup.enter="addTodo"
                    />
                    <label for="floatingInput"
                      >What do you need to do today?</label
                    >
                  </div>
                </div>
                <div class="col-2">
                  <div class="d-grid">
                    <button
                      type="button"
                      class="btn btn-primary btn-lg"
                      @click="addTodo"
                    >
                      ADD
                    </button>
                  </div>
                </div>
              </div>

              <List
                :todos="todos"
                @deleteTodo="deleteTodo"
                @doneTodo="doneTodo"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import List from "./components/List.vue";

export default {
  components: { List },
  data() {
    return {
      todo: "",
      todos: [],
    };
  },
  mounted() {
    if (localStorage.getItem("todos") != null) {
      this.todos = JSON.parse(localStorage.getItem("todos"));
    }
  },
  methods: {
    addTodo() {
      if (this.todo != "") {
        this.todos.unshift({
          activity: this.todo,
          isDone: false,
        });
        this.todo = "";
        this.saveToLocalStorage();
      }
    },
    deleteTodo(indexDelete) {
      this.todos = this.todos.filter((item, index) => {
        if (index != indexDelete) {
          return item;
        }
      });
      this.saveToLocalStorage();
    },
    doneTodo(indexDone) {
      this.todos[indexDone].isDone = !this.todos[indexDone].isDone;
      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
  computed: {
    totalTodo() {
      return this.todos.length;
    },
  },
};
</script>

<style>
@import "./assets/styles.css";
</style>
