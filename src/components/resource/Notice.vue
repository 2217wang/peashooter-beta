<template>
  <div id="notice">
    <strong>公告</strong><i class="el-icon-chat-dot-square"></i>
    <slot></slot>
  </div>
</template>

<script>
  export default {
    name: "Notice"
  }
</script>

<style scoped>
  #notice {
    /*background-color: #D7D7D7;*/
    padding: 10px 21px;
    border-radius: 10px;
  }
  .el-icon-chat-dot-square {
    margin-left: 3px;
  }
  .notice-item {
    margin: 10px 0 2px 0;
  }
</style>