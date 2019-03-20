<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm">task name</div>
      <div class="col-sm">task id</div>
      <div class="col-sm">options</div>
    </div>
    <div class="row" v-for="obj in todoTaskData" :key="obj.businessKey">
      <single-row v-bind:value="obj.title"></single-row>
      <single-row v-bind:value="obj.businessKey"></single-row>
      <div class="col-sm">
        <a
          class="weui-btn weui-btn_mini weui-btn_plain-primary"
          v-on:click="confirm(obj.taskId);"
          >confirm</a
        >
        <a
          class="weui-btn weui-btn_mini weui-btn_plain-primary"
          v-on:click="info();"
          >detail</a
        >
        <a
          class="weui-btn weui-btn_mini weui-btn_warn"
          v-on:click="deny(obj.taskId);"
          >deny</a
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>
