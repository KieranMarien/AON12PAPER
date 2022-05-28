<template>
    <div v-if="task.length > 0" class="ViewTask" id="showTask">
        <h2>Title: {{JSON.parse(task).title}}</h2>
        <div class="taskInfo">
            <h3>DeadLine:  {{JSON.parse(task).deadline }}</h3>
            <h3>Priority:  {{JSON.parse(task).priority }}</h3>
        </div>
        <p class="description">
          Description: {{JSON.parse(task).description }}
        </p>
        <div class="buttons">
          <button v-if="!JSON.parse(task).finished" @click="taskDone(JSON.parse(task))" class="doneBtn">Done</button>
          <button @click="removeTask(JSON.parse(task).id)" class="removeBtn">Remove</button>

        </div>
    </div>
</template>

<script>
export default {
  name: "ViewTask",
  props: ['task'],
  data(){
    return {
    }
  },
  methods: {
    taskDone: function (task){
      task.finished = true;
      localStorage.setItem(task.id, JSON.stringify(task));
      document.getElementById("Task"+task.id).className = "finished";
    },
    removeTask: function (id){
      document.getElementById("Task"+id).remove();
      document.getElementById("showTask").style.display = "none";
    },

  }
}
</script>

<style scoped>
.ViewTask{
    display: flex;
    flex-direction: column;
    width: 70%;
    background: #40a67dcf;
    padding: 25px;
}
.taskInfo{
    display: flex;
    justify-content: space-between;
}
.removeBtn{
    padding: 10px;
    width: 150px;
    border: none;
    cursor: pointer;
    background-color: rgb(189, 70, 70);
    margin: auto;
    border-radius: 10px;
    color: #fff;
}
.removeBtn:hover,
.doneBtn:hover{
    opacity: .8;
}
.description {
  text-align: left;
}
.buttons {
    width: 100%;
    justify-content: space-between;
    display: flex;
    margin-top: auto;
}
.doneBtn{
  padding: 10px;
    width: 150px;
    border: none;
    cursor: pointer;
    background-color: rgb(0 82 0);
    margin: auto;
    color: #fff;
    border-radius: 10px;
}
</style>
