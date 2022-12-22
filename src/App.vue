<template>
  <div id="app">
    <p style="text-align: center; font-size: 60px">
      <strong> <em> SteamChart by BlackPanther </em> </strong>
    </p>
    <steamChart></steamChart>
  </div>
</template>

<script>
import steamChart from "./components/steamChart.vue";

export default {
  name: "App",
  components: {
    steamChart,
  },
};
</script>
