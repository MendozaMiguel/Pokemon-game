<template>
  <div class="ExoTest">
    <p>TEST</p>
    <ExoTestComponent msg="Welcome to Exo Test Component" />
  </div>
</template>
<script>
import ExoTestComponent from '@/components/ExoTestComponent.vue'
export default {
  name: 'ExoTest',
  components: {
    ExoTestComponent
  }
}
</script>
