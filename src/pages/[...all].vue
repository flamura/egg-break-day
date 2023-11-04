<script setup>
function backMain() {
  location.href = '/'
}
</script>

<template>
  <div>
    你来到了还没有建好的房间哦
  </div>
  <div>
    要不<a @click.prevent="backMain">回头看看</a>吧
  </div>
</template>

<style scoped lang="scss">
a {
  color: #4a98e6;
  text-decoration: underline;

  &:hover {
    text-decoration: none;
  }
}
</style>
