<template>
  <div class="sidebar">
    <div class="newchat">
      <button @click="newChat">New Chat</button>
    </div>
  </div>
</template>

<script>
export default {
  methods: {

  }
}
</script>

<style scoped>
.sidebar {
  position: absolute;
  bottom: 0px;
  left: 0px;
  border: 1px solid #ccc;
  border-left: 0px;
  border-bottom: 0px;
  border-right: 0px;
  width: 18%;
  height: 86%;
}

.newchat {
  display: flex;
  width: 100%;
  height: 66px;
  justify-content: center;
  align-items: center;
}

.newchat button {
  width: 90%;
  height: 70%;
  border: 1px solid #ccc;
  background-color: white;
  border-radius: 7px;
  cursor: pointer;
}

.newchat button:hover {
  background-color: #f1f1f1
}
</style>