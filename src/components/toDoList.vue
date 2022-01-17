<template>
  <div class="row d-flex justify-content-center mt-3" v-if="ToDos.length">
    <div class="col-md-6">
      <h5 class="mb-3">ToDo List</h5>
      <div style="list-style-type: none">
        <li
          v-for="(todo, index) in ToDos"
          v-bind:key="index"
          class="row"
          style="
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 24px;
            border-radius: 6px;
            margin-bottom: 12px;
            border: 2px solid hsla(0, 0%, 0%, 0.35);
          "
        >
        <toDoItem :getTodos="getTodos" :todo="todo"></toDoItem>
        </li>
      </div>
    </div>
  </div>
</template>
<script>
import toDoItem from '@/components/toDoItem.vue';
export default {
    props: {
        newTodo:Object
    },
    components: {
        toDoItem,
    },
    data() {
        return {
            todo: "",
            ToDos: [],
            errors: [],
            responseData: null,
            input_errors: []
        };
    },
    mounted() {
        this.getTodos();
    },
    methods: {
        async getTodos() {
            try {
                const response = await this.$axios.get("/todos");
                this.ToDos = response.data;
            } catch (error) {
                this.errors.push(error);
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
        newTodo: {
            deep: true,
             handler: function(){
                this.getTodos();
            }
        }
    }
};
</script>