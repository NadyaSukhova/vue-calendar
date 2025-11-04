<script setup lang="ts">
import HelloWorld from './components/HelloWorld.vue'
import Calendar from './components/Calendar.vue'
</script>

<template>
  <div id="app">
    <header>
      <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

      <div class="wrapper">
        <HelloWorld msg="You did it!" />
      </div>
    </header>

    <main>
      <Calendar />
    </main>
  </div>
</template>
