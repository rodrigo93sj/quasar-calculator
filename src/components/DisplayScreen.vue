<template>
  <div class="display-screen">
    {{ displayValue }}
  </div>
</template>

<script>

export default {
  name: 'DisplayScreen',
  props: {
    displayValue: String
  }
}
</script>

<style>
  .display-screen {
    display: flex;
    justify-content: flex-end;
    align-items: flex-end;
    font-size: 22px;
    flex: 1;
  }
</style>
