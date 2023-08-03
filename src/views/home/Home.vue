<template>
    <div class="wrapper">
        <StaticPart />
        <Nearby />
    </div>
    <Docker />
</template>
<script>
import StaticPart from './StaticPart'
import Nearby from './Nearby'
import Docker from './Docker'
export default {
  name: 'Home',
  components: { StaticPart, Nearby, Docker }
}
</script>

<style lang="scss" scoped>
.wrapper {
  overflow: auto;
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0.5rem;
  padding: 0 .18rem .1rem .18rem;
}
</style>
