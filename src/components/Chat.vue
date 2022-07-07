<template>
  <div ref="top" class="bg-slate-100 px-5 py-4 text-slate-800">
    {{ selectedChannel }}
  </div>

  <div
    class="overflow-scroll h-[calc(100vh-6.75rem)] bg-slate-400 w-screen overflow-x-hidden"
  >
    <ul
      ref="messages"
      class="py-4 px-4 overflow-scroll max-h-full w-screen overflow-x-hidden"
    >
      <li
        v-for="message in messages"
        :class="{ self: message.UID == currentUser.UID }"
      >
        <h1>{{ message.Author }}</h1>
        <p class="text-slate-800">{{ message.Message }}</p>
        <!-- <p>{{ message.Date }} | {{ message.UID }}</p> -->
      </li>
    </ul>
  </div>
  <div
    ref="bottom"
    class="grid grid-cols-[1fr,auto] bg-slate-200 mt-4 px-4 py-2 absolute bottom-0 w-full"
  >
    <form class="flex" @submit.prevent="sendMessage">
      <input
        class="w-full h-full px-4 outline-none text-slate-800 rounded-l-md text-sm"
        type="text"
        v-model="newMessage"
        :placeholder="`Message ${selectedChannel}`"
      />
      <button
        class="inline-flex items-start pl-4 py-2 pr-[calc(1rem+1px)] border border-transparent text-sm font-medium shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none active:bg-indigo-800"
      >
        Send
      </button>
    </form>
    <button
      @click="getMessages"
      class="inline-flex items-start px-4 py-2 border border-transparent text-sm font-medium rounded-r-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none active:bg-indigo-800"
    >
      Update
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedChannel: "channel-1",
      messages: [],
      newMessage: "",
      updateLoop: null,
    };
  },
  props: {
    serverUrl: String,
    currentUser: Object,
  },
  methods: {
    async getMessages() {
      console.log("get");
      const response = await fetch(`${this.serverUrl}users/messages`, {
        method: "POST",
        body: JSON.stringify({
          User: this.currentUser,
          Channel: this.selectedChannel,
        }),
        headers: new Headers({ "content-type": "application/json" }),
      });
      const messages = await response.json();
      this.messages = messages.Messages;
    },
    async sendMessage() {
      const newMessage = this.newMessage;
      console.log("send");
      const response = await fetch(`${this.serverUrl}users/send`, {
        method: "POST",
        body: JSON.stringify({
          User: this.currentUser.Username,
          UID: this.currentUser.UID,
          Channel: this.selectedChannel,
          Message: newMessage,
        }),
        headers: new Headers({ "content-type": "application/json" }),
      });

      this.getMessages();
      this.newMessage = "";
    },
    startUpdateLoop() {
      if (this.updateLoop) return;
      this.updateLoop = setInterval(() => {
        this.getMessages();
      }, 1000);
    },
  },
  mounted: function () {
    this.getMessages();
    setTimeout(() => {
      this.$refs.messages.scrollTop = 9007199259;
      console.log(this.$refs.messages.scrollTop);
    }, 1000);
    this.startUpdateLoop();
  },
  unmounted: function () {
    clearInterval(this.updateLoop);
  },
};
</script>

<style>
ul {
  @apply flex flex-col gap-4 items-start;
}

li {
  @apply bg-slate-100 p-2 rounded-md;
}

li h1 {
  @apply text-indigo-700;
}

.self {
  @apply bg-slate-200;
}
</style>
