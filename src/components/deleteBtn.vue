<template>
    <button class="btn btn-danger btn-sm" @click="delete_todo(todo.id)">
        Delete
    </button>
</template>
<script>

export default {
    props: {
        getTodos:Function,
        todo:Object
    },
   methods: {
       async delete_todo(index) {
            try {
                const response = await this.$axios.delete("/todos/" + index);
                this.responseData = response.data;
            } catch (error) {
                this.errors.push(error);
            }
            this.getTodos();
            this.$notify("Deleted Succesfully");
        },
   }
}
</script>