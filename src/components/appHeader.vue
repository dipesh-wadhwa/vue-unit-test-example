<template>
  <h2 class="text-center">VUE 3 ToDo App With Rest Api</h2>
  <div class="row d-flex justify-content-center">
    <div class="col-xs-12 col-lg-6 my-3">
      <div class="form-group mb-3">
        <label for="todo" class="form-label">Add ToDo</label>
        <div class="row">
          <div class="col-10">
            <input
              v-model="todo"
              type="text"
              class="form-control"
              name="todo"
              id="todo"
              placeholder="Enter New Todo"
              v-bind:class="{
                'is-invalid': input_errors.length > 0,
                'is-valid': input_errors.length == 0 && todo != '',
              }"
            />
            <div class="invalid-feedback">
              <span :key="key" v-for="(error, key) in input_errors">{{
                error
              }}</span>
            </div>
          </div>
          <div class="col-2 d-grid gap-2">
            <button class="btn btn-primary btn-s float-end" @click="save">
              Add
            </button>
          </div>
        </div>
      </div>
      <div class="form-group"></div>
    </div>
  </div>
  <toDoList :newTodo="newTodo"></toDoList>
</template>

<script>

import toDoList from '@/components/toDoList.vue'
export default {
  components: {
    toDoList
  },
    data() {
        return {
            todo: "",
            ToDos: [],
            errors: [],
            newTodo: {
                todo: "",
                done: false
            },
            responseData: null,
            input_errors: []
        };
    },
    methods: {
        async save() {
            if (this.input_errors.length > 0 || this.todo == '') {
                if (this.todo == '' && this.input_errors.length == 0)
                    this.input_errors.push('ToDo field cannot be left blank')
                this.input_errors.forEach((value) => {
                    this.$notify(value);
                });
            } else {
                try {
                    this.newTodo.todo = this.todo;
                    const response = await this.$axios.post("/todos", this.newTodo);
                    this.responseData = response.data;
                } catch (error) {
                    this.errors.push(error);
                }
                this.$notify("Added Succesfully");
                this.todo = "";
            }
        },
    },
    watch: {
        todo(val) {
            this.input_errors = [];
            if (val == '') {
                this.input_errors.push('ToDo field cannot be left blank')
                return;
            }
            if (val.length < 3 || val.length > 40) {
                this.input_errors.push('ToDo field be Minimum 6, Maximum 25 characters')
                return;
            }
        },
    }
};
</script>