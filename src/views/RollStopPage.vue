<template>
    <div class="container">
        <!-- 最上方四行信息 -->
        <div class="info-panel">
            <div class="info">{{ `正在抽取：${data.rafflePrize.prizeName}` }}</div>
            <div class="info">{{ `金额：${data.rafflePrize.prizeAmount}` }}</div>
            <div class="info">{{ `已抽次数：${data.rafflePrize.drawnCount}` }}</div>
            <div class="info">{{ `剩余可抽次数：${data.rafflePrize.drawsLeft}` }}</div>
        </div>

        <!-- 中间展示当前获奖者信息 -->
        <div class="employee-panel">
            <div class="employee-info">{{ hideEmployeeInfo ? '*******' : data.currentWinner.userId }}</div>
            <div class="employee-info">{{ hideEmployeeInfo ? '**' : data.currentWinner.userName }}</div>
            <div class="employee-info">{{ data.currentWinner.department }}</div>
        </div>

        <!-- 历史中奖记录 -->
        <div class="record-panel">
            <el-table :data="data.prizeRecordOfCurrentRafflePrize" border style="width: 100%;">
                <el-table-column prop="rafflePrizeName" label="奖项" width="80"></el-table-column>
                <el-table-column prop="prizeAmount" label="金额" width="60"></el-table-column>
                <el-table-column label="中奖人" width="80">
                    <template #default="scope">
                        <span>{{
                            hideEmployeeInfo ? '*'.repeat(scope.row.winnerName && scope.row.winnerName.length) :
                            scope.row.winnerName
                            }}</span>
                    </template>
                </el-table-column>
                <el-table-column prop="winnerDepartment" label="部门" width="80"></el-table-column>
                <el-table-column prop="winTime" label="中奖时间" width="120">
                    <template #default="scope">
                        {{ formatDate(scope.row.winTime) }}
                    </template>
                </el-table-column>
            </el-table>
        </div>
    </div>
</template>

<script setup>
import { defineProps } from 'vue';

// 接收 props 数据
const props = defineProps({
    data: {
        type: Object,
        required: true,
    },
    hideEmployeeInfo: {
        type: Boolean,
        required: true
    }
});

// 格式化中奖时间
const formatDate = (date) => {
    const d = new Date(date);
    return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}-${String(d.getDate()).padStart(2, '0')} ${String(d.getHours()).padStart(2, '0')}:${String(d.getMinutes()).padStart(2, '0')}`;
};
</script>

<style scoped>
/* 容器：适配手机端 */
.container {
    background-color: #1a1a1a;
    /* 深灰色背景 */
    color: #fff;
    /* 白色文字 */
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

/* 中间获奖者信息展示 */
.employee-panel {
    flex: 1;
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
    color: #ffcc00;
    /* 金黄色字体 */
    text-align: center;
}

/* 历史记录表格 */
.record-panel {
    width: 100%;
    background-color: #444444;
    padding: 10px;
    border-radius: 10px;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.4);
}

.el-table {
    background-color: #444444;
    border-color: #555555;
    font-size: 12px;
    /* 压缩字体 */
}

.el-table th {
    background-color: #555555;
    color: #ffffff;
    font-size: 12px;
    /* 更小的标题字体 */
}

.el-table td {
    color: #ffffff;
    font-size: 12px;
    /* 更小的内容字体 */
}
</style>