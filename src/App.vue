<template>
  <div>
    <h1>🎥 영화관 예매 시스템</h1>
    <button @click="current = 'builder'">Builder</button>
    <button @click="current = 'facade'">Facade</button>
    <button @click="current = 'strategy'">Strategy</button>

    <BuilderExample v-if="current === 'builder'" />
    <FacadeExample v-if="current === 'facade'" />
    <StrategyExample v-if="current === 'strategy'" />
  </div>
</template>

<script lang="ts">
import BuilderExample from "./components/BuilderExample.vue"
import FacadeExample from "./components/FacadeExample.vue"
import StrategyExample from "./components/StrategyExample.vue"

export default {
  components: { BuilderExample, FacadeExample, StrategyExample },
  data() {
    return { current: "builder" }
  },
}
</script>
