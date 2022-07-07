<script setup>
import Login from "./components/Login.vue";
import Logout from "./components/Logout.vue";
import Chat from "./components/Chat.vue";
</script>

<template>
  <div
    v-if="!currentUser"
    class="bg-gray-50 flex h-screen justify-center items-center"
  >
    <Login @response="(user) => (currentUser = user)" :serverUrl="serverUrl" />
  </div>
  <div v-else class="max-h-screen overflow-hidden">
    <div class="flex items-baseline gap-4 max-h-screen overflow-hidden">
      <Logout
        class="absolute top-0 right-0 my-2 mx-3"
        @response="(user) => (currentUser = user)"
      />
    </div>
    <Chat :currentUser="currentUser" :serverUrl="serverUrl" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentUser: null,
      // serverUrl: "http://localhost:5000/",
      serverUrl: "https://nodevuerest.ericzheng28.repl.co/",
    };
  },
  methods: {
    async getUsername(UID) {
      console.log(UID);
      const response = await fetch(`${this.serverUrl}users/username`, {
        method: "POST",
        body: JSON.stringify({
          UID: UID,
        }),
        headers: new Headers({ "content-type": "application/json" }),
      });
      const responseObj = await response.json();
      console.log(responseObj);
      console.log(responseObj.Response);
      return responseObj.Response;
    },
    checkLocal() {
      if (localStorage.getItem("UID") && localStorage.getItem("Username")) {
        this.currentUser = {
          UID: localStorage.getItem("UID"),
          Username: localStorage.getItem("Username"),
        };
      }
    },
  },
  mounted() {
    this.checkLocal();
  },
};
</script>

<style scoped>
* {
  font-family: apple-system, Segoe UI, Roboto, Noto Sans, Ubuntu, Cantarell,
    Helvetica Neue;
}
</style>
