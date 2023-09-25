<!-- Plase run backend on one terminal and frontend on other terminal for full functionality :P -->


<!-- This is a global File  -->
<template>
  <div class="container">
    <!-- Accessing Components -->
      <Header @toggle-add-task="toggleAddTask"  title='Task Tracker' />
      <!-- vue if else  -->
      <div v-show="showAddTask" :class="[showAddTask ? 'open': '']">
        <AddTask @add-task="addTask" />
      </div>
      <!-- creating methods for the functionality of the tasks  -->
      <Tasks @toggle-reminder="toggleReminder" @delete-task = 'deleteTask' :tasks = "tasks" />
      <Footer />
  </div>
</template>

<script>
import Header from "./components/Header.vue"
import Footer from "./components/footer.vue"
import Tasks from "./components/Tasks.vue"
import AddTask from './components/Add_Task.vue'
export default {
    name : "App",
    // defining components of the file
    components:{ 
      // importing Header functionality from the Header.vue
      Header,
      Tasks,
      AddTask,
      Footer,
    },
    //it holds the components data 
    data (){
        return {
          tasks: [],
          showAddTask: false

        }
      },
      methods:{
        toggleAddTask(){
          this.showAddTask = !this.showAddTask
        },
        async addTask(task){
          const res = await fetch("http://localhost:5000/tasks", {
            method: 'POST',
            headers: {
              'Content-type': 'application/json',
            },
            body: JSON.stringify(task),
          })
          const data = await res.json()
          this.tasks = [...this.tasks, data]
        },

        // adding data to back end
        // addTask(task) {
        //   this.tasks = [...this.tasks, task]
        // },

        // delete task with respect to ID 
        // deleteTask(id) {
        //   this.tasks = this.tasks.filter((task) => task.id !== id);
        // },

        // Deleting data from the backEnd

        async deleteTask(id){
            const res = await fetch(`http://localhost:5000/tasks/${id}`, {
              method: 'DELETE'
            })
            if (res.status === 200) {
              this.tasks = this.tasks.filter((task) => task.id !== id)
            }
            else {
              alert("Error deleting ")
            }
        },

        // changes the active status with respect to id of the task on db click if true then becomes false if false then becomes true
        async toggleReminder(id){
          // if id equals to the task.id then active status will be reversed else nothing will happen
          // this.tasks = this.tasks.map((task) => task.id === id ? {...task, reminder: !task.reminder} : task)
          
          // removing from backend 
          const tasktoToggle = await this.fetchTask(id)
          const updateTask = {...tasktoToggle, reminder : !tasktoToggle.reminder}

          const res = await fetch(`http://localhost:5000/tasks/${id}`, {
            method: 'PUT',
            headers:{
              'Content-type' : 'application/json',
            },

            body: JSON.stringify(updateTask),
          })
          const data = await res.json()

          this.tasks = this.tasks.map((task) => task.id === id ? {...task, reminder: data.reminder} : task)
       
        },
        async fetchTask(id){
          const res = await fetch(`http://localhost:5000/tasks/${id}`)
          const data = await res.json()
          return data
        },
        async fetchtasks(){
          const res = await fetch(`http://localhost:5000/tasks`)
          const data = await res.json()
          return data
        },

    },
    // store/create data need to be accessed for the program
    async created(){
    // for backend retrieving of data 
      this.tasks = await this.fetchtasks()
    //   this.tasks = [{
    //     id : 1,
    //     text : 'Doctors Appointment',
    //     day : 'March 1st at 2:30pm',
    //     reminder: true,
    //   },
    //   {
    //     id : 2,
    //     text : 'Meeting at School',
    //     day : 'March 3rd at 1:30 pm',
    //     reminder: true,           
    //   },
    //   {
    //     id : 3,
    //     text : 'Food Shopping',
    //     day : 'March 3rd at 11:00 am',
    //     reminder: false,
    //   }
    // ]

  }

}
</script>
<style >
  * {
    box-sizing: border-box;
    padding: 0;
    margin: 0;
  }
  body{
    background-color:	#e7e6e6 ;
  }
  .container{
    max-width: 500px;
    margin: 30px auto;
    overflow: auto;
    min-height: 300px;
    border:1px solid gray;
    padding: 30px;
    border-radius: 5px;
    background-color: white;
  }
  .btn{
    padding:15px;
    background-color: black;
    color:white;
    border-top-left-radius:10px;
    border-bottom-right-radius: 10px;
  }
  .btn:hover{
    border-top-left-radius: 0px;
    border-bottom-right-radius: 0px;
    border-top-right-radius: 10px;
    border-bottom-left-radius: 10px;
    background-color: black;
    transition: all .3s ease;
  }

  .text{
    background-color: #f4f4f4;
    margin:5px;
    padding: 10px 20px;
    cursor:pointer;
  }

  .text.reminder{
    border-left : 5px solid green;
  }

  .text h3{
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .text:hover{
    background-color: #cecbcb;
    border-radius: 5px;
    transition: all .3s ease;
  }
  .text:hover .fas{
    padding-top:5px;
    font-size: 25px;
    transition: all .5s ease;
    color:rgb(192, 29, 29);
  }
  .text:hover .fas:hover{
    color:red
  }
  .add-form{
    margin-bottom: 40px;
  }
  .form-control{
    margin:20px 0;
  }
  .form-control label{
    display: block;
  }
  .form-control input{
    width:100%;
    height: 40px;
    margin: 5px;
    padding: 3px 7px;
    font-size: 17px;
    border-radius:5px;
  }
  .form-control-check{
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .form-control-check label{
    flex: 1;
  }
  .form-control-check input {
    flex:2;
    height: 20px;
  }
  .btn-block{
    width:100%;
  }
  .btn-block:active{
    background-color: rgb(41, 41, 41);
  }
  .open{
    transition: all .3s ease;
  }

</style>