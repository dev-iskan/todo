<template>
        <tr>
            <td>{{task.id}}</td>
            <td>{{task.title}}</td>
            <td>{{task.priority}}</td>
            <td><button class="btn btn-danger" @click="removeTask">Remove</button></td>
        </tr>
</template>

<script>
    export default{
        data(){
            return {

            }
        },
        methods: {
            removeTask() {
                this.$emit('delete', this.task.id);
            }
        },
        props: ['task']
    }
</script>

<style>

</style>