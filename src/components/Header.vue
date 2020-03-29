<template>
  <div>
    <b-nav tabs align="center">     
      <b-nav-text><b>20 question Quiz App </b></b-nav-text>
      <b-nav-item disabled>Correct answers: {{ correctNum }}/{{ totalNum }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: [
    'correctNum',
    'totalNum'
  ]
}
</script>