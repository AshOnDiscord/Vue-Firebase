<script>
export default {
  emits: ["response"],
  methods: {
    logout() {
      this.$emit("response", null);
      localStorage.removeItem("UID");
      localStorage.removeItem("Username");
    },
  },
};
</script>

<template>
  <button
    type="button"
    @click="logout"
    class="inline-flex items-start px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none active:bg-indigo-800"
  >
    Logout
  </button>
</template>
