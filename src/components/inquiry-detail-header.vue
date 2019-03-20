<template>
  <div class="header-container fluid-container">
    <div class="row" v-for="header in headers" :key="header.short">
      <h3
        :class="{ selected: currentHeader === header }"
        @click="$emit('selecteHeader', header);"
      >
        {{ header.short }}
      </h3>
    </div>
    <div class="row">
      <p>current selected header: {{ currentHeader }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    headers: "",
    currentHeader: ""
  },
  methods: {
    selectHeader: function(header) {
      console.log("sup-header @click=selectHeader" + header.short);
    }
  }
};
</script>

<style>
.selected {
  background-color: purple;
}
</style>
