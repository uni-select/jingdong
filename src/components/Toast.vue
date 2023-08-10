<template>
    <div class="toast">{{message}}</div>
</template>
<script>
export default {
  props: ['message']
}
</script>
<style lang="scss" scoped>
.toast{
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: .1rem;
    background: rgba(0, 0, 0, .35);
    border-radius: .05rem;
    color: #fff;
    border: 1px solid red;
}
</style>
