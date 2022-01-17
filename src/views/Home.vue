<template>
    <notifications
        class="mt-3 ms-3"
        :duration="2000"
        :width="250"
        animation-name="v-fade-left"
        position="top left"
    />
    <div class="container my-5">
        <appHeader></appHeader>
    </div>
</template>

<script>

import appHeader from '@/components/appHeader.vue'

export default {
  components: {
    appHeader,
  },
};
</script>