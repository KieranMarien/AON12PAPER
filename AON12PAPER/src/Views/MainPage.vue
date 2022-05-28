<template>
    <div class="container">
      <h1>TODO LIST AON 12</h1>
      <div id="main">
        <SideBar v-if="this.live" :tasks="this.Tasks" @addTask="addTask" @showTask="showTask"></SideBar>
        <ViewTask :task="task"></ViewTask>
      </div>
    </div>

</template>

<script>

// import SideBar from '../components/SideBar.vue'
import ViewTask from '../components/ViewTask.vue'
import SideBar from "@/components/SideBar";

//        <SideBar v-if="this.live" :tasks="this.Tasks" @addTask="addTask" @showTask="showTask"></SideBar>

export default {
  name: "MainPage",

  components: {
    SideBar,
    // SideBar,
    ViewTask
  },
  data() {
    return {
      Tasks: [],
      task: [],
      live: false,
      subscribed: false,
      denied: false
    }
  },
  created() {
    this.getData();
    this.firstUpdate();
  },
  methods: {
    addTask: function (task) {
      localStorage.setItem(task.id, JSON.stringify(task));
      this.Tasks = {...localStorage};
      this.subscribe(task.deadline, task.title);
    },
    getData: function () {
      this.Tasks = {...localStorage};
      this.live = true;
    },
    showTask: function (task) {
      this.task = task;
    },
    async firstUpdate() {
      const reg = await navigator.serviceWorker.getRegistration();
      this.subscribed = !!(await reg?.pushManager.getSubscription());
    },
    async subscribe(deadline, task) {
      const notificationPermission = await Notification.requestPermission();

      if (notificationPermission === 'granted') {
        // eslint-disable-next-line no-unused-vars
        const publicKey = "BPtkPuu4t5bvQo_Iwnb3w2a_95a2u5Z8L0-hk3MiDVCSKTvn1crRvGxAVEDWKAd9QK7Yupy90XTffvhoL9TkJAI";

        // eslint-disable-next-line no-unused-vars
        const key = this.urlB64ToUint8Array(publicKey);


        navigator.serviceWorker.ready.then(function (reg) {
          reg?.pushManager.subscribe({
            userVisibleOnly: true,
            applicationServerKey: key,
          }).then(function (pushSubscription) {

            var subb = JSON.parse(JSON.stringify(pushSubscription));

            var sub = pushSubscription.endpoint + ",,,," + subb.keys.p256dh + ",,,," + subb.keys.auth + ",,,," + deadline + ",,,," + task;


        var myHeaders = new Headers();
        myHeaders.append("Authorization", "key=AAAAMDlYnKI:APA91bHulpZchl06WaJc-aW1NzBGf7qGX0wW6kHGpI2-AftyembwS1U8zHKEIQIKXtEtTyRMYTWjvJOgwLPjHgN8TeZWzSDeMIH0QXK9kuIL7rokRCk5MPXIgBIAXGfyaamjEtSBBsih");
        myHeaders.append("Content-Type", "text/plain");


        var requestOptions = {
          method: 'POST',
          headers: myHeaders,
          body: sub
        };

        fetch("http://localhost:8089/Subscribe/", requestOptions)
            .then(response => response.text())
            .then(result => console.log(result))
            .catch(error => console.log('error', error))

      })
        })
    }
    },
    urlB64ToUint8Array(base64String) {
      const padding = '='.repeat((4 - (base64String.length % 4)) % 4);
      // eslint-disable-next-line
      const base64 = (base64String + padding).replace(/\-/g, '+').replace(/_/g, '/');
      const rawData = window.atob(base64);
      const outputArray = new Uint8Array(rawData.length);
      for (let i = 0; i < rawData.length; ++i) {
        outputArray[i] = rawData.charCodeAt(i);
      }
      return outputArray;
    }
}
}

</script>

<style scoped>
body{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    background: #c9c8c8;
    height: 100vh;
    overflow: hidden;
}
.container{
    background: #393E46;
    border-top-right-radius: 10px;
    border-top-left-radius: 10px;
    height: 98vh;
}
.container h1 {
  color: #fff;
  padding: 10px;
  margin: 0;
}
#main{
  display: flex;
  width: 100%;
  height: 80vh;
  background-color: #10585b45;
}

.bell{
  background-color: dodgerblue;
}

</style>
