<template>
    <div class="app-component">
        <loading :active.sync="isLoading" :is-full-page="true"></loading>

        <table class="table">
            <thead>
            <tr>
                <th>Id</th>
                <th>Task title</th>
                <th>Priority level</th>
                <th>Actions</th>
            </tr>
            </thead>
            <tbody>
            <task-component v-for="task in tasks" :key="task.id" :task="task" @delete="deleteTask"></task-component>
            <tr>
               <td><input type="text" id="task" class="form-control" v-model="task.title"></td>
               <td><select name="" id="select" class="form-control" v-model="task.priority">
                   <option>Low</option>
                   <option>Medium</option>
                   <option>High</option>
               </select></td>
                <td><button @click="storeTask" class="btn btn-primary">Add</button></td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    import TaskComponent from './Task.vue';
    // Import component
    import Loading from 'vue-loading-overlay';
    // Import stylesheet
    import 'vue-loading-overlay/dist/vue-loading.min.css';


    export default {
        data() {
            return {
                tasks: [],
                task: {
                    title: '',
                    priority: ''
                },
                isLoading: false,
            }
        },
        components: {
            TaskComponent,
            Loading
        },
        methods: {
            getTasks() {
                this.isLoading = true;
                window.axios.get('/api/tasks')
                    .then(({data})=>{
                        data.forEach(task => {
                           this.tasks.push(task)
                        });
                        this.isLoading = false;
                    });
            },
            storeTask(){
                this.isLoading = true;  //initialize spinner
                if(this.checkInputs()){
                    window.axios.post('/api/tasks', this.task)
                        .then(savedTask=>{
                            this.tasks.push(savedTask.data);
                            this.task.title = '';
                            this.isLoading = false; // stop spinner
                        });
                }
            },

            checkInputs(){
                if(this.task.title && this.task.priority) return true
            },

            deleteTask(id){
                this.isLoading = true;
                // console.log(`I GOT THE ATA ${id}`);
                window.axios.delete(`/api/tasks/${id}`)
                    .then(()=>{
                        let index = this.tasks.findIndex(task=>task.id == id);
                        this.tasks.splice(index, 1);
                        this.isLoading = false;
                    })
            }
        },
        created(){
            this.getTasks();
        }
    }
</script>

<style>

</style>