<template>
  <div class="result-screen">
    {{ resultValue }}
  </div>
</template>

<script>
export default {
  name: 'ResultScreen',
  props: {
    resultValue: String
  }
}
</script>

<style>
  .result-screen {
    display: flex;
    justify-content: flex-end;
    align-items: flex-end;
    font-size: 20px;
    flex: 1;
  }

</style>
