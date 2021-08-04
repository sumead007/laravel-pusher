<template>
  <div>
    <div>username: <input v-model="userAcc" /></div>
    <div>
      input: <input v-model="inputText" @keyup.enter="SentMessage()" /><button
        @click="SentMessage()"
      >
        click
      </button>
    </div>
    <div style="margin-top: 10px">
      <div>ข้อความทั้งหมด:</div>
      <div v-for="(item, index) in message" :key="index">
        <div>
          {{ userAcc == item.username ? "Me" : item.username }} >>
          {{ item.message }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Pusher from "pusher-js";
import Axios from "axios";

export default {
  data() {
    return {
      userAcc: "user1",
      inputText: null,
      pusher: null,
      channel: null,
      message: [],
    };
  },
  methods: {
    SentMessage() {
      //   return console.log(this.inputText);
      Axios.get("/sendMsg", {
        params: {
          username: this.userAcc,
          msg: this.inputText,
        },
      }).then((response) => {
        this.inputText = null;
      });
    },
  },
  created() {
    this.pusher = new Pusher("3a3e72f34a4be85dd6e2", {
      cluster: "ap1",
    });
    this.channel = this.pusher.subscribe("monsterlove_ch");
    this.channel.bind("monsterlove_resp", (data) => {
      this.message.push(data.message);
    });
  },
};
</script>
