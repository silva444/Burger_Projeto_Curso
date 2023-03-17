<template>
  <div>
    <div class="main-container">
      <h1>gerenciar Pedidos</h1>
      <Dashboard />
    </div>
  </div>
</template>

<script>
import Dashboard from "../components/dashboard.vue";
export default {
  name: "pedidos",
  components: {
    Dashboard,
  },
};
</script>
