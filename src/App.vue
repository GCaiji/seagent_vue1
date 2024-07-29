<script setup>
import Chat from './components/Chat.vue'
import UserInfo from './components/UserInfo.vue'
import ChatHistory from './components/ChatHistory.vue'
</script>

<template>
  <main>
    <div class="wrap">
      <UserInfo />
      <ChatHistory />
    </div>

  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.wrap {
  position: absolute;
  top: 0;
  right: 0;
  width: 95%;
  height: 95%;
  margin: 20px 30px auto auto;
  border: 1px solid rgb(218, 218, 218);
  border-radius: 10px;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
