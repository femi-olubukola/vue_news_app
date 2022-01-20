<template>

  <div>
    <Header/>
    <main class="container flex-grow px-4 max-auto my-12 px-10">
      <slot/>
    </main>
    <Footer/>
  </div>
</template>

<script>
import Header from "@/components/Header";
import Footer from "@/components/Footer";
export default {
  name:"Layout",
  components: {Header,
              Footer}

}
</script>

<style scoped>

</style>