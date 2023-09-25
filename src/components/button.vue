<template>
    <!-- added fuction such that to provide functionality to the button -->
    <button @click="onClick()" :style="{ background:color }" class="btn">
        {{text}} 
    </button>
</template>

<script>
    export default{
        name:'Button',
        // for example take Props as a Variable
        props:{
            text: String,
            color: String
        },
        // Methods are function that define the functionality of the code
         methods:{
            onClick(){
                this.$emit('toggle-add-task')

            }
        }
    }
</script>
