<template>
  <div>
    <Header />
    <Nuxt />
    <Footer />
  </div>
</template>

<script>
import Header from '../components/Header';
import Footer from '../components/Footer';

export default {
  components: {
    Header,
    Footer
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@600;800&family=Raleway:ital,wght@0,400;0,500;1,400&display=swap');
html {
  font-size: 100%;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}
.container {
  max-width: 1366px;
  margin: 0 auto;
  padding: 0 10px;
}
</style>
