<template>
  <div>
    <div class="container">
    <h1>Tasks({{itemsLeft}})</h1>

<div class="container">
<div class="row">
<div class="col-sm-12">
<div class="input-group mb-3">
  <input type="text" class="form-control" placeholder="New task"
  v-model="newTask">
  <span class="input-group-btn">
   <button class="btn btn-primary" type="button" id="button-addon2" @click="addTask"><span class="glyphicon glyphicon-plus"></span> Add</button>
  </span>
</div>
</div>
</div>
</div>

<div class="container button-group-clear">
<div class="row">
<div class="col-sm-12">
  <div class="tasks__clear button-group pull-right">
    <button type="button" class="btn btn-warning" @click="clearCompleted"><span class="glyphicon glyphicon-ok"></span> Clear completed</button>
    
</div>
</div>
</div>
</div>

<div class="extra-container">
  <div><label><input type="checkbox" @change="checkAllTasks" :checked="!anyRemaining"> Check all</label></div>
</div>
<hr>
<div class="col-md-12">
    <app-task-item v-for="(task, index) in allTasksFiltered" :key="task.id" :task="task" :index="index" @removedTask="removeTask" 
    @finishedEdit="finishedEdit" :checkAll="!anyRemaining">
    </app-task-item>
</div>

<div class="filter-container">
<div>
  <button type="button" class="btn btn-success" :class="{active : filter == 'all'}" @click = "filter = 'all'">All</button>
  <button type="button" class="btn btn-success" :class="{active: filter == 'active'}" @click = "filter = 'active'">Active</button>
  <button type="button" class="btn btn-success" :class="{active: filter == 'completed'}" @click = "filter = 'completed'">Completed</button>
</div>
</div>



</div>
</div>
  
</template>

<script>
import taskItem from '../src/task-item';
export default {
 
  data () {
    return {
     newTask: '',
     idForTask: 4,
     filter: 'all',
     allTasks: [
       {
        id: 1,
       title: ' finish first task',
       completed: false,
       editing: false
       },
        {
        id: 2,
       title: ' finish second task',
       completed: false,
       editing: false
       }
     ]
    }
  },
  components: {
   'app-task-item': taskItem
  },
  computed:{
   itemsLeft(){
     return this.allTasks.filter(task => !task.completed).length
   },
   anyRemaining(){
     return this.itemsLeft != 0
   },
   allTasksFiltered(){
     if(this.filter == 'all'){
       return this.allTasks
     } else if (this.filter == 'active'){
       return this.allTasks.filter(task => !task.completed)
     } else if (this.filter == 'completed'){
       return this.allTasks.filter(task => task.completed)
     }
     return this.allTasks
   }
  },
 methods:{
   addTask(){
     if(this.newTask.trim().length == 0){
       return
     }
     this.allTasks.push(
       { id: this.idForTask,
         title: this.newTask,
         completed: false
       })
       this.newTask = ""
       this.idForTask ++
   },
   removeTask(index){
     this.allTasks.splice(index,1)
   },
  finishedEdit(data){
  this.allTasks.splice(data.index, 1, data.task)
  },
   checkAllTasks(){
       this.allTasks.forEach((task) => task.completed = event.target.checked)
   },
   clearCompleted(){
     this.allTasks = this.allTasks.filter(task => !task.completed)
   },
 
}
}
</script>

<style>
.task-item{
  margin-bottom: 12px;
  margin-left: 1.8em;
  width: 100%;
  align-items: center;
  justify-content: space-between;
  display: inline-block;
 
}

.task-edit{
  margin-bottom: 3px;
  margin-left: 1px;
  width: 100%;
  padding: 6px;
  align-items: center;
  justify-content: space-between;
  border: 1px solid lightgray;
  border-radius: 5px;
   
}

.task-item-label{
  margin-bottom: 3px;
  margin-left: 1px;
  width: 100%;
  padding: 6px;
  align-items: center;
  justify-content: space-between;
  border: 1px solid lightgray;
  border-radius: 5px;
  
}

.completed{
  text-decoration: line-through;
  color: gray
}


.button-group-clear{
  margin-top: 1em
}

.extra-container{
  margin-left: 1.5em
}

.filter-container{
  margin-left: 3em;
 
}

</style>