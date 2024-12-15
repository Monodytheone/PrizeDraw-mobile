<template>
    <div class="container">
      <div class="message">抽奖未开始</div>
    </div>
  </template>
  
  <script setup>
  </script>
  
  <style scoped>
  /* 背景设置为特别浅的灰色 */
  .container {
    background-color: #f8f8f8; /* 特别浅的灰色 */
    display: flex;
    justify-content: center; /* 水平居中 */
    align-items: flex-start; /* 顶部对齐 */
    height: 100vh; /* 全屏高度 */
    width: 100vw; /* 全屏宽度 */
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  /* 使用 margin 将内容推到黄金分割线位置 */
  .message {
    font-size: 55px;
    color: #333; /* 深灰色字体 */
    margin-top: 30vh; /* 偏移到接近黄金分割线位置 */
  }
  </style>
  