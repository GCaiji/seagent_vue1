<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import Chat from './components/Chat.vue'
</script>

<template>
  <!-- <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header> -->

  <main>
    <div class="wrap">
      <Chat />

    </div>

  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.wrap {
  position: relative;
  width: 1280px;
  height: 600px;
  border: 1px solid gray;
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
