<template>
  <div class="content">
    <!-- 奖项列表 -->
    <div class="title">奖项列表</div>
    <el-table
      :data="tableData.rafflePrizes"
      class="highlight-table"
      style="width: 100%"
      border
      :fit="true"
    >
      <el-table-column prop="prizeName" label="奖项名称" :resizable="false" />
      <el-table-column prop="prizeAmount" label="金额" :resizable="false" />
      <el-table-column prop="quantity" label="数量" :resizable="false" />
      <el-table-column prop="drawnCount" label="已抽次数" :resizable="false" />
      <el-table-column prop="drawsLeft" label="剩余次数" :resizable="false" />
    </el-table>

    <!-- 中奖记录 -->
    <div class="title">中奖记录</div>
    <el-table
      :data="tableData.prizeRecords"
      style="width: 100%"
      border
      :cell-style="{ padding: '6px' }"
    >
      <el-table-column prop="rafflePrizeName" label="奖项名称" />
      <el-table-column prop="prizeAmount" label="中奖金额" />
      <el-table-column prop="winnerId" label="中奖人工号" />
      <el-table-column prop="winnerName" label="中奖人姓名" />
      <el-table-column prop="winnerDepartment" label="中奖人部门" />
      <el-table-column prop="winTime" label="中奖时间" />
    </el-table>

    <!-- 中奖金额汇总表 -->
    <div class="title">中奖金额汇总表</div>
    <el-table :data="tableData.winningAmountByDepartment" style="width: 100%" border>
      <el-table-column prop="department" label="部门" />
      <el-table-column prop="amount" label="金额" />
    </el-table>

    <!-- 员工名字列表 -->
    <div class="title">员工名字</div>
    <el-table
      :data="tableData.employees"
      class="employee-table"
      style="width: 100%; height: 450px"
      border
    >
      <el-table-column prop="userId" label="工号" />
      <el-table-column prop="userName" label="姓名" />
      <el-table-column prop="department" label="部门" />
      <el-table-column label="是否已中奖">
        <template #default="scope">
          <span
            :style="{
              color: scope.row.hasWon ? 'red' : '#000000',
              fontWeight: 'bold'
            }"
          >
            {{ scope.row.hasWon ? '已中奖' : '未中奖' }}
          </span>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script setup>
const props = defineProps({
  data: Object
});

const tableData = props.data;
</script>

<style lang="less" scoped>
/* 页面整体背景和字体 */
.content {
  background-color: #1a1a1a;
  color: #fff;
  padding: 10px;
  box-sizing: border-box;
  font-family: "Helvetica Neue", Arial, sans-serif;
}

/* 标题样式 */
.title {
  font-size: 18px;
  font-weight: bold;
  color: #ffcc00;
  margin: 15px 0 10px;
  text-align: center;
}

/* 奖项列表表格：突出风格 */
.highlight-table {
  border: 3px solid #ffcc00;
  border-radius: 10px;
  overflow: hidden;
}

.highlight-table :deep(.el-table__header-wrapper) {
  background-color: #333333; /* 表头背景颜色 */
}

.highlight-table :deep(.el-table__body-wrapper .el-table__row td) {
  background-color: #ffd700 !important; /* 金黄色单元格 */
  color: #333333 !important; /* 深色字体 */
  font-weight: bold;
  text-align: center;
}

.highlight-table :deep(.el-table__header th) {
  background-color: #ffcc00 !important; /* 表头背景金黄色 */
  color: #333333 !important; /* 字体深灰色 */
  font-weight: bold;
  text-align: center;
}

/* 其他表格样式 */
.el-table {
  margin-top: 10px;
  font-size: 14px;
}

.el-table th {
  background-color: #555555;
  color: #ffffff;
}

.el-table td {
  background-color: #444444;
  color: #ffffff;
}

/* 员工名字表格：高度调整 */
.employee-table {
  height: 450px;
  overflow-y: auto;
}

.employee-table td span {
  font-weight: bold;
}
</style>
