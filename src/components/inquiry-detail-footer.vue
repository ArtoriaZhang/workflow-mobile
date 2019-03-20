<template>
  <div class="fluid-container footer-container">
    <div class="row" v-for="(link, index) in links" :key="index">
      <a :href="link.href" :target="link.target">{{ link.name }}</a>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    links: Array
  }
};
</script>
