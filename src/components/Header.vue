<template>
    <header>
        <!-- consider title as a variable and its value if Task Tracker if nothing is passed in the App.vue Header component 
        then it will display the default message  -->
        <h1>{{title }}</h1>

        <Button @toggle-add-task="$emit('toggle-add-task')" text="Add Task" color="rgb(56, 55, 55)"/>
    </header>
</template>

<script>
    import Button from './button.vue'
    export default{
        name : "Header",
        props: {
            title:{
                type : String,
                // if nothing passes in App.vue default value will be
                default: "Nothing Exist as Title"
            },
        },
        components : {
            Button,
        }
    }
</script>

<style scoped>
    header{
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>