<template>
<div>
 

<div class="task-item">

<div class="checkbox"> 
        <input type="checkbox" v-model="completed" @change="doneEdit">
      </div>
      <div class="input-group mb-3">
      
      <div v-if="!editing" @click="editTask" class="task-item-label" :class="{completed : completed}">{{title}}</div>
      <input type="text" v-model="title" v-else class="task-edit form-control" @blur="doneEdit" @keyup.enter="doneEdit">
       <span class="input-group-btn"><button class="btn btn-danger pull-right" type="button" @click="removeTask(index)"> 
         <span class="glyphicon glyphicon-remove"></span></button></span>
    </div>




</div>
</div> 
</template>

<script>
export default{
data(){
return {
  'id': this.task.id,
  'title': this.task.title,
  'completed': this.task.completed,
  'editing': this.task.editing,
}
},
props: {
  task: {
    type: Object,
    required: true
  },
  index: {
    type: Number,
    required: true
  },
  checkAll:{
    type: Boolean,
    required: true
  },
  
},
watch: {
 checkAll(){
   if (this.checkAll){
     this.completed = true
   } else{
     this.completed = this.task.completed
   }
 }
},
methods:{
  removeTask(index){
  this.$emit('removedTask', index)
   },
   editTask(){
   this.editing = true
   },
   doneEdit(){
     this.editing = false
     this.$emit('finishedEdit', {
       "index": this.index,
       'task':{
         'id': this.id,
         'title': this.title,
         'completed': this.completed,
         'editing': this.editing,
       }
     })
   },
   
}
}
</script>

<style>
.extra-container{
  margin-left: 1.5em
}

/* .completed{
  text-decoration: line-through;
  color: gray
} */
</style>