<template>
  <div class="container" >
    <h2 class="text-center mt-5">Task Management App</h2>
    <!-- Input -->
    <div class="d-flex">
      <!-- v-model = create a two-way binding on a form input element or a component.-->
      <input v-model="task" type="text" placeholder="Enter Task" class="form-control">
    <button @click="submitTask" class="btn btn-warning rounded-0">SUBMIT</button>
    </div>
    <!-- Task Table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="center">#</th>
          <th scope="col" class="center">#</th>
        </tr>
      </thead>
      <tbody>
        <!-- v-for (directive to render a list of items based on array)  -->
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{'finished': task.status === 'finished'}">{{task.name}}</span>
          </td> <!-- read data based on obj -->
          <td style="width: 120px">
            <span @click="changeStatus(index)" class="pointer"
            :class="{'text-danger': task.status === 'to-do',
            'text-warning': task.status === 'in-progress'}">
            {{firstCharUpper(task.status)}}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data(){
    return{
      task : 'what you want to do today?',
      editedTask : null,
      availableStatuses: ['to-do', 'in-progress', 'finished'],

      tasks: [
        {
          name: 'wash clothes',
          status: 'in-progress'
        },
        {
          name: 'read a book',
          status: 'to-do'
        },

          {
          name: 'cook dumpling',
          status: 'to-do'
        },
      ]
    }},

      methods: {
        submitTask(){
      if(this.task.length == 0) return;
      if(this.editedTask === null){
        this.tasks.push({
        name: this.task,
        status: 'todo'
      });
      } else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = '';
        },
        deleteTask(index){
          this.tasks.splice(index, 1); //overwrites original array / method add or removes array
        },
        editTask(index){
          this.task = this.tasks[index].name;
          this.editedTask = index;
        },

        changeStatus(index){
        let newIndex =  this.availableStatuses.indexOf(this.tasks[index].status);
        if(++newIndex >2) newIndex = 0;
        this.tasks[index].status = this.availableStatuses[newIndex];
        },
        firstCharUpper(str){
          return str.charAt(0).toUpperCase() + str.slice(1);
        }
      }

    };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer {
  cursor:grab;
}

.finished{
  text-decoration: line-through;
}
</style>
