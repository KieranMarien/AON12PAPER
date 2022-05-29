<template>
  <div class="SideBar">
      <button class="addNewTast" @click="showModal">+</button>
    <div class="allTasks">
      <h4 :id="'Task'+JSON.parse(item).id" @click="showTaskInfo(item)" v-for="item in tasks" :key="item" :class="selectClassName(item)" >{{ JSON.parse(item).title }}</h4>
    </div>


    <dialog class="modal" id="modal">
        <form>
            <h2>Add new task</h2>
            <label for="inputTitle">Title: </label>
            <input type="text" name="title" id="inputTitle" placeholder="Title" required maxlength="50">

            <label for="inputTitle">Description: </label>
            <input type="text" name="description" id="inputDescription" placeholder="Description">

            <label for="inputDeadline">Deadline: </label>
            <input type="datetime-local" name="deadLine" id="inputDeadline" required>

          <label for="priority">Priority:</label>
          <select id="priority" name="priority">
            <option value="High">High</option>
            <option selected value="Medium">Medium</option>
            <option value="Low">Low</option>
          </select>

            <div class="btns">
                <button v-on:click.prevent="addNewTask()" class="addTask">Submit</button>
                <button v-on:click.prevent="CloseDialog()" class="remove">Close</button>
            </div>
        </form>
  </dialog>
  </div>

</template>

<script>
export default {
  name: "SideBar",
  props: ['tasks'],
  data(){
    return {
    }
  },
  methods: {
    showModal: function() {
      var modal = document.querySelector("#modal");
      modal.show();
    },
    addNewTask: function (){
      let task = {
        id: Math.random().toString(36).slice(2),
        title: document.getElementById("inputTitle").value,
        description: document.getElementById("inputDescription").value,
        deadline: document.getElementById("inputDeadline").value,
        priority: document.getElementById("priority").value,
        finished: false
      }
      this.$emit('addTask', task);
      this.clearInputs();
      this.CloseDialog();
    },
    showTaskInfo: function (task){
      this.$emit('showTask', task);
      if (document.getElementById("showTask") != null) {
      document.getElementById("showTask").style.display = "flex";
      }
    },
    CloseDialog: function(){
        const modal = document.getElementById("modal");
        modal.close();
    },
    clearInputs: function (){
      document.getElementById("inputTitle").value = "";
      document.getElementById("inputDescription").value = "";
    },
    selectClassName: function(task){
      if(JSON.parse(task).finished){
        return "finished";
      }
      return "unfinished";
    }
  }
}



</script>

<style scoped>

.modal {
  top: 150px;
}
.SideBar {
    width: 30vw;
    background: #222831;
}
.addNewTast {
    width: 100%;
    background-color: #59b259;
    border: none;
    cursor: pointer;
    font-size: 46px;
    font-weight: bold;
}
.addNewTast:hover {
    opacity: .8;
}

.modal form{
    display: flex;
    flex-direction: column;
    width: 400px;
    text-align: left;

}
.btns {
    display: flex;
    justify-content: space-between;
    padding: 15px;
}
.btns button{
    padding: 10px;
    width: 150px;
    border: none;
    cursor: pointer;
}
.btns button:hover{
    opacity: .8;
}
.btns button:first-child{
    background-color: #59b259;
}
.btns button:last-child{
    background-color: rgb(184, 61, 61);
}
.modal form h2{
    text-align: center;
}
.modal form input{
    padding: 10px;
    margin: 15px;
}

.modal form select{
  padding: 10px;
  margin: 15px;
}

h4 {
  cursor: pointer;
  background-color: rgba(255, 255, 255, 0.52);
  width: 90%;
  padding: 10px 0;
  opacity: .8;
}
.finished{
  border-left: #59b259 10px solid;
}
.unfinished{
  border-left: rgb(184, 61, 61) 10px solid;
}

h4:hover{
  opacity: 1;
}
.allTasks{
  overflow: auto;
  height: 70vh;
}

</style>
