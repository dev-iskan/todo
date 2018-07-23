<template>
    <div class="app-component">
        <table class="table">
            <thead>
            <tr>
                <th>Task title</th>
                <th>Priority level</th>
                <th>Actions</th>
            </tr>
            </thead>
            <tbody>
            <task-component v-for="task in tasks" :key="task.id" :task="task"></task-component>
            <tr>
               <td><input type="text" id="task" class="form-control" ></td>
               <td><select name="" id="select" class="form-control">
                   <option value="">Low</option>
                   <option value="">Medium</option>
                   <option value="">High</option>
               </select></td>
                <td><button class="btn btn-primary">Add</button></td>
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
                tasks: [
                    {
                        id: 1,
                        title: 'Task1',
                        priority: 'low'
                    },
                    {
                        id: 2,
                        title: 'Task2',
                        priority: 'low'
                    },
                    {
                        id: 3,
                        title: 'Task3',
                        priority: 'high'
                    }
                ]
            }
        },
        components: {
            TaskComponent
        }
    }
</script>

<style>

</style>