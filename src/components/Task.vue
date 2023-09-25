<template>
    <div @dblclick="$emit('toggle-reminder'), task.id" :class="[task.reminder ? ' reminder' : '', 'text']">
        <h3>{{ task.text }} 
            <i @click="$emit('delete-task', task.id)" class="fas fa-times"></i> <!-- since the data is presented in App.vue we have to move level to gather data so that it can implemenet changes -->
        </h3>
        <p>{{ task.day }} </p>
    </div>
</template>

<script>

    export default {
        name: 'Task',
        // writing task as an object because it hold the data in form of an array such that array is an object so task will also be considered object
        props: {
            task: Object,
        },
    }
</script>
