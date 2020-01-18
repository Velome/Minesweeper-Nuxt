<template>
  <section class="section">
    <div class="columns is-mobile">
      <minesweeper></minesweeper>
    </div>
    <br /><br /><br />
    <div class="columns is-mobile">
      <minesweeper difficulty="Intermediate"></minesweeper>
    </div>
    <br /><br /><br />
    <div class="columns is-mobile">
      <minesweeper difficulty="Expert"></minesweeper>
    </div>
  </section>
</template>

<script>
  import Minesweeper from "~/components/Minesweeper";

  export default {
    name: "Minesweeper-Nuxt",

    components: {
      Minesweeper
    }
  };
</script>