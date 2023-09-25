<template>
    <!-- looping through data of the task -->
    <!-- vue for  -->
    <div v-for="task in tasks" >
       <!-- <h3>{{ task.text }}</h3> -->
       <!-- since the data is presented in App.vue we have to move level to gather data so that it can implemenet changes -->
       <Task @toggle-reminder="$emit('toggle-reminder', task.id)" @delete-task="$emit('delete-task',task.id)" :task = task />
    </div>
</template>

<script>
    import Task from './Task.vue'

    export default {
        name: 'Tasks',
        // defining tasks such that it is an Array 
        props: {
            tasks : Array,
        },
        components: {
            Task,
        },
        emits: ["delete-task" , 'toggle-reminder']
    }
</script>