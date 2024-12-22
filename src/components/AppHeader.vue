<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>Fancy Quiz App</b-nav-item>
      <b-nav-item disabled
        >Counter: {{ numOfCorrectAnswers }}/{{ numOfTotalAnswers }}</b-nav-item
      >
    </b-nav>
  </div>
</template>

<script>
export default {
  name: "AppHeader",
  props: ["numOfCorrectAnswers", "numOfTotalAnswers"],
};
</script>
