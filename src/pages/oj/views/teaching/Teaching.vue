<template>
    <panel>
      <div slot="title" class="title">
      教学
    </div>
        <div class="main">
        我是教学部分
    </div>
    </panel>
    
  </template>
  
  <script>
import Panel from '../../../admin/components/Panel.vue'
  export default {
  components: { Panel },
  
  }
  </script>
  
  <style>
  .main{
    font-size: 16px;
    margin: 0 50px 40px 50px;
      }
  </style>