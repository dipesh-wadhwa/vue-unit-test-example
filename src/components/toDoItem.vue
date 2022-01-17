<template>
    
          <div class="col-8">
            <span
              v-bind:style="todo.done ? 'text-decoration:line-through;' : ''"
              >{{ todo.todo }}</span
            >
          </div>
          <div class="col-2 d-flex justify-content-center">
            <span
              class="form-check form-switch"
              @click="done_todo(todo)"
              style="margin-left: 20px"
            >
              <input
                class="form-check-input"
                type="checkbox"
                role="switch"
                id="flexSwitchCheckDefault"
                data-onstyle="#1f2023"
                :checked="todo.done"
              />
            </span>
          </div>
          <div class="col-2 d-flex justify-content-center">
              <deleteBtn :getTodos="getTodos" :todo="todo"></deleteBtn>
          </div>
</template>
<script>
import deleteBtn from '@/components/deleteBtn.vue';
export default {
    components:{
        deleteBtn
    },
    props: {
        getTodos:Function,
        todo:Object
    },
   methods: {
    async done_todo(todo) {
            try {
                const response = await this.$axios.put("/todos/" + todo.id, { "todo": todo.todo, "done": !todo.done });
                this.responseData = response.data;
            } catch (error) {
                this.errors.push(error);
            }
            this.getTodos();
            this.$notify("Updated Succesfully");
        },
   }
}
</script>
