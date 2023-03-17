<template>
  <div>
  <Banner/>
    <div class="main-container">
      <h1>monte o seu burguer</h1>
        <Burgerform/>
    </div>
  </div>
</template>


<script>
import Burgerform from "../components/burgerform.vue"
import Banner from "../components/Banner.vue"
// @ is an alias to /src

export default {
  name: "HomeView",
  components:{
    Banner,Burgerform
  }
};
</script>
