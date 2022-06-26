<script setup></script>

<template>
  Webs
  <form @submit.prevent="signIn" v-if="!currentUser">
    <input v-model="userSign" placeholder="Username" type="text" />
    <input v-model="passSign" placeholder="Password" type="text" />
    <button type="submit">Sign In</button>
  </form>
  <br />
  {{ responseObj || "No Object Yet" }}
  <br />
  {{ response || "Nothing" }}
</template>

<script>
export default {
  data() {
    return {
      signedIn: false,
      userSign: "",
      passSign: "",
      currentUser: null,
      responseObj: {},
      response: "",
      serverUrl: "http://localhost:5000/",
    };
  },
  methods: {
    async signIn() {
      const response = await fetch(`${this.serverUrl}users/login`, {
        method: "POST",
        body: JSON.stringify({
          Username: this.userSign,
          Password: this.passSign,
        }),
        headers: new Headers({ "content-type": "application/json" }),
      });
      //console.log(await response.json());
      this.responseObj = await response.json();
      this.response = `${this.responseObj.loggedIn} | ${
        this.responseObj.error || this.responseObj.UID
      }`;
    },
  },
};
</script>

<style>
input {
  @apply border border-slate-300 rounded-md;
}
</style>
