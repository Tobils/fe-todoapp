<template>
    <div class="hello">
        <div id="todo-list-example" class="container">
            <div class="row">
                <div class="col-md-6 mx-auto">
                    <h1 class="text-center">To Do List App</h1>
                    <form v-on:submit.prevent="addNewTask">
                        <label for="tasknameinput">Task Name</label>
                        <input type="text" id="tasknameinput" class="form-control" placeholder="Add new task">
                        <button v-if="this.isEdit == flase " type="submit" class="btn btn-success btn-block mt-3">
                            Submit
                        </button>
                        <button v-else type="button" class="btn btn-primary btn-block mt-3">
                            Update
                        </button>
                    </form>

                    <table class="table">
                        <tr v-for="(todo) in todos" v-bind:key="todo.id" v-bind:title="todo.task_name">
                            <td class="text-left">
                                {{ todo.task_name }}
                            </td>
                            <td class="text-right">
                                <buttton class="btn-info" v-on:click="editTask(todo.task_name, todo.id)">Edit</buttton>
                                <buttton class="btn-danger" v-on:click="deleteTask(todo.id)">Delete</buttton>
                            </td>
                        </tr>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            todos:[],
            id:'',
            taskname:'',
            isEdit:false
        }
    }, 
    mounted(){
        this.getTasks()
    },
    methods: {
        getTasks() {
            axios.get("/api/tasks").then(
                result => {
                    console.log(result.data)
                    this.todos = result.data
                },
                error => {
                    console.error(error)
                }
            )
        },
        addNewTask(){
             axios.post("/api/task", {task_name: this.taskname})
             .then((res) => {
                 this.taskname = ''
                 this.getTasks()
             }).catch((err) => {
                 console.log(err)
             })
        },
        editTask(title, id) {
            this.is = id
            this.taskname = title
            this.isEdit = true
        },
        updateTask(){
            axios.put(`/api/task/${this.id}`,
            {task_name:this.taskname})
            .then((res) => {
                this.taskname = ''
                this.isEdit = false
                this.getTasks()
                console.log(res)
            })
            .catch((err) => {
                console.log(err)
            })
        }
    }
}

</script>