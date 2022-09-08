<template>
  <main id="Home-page">
    <div class="container name">
      <h3>Plataforma Cadastro Clientes</h3>
    </div>
    <div class="container">

      <DataTableComponent/>
    </div>
  </main>

</template>
<script>
import DataTableComponent from "@/components/DataTableComponent";

export default {
  components: {DataTableComponent}
}
</script>
<style scoped>
.name {
  text-align: center;
}
</style>