<template>
  Chat
  <button
    @click="update"
    class="inline-flex items-start px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
  >
    Update
  </button>
  <ul class="mt-4">
    <li v-for="message in messages">
      <h1>{{ message.Author }}</h1>
      <p>{{ message.Message }}</p>
      <!-- <p>{{ message.Date }} | {{ message.UID }}</p> -->
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      selectedChannel: "channel-1",
      messages: [],
    };
  },
  props: {
    serverUrl: String,
    currentUser: Object,
  },
  methods: {
    async getMessages(selectedChannel) {
      const response = await fetch(`${this.serverUrl}users/messages`, {
        method: "POST",
        body: JSON.stringify({
          Username: this.currentUser,
          Channel: selectedChannel,
        }),
        headers: new Headers({ "content-type": "application/json" }),
      });
      const messages = await response.json();
      this.messages = messages.Messages;
    },
    update() {
      this.getMessages(this.selectedChannel);
    },
  },
  mounted: function () {
    this.update();
  },
};
</script>

<style>
ul {
  @apply flex flex-col gap-4 items-start;
}

li {
  @apply bg-slate-200 p-2 rounded-md;
}

li h1 {
  @apply text-indigo-700;
}

li {
}
</style>
