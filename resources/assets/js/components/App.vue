<template>
    <div class="app-component">
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
    export default {
        data() {
            return {
                tasks: [],
                task: {
                    title: '',
                    priority: ''
                }
            }
        },
        components: {
            TaskComponent
        },
        methods: {
            getTasks() {
                window.axios.get('/api/tasks')
                    .then(({data})=>{
                        data.forEach(task => {
                           this.tasks.push(task)
                        });
                    });
            },
            storeTask(){
                if(this.checkInputs()){
                    window.axios.post('/api/tasks', this.task)
                        .then(savedTask=>{
                            this.tasks.push(savedTask.data);
                            this.task.title = '';
                        });
                }
            },

            checkInputs(){
                if(this.task.title && this.task.priority) return true
            },

            deleteTask(id){
                // console.log(`I GOT THE ATA ${id}`);
                window.axios.delete(`/api/tasks/${id}`)
                    .then(()=>{
                        let index = this.tasks.findIndex(task=>task.id == id);
                        this.tasks.splice(index, 1);
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