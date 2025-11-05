<template>
  <div id="app">
    <header>
      <Header :lang="lang" :toggleLang="toggleLang" />
    </header>
    <main>
      <Calendar :lang="lang" />
    </main>
  </div>
</template>

<script>
import Calendar from './components/Calendar.vue'
import Header from './components/Header.vue';

export default {
  name: 'App',
  components: {
    Calendar,
    Header
  },
  data() {
    const today = new Date();
    return {
      lang: "Rus",
    }
  },
  methods: {
    toggleLang() {
      if (this.lang == "Rus") {
        this.lang = "Eng";
      }
      else {
        this.lang = "Rus";
      }
    }
  }
}
</script>