<template>
  <h1>To-Do Page</h1>
  <div v-if="loading">
      Loading... 
  </div>
  <form 
    v-else
    @submit.prevent="onSave"
  >
    <div class="row">
        <div class="col-6">
            <div class="form-group">
                <label>Todo Subject</label>
                <input 
                    v-model="todo.subject" 
                    type="text" 
                    class="form-control" 
                >
            </div>  
        </div>
        <div class="col-6">
            <div class="form-group">
                <label>Status</label>
                <div>
                    <button 
                        class="btn btn-danger"
                        :class="todo.completed ? 'btn-success' : 'btn-danger' "
                        @click="toggleTodoStatus"
                        type="button"
                    >
                        {{ todo.completed ? 'Completed' : 'Incomplete' }}
                    </button>
                </div>
            </div>  
        </div>
    </div>  
    <div class="mt-2">
      <button type="submit" class="btn btn-primary ">Save</button>
      <button 
        class="btn btn-outline-dark ms-2"
        @click="moveToTodoListPage"
      >
        Cancle
    </button>
    </div>
  </form>
</template>

<script>
import { ref } from "@vue/reactivity";
import { useRoute, useRouter } from "vue-router";
import axios from "axios";

export default {
    setup() {
        const route = useRoute();
        const router = useRouter();
        const todo = ref(null);
        const loading = ref(true);
        const todoId = route.params.id;
        
        const getTodo = async () => {
            const res = await axios.get(`http://localhost:3000/todos/${todoId}`)

            todo.value = res.data;
            loading.value = false;
        };

        const toggleTodoStatus = () => {
            todo.value.completed = !todo.value.completed;
        };

        const moveToTodoListPage = () => {
            router.push({
                name: "Todos"
            })
        };

        const onSave = async () => {
            await axios.put(`http://localhost:3000/todos/${todoId}`,{
                subject: todo.value.subject,
                completed: todo.value.completed
            });
        }

        getTodo();

        return {
            todo,
            loading,
            toggleTodoStatus,
            moveToTodoListPage,
            onSave,
        }
    }
}
</script>

<style>

</style>