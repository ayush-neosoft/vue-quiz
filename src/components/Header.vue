<template>
  <div>
    <b-nav>
      <b-nav-item disabled><b>Vue Quiz App</b></b-nav-item>
      <b-nav-item disabled>Correct: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: [
    'numCorrect',
    'numTotal'
  ]
}
</script>