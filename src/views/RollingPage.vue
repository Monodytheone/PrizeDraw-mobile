<template>
    <div class="container">
      <!-- 最上方四行信息 -->
      <div class="info-panel">
        <div class="info">{{ `正在抽取：${data.rollingRafflePrize.prizeName}` }}</div>
        <div class="info">{{ `金额：${data.rollingRafflePrize.prizeAmount}` }}</div>
        <div class="info">{{ `已抽次数：${data.rollingRafflePrize.drawnCount}` }}</div>
        <div class="info">{{ `剩余可抽次数：${data.rollingRafflePrize.drawsLeft}` }}</div>
      </div>
  
      <!-- 中间展示滚动的员工信息 -->
      <div class="employee-panel">
        <div class="employee-info">{{ hideEmployeeInfo ? '*******' : currentEmployee.userId }}</div>
        <div class="employee-info">{{ hideEmployeeInfo ? '**' : currentEmployee.userName }}</div>
        <div class="employee-info">{{ currentEmployee.department }}</div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { defineProps, ref, onMounted, onUnmounted } from 'vue';
  
  // 接收 props 数据
  const props = defineProps({
    data: {
      type: Object,
      required: true
    },
    hideEmployeeInfo: {
      type: Boolean,
      required: true
    }
  });
  
  // 获取抽奖员工数据
  const employees = props.data.rollingEmployees || [];
  
  // 当前展示的员工信息
  const currentEmployee = ref({
    userId: '',
    userName: '',
    department: ''
  });
  
  // 随机展示员工信息的函数
  const randomizeEmployee = () => {
    if (employees.length > 0) {
      const randomIndex = Math.floor(Math.random() * employees.length);
      currentEmployee.value = employees[randomIndex];
    }
  };
  
  // 定时器，每秒切换 10 次
  let timer;
  onMounted(() => {
    randomizeEmployee(); // 初始化展示
    timer = setInterval(() => {
      randomizeEmployee();
    }, 100); // 每 0.1 秒切换一次，一秒 10 次
  });
  
  // 组件销毁时清理定时器
  onUnmounted(() => {
    if (timer) clearInterval(timer);
  });
  </script>
  
  <style scoped>
  /* 容器：适配手机端 */
  .container {
    background-color: #1a1a1a; /* 深灰色背景，但不是纯黑 */
    color: #fff; /* 白色文字 */
    height: 100vh;
    width: 100vw;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    font-family: "Helvetica Neue", Arial, sans-serif;
    padding: 20px 10px;
    box-sizing: border-box;
  }
  
  /* 信息面板 */
  .info-panel {
    width: 100%;
    text-align: center;
    font-size: 16px;
    margin-top: 10px;
    line-height: 1.8;
  }
  
  /* 中间员工信息展示 */
  .employee-panel {
    flex: 1; /* 占据主要区域 */
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100%;
    background-color: #333333;
    border-radius: 10px;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.4);
    margin: 20px 0;
    padding: 20px 10px;
    box-sizing: border-box;
  }
  
  .employee-info {
    font-size: 28px;
    font-weight: bold;
    margin: 10px 0;
    color: #ffcc00; /* 金黄色字体，适合移动端视觉 */
    text-align: center;
  }
  
  /* 信息栏文字样式 */
  .info {
    font-size: 14px;
    font-weight: bold;
    color: #cccccc; /* 浅灰色文字 */
    margin-bottom: 5px;
  }
  </style>
  