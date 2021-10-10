<template>
  <div id="" class=" container">
    <h1>{{ todo }}</h1>
    <h1>
      Todos List
      <span>{{ todoList.length }} </span>
    </h1>
    <form class="row m-2 d-inline">
      <div class="col mx-auto d-inline">
        <div class="form-group  m-2">
          <label htmlFor="t" class="p-2">
            Titre
          </label>
          <input
            type="text"
            class="form-control"
            id="t"
            name="t"
            v-model="titre"
          />
          <!-- -->
        </div>
        <div class="form-group  m-2">
          <label htmlFor="todo" class="p-2">
            Todo
          </label>
          <input
            type="text"
            class="form-control"
            id="todo"
            name="todo"
            v-model="todo"
          />
          <!-- " -->
        </div>
      </div>
      <button class="m-2 btn btn-primary submit col-3" v-on:click="submitTodo">
        Ajouter
      </button>
    </form>

    <!-- {{ ( c = contrast(val.color)) }} -->
    <!-- -->
    <div
      v-for="item in todoList"
      :key="item.id"
      class="section m-2 p-3 "
      :style="{ background: item.color, color: contrast(item.color) }"
    >
      <div class="row mx-auto">
        <div class="col mx-auto df">
          <p class="mx-auto m-0">
            <span>
              <strong>
                {{ item.titre + ": " }}
              </strong>
            </span>
            {{ item.todo }}
          </p>
        </div>
        <div
          type="button"
          class="text-center btn btn-danger bt col-1"
          v-on:click="deleteTodo(item.id)"
        >
          <i class="mx-auto  fa fa-trash" aria-hidden="true"></i>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Todo",
  data() {
    return {
      msg: "",
      titre: "",
      todo: "",
      todoList: {},
    };
  },

  mounted() {
    this.getTodoList();
  },
  methods: {
    contrast(color) {
      let c = color.substr(1);
      return "#" + (0xffffff - parseInt(c, 16)).toString(16);
    },
    async getTodoList() {
      await axios
        .get("http://localhost:3001/api/get")
        .then((res) => (this.todoList = res.data))
        .catch((error) => {
          console.log(error);
        });
    },
    submitTodo(event) {
      event.preventDefault();
      const colorRandom = "#" + ((Math.random() * 0xeeeeee) << 0).toString(16);
      const ntodo = {
        titre: this.titre,
        todo: this.todo,
        color: colorRandom,
      };
      axios
        .post("http://localhost:3001/api/insert", ntodo)
        .then(this.getTodoList());
      this.todo = "";
      this.titre = "";
    },

    deleteTodo(id) {
      axios
        .delete(`http://localhost:3001/api/delete/${id}`)
        .then(this.deleteElm(id));
    },
    deleteElm(id) {
      this.todoList = this.todoList.filter((todoelm) => todoelm.id !== id);
    },
  },
};
</script>

<style scoped>
body {
  margin-top: 3rem !important;
  font-family: Helvetica, Arial, sans-serif;
}

.bt {
  width: 30px !important;
  height: 30px !important;
  font-size: 18px;
  padding: 0;
}

.section {
  position: relative;
  border-radius: 10px;
  min-height: 30px;
  line-height: 30px;
  background-color: antiquewhite;
}

.bt {
  position: absolute;
  right: 0;
  top: 0;
}

h1 {
  text-align: center;
  padding: 2rem;
}

.submit {
  max-width: 100px;
  max-height: 45px;
}

form {
  align-items: end;
}

h1 > span {
  border-radius: 50%;
  width: 35px !important;
  height: 35px !important;
  padding: 12px 12px;
  background: rgb(168, 9, 9);
  color: rgb(248, 238, 238);
  text-align: center;
  font: 30px Arial, sans-serif;
}
</style>
