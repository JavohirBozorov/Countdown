<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <Countdown
      :year="2022"
      :month="9"
      :date="30"
      :hour="20"
      :minute="59"
      :second="59"
      :millisecond="5"
    />
    <!-- <Countdown /> -->
  </div>
</template>

<script>
import Countdown from "@/components/Countdown.vue";

export default {
  name: "HomeView",
  components: {
    Countdown,
  },
};
</script>
