<template>
  <div id="app">
    <el-button type="primary" @click="dialogVisible = true"
      >click to open the Dialog</el-button
    >
    <el-dialog
      v-model="dialogVisible"
      width="30%"
      :before-close="handleClose"
      custom-class="dialog"
      top="20vh"
      :show-close="false"
      :close-on-click-modal="false"
      @opened="showBtn"
    >
      <template #title>
        <span class="title">温馨提示</span>
        <hr />
      </template>
      <span>
        本系统所提供的碳排放数据信息用于企业内部管理和参考，
        其真实性、准确性未经主管部门审核确认，仅作为与碳排放相关行政、法律行为的参考，我公司
        不对所获取信息的真实性、准确性、完整性负责、用户或使用方应对基于此进行的法律行为负责，我公司不承担任何法律责任。
      </span>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false" v-show="show"
            >我已读</el-button
          >
          <el-button
            @click="dialogVisible = false"
            disabled="disabled"
            v-show="!show"
            >我已读({{ count }})s</el-button
          >

          <span class="check">
            <el-checkbox v-model="checked" label="1周内不提醒"></el-checkbox>
          </span>
        </span>
      </template>
    </el-dialog>
  </div>
</template>
<script lang="ts" setup>
import { ref } from 'vue'
import { ElMessageBox } from 'element-plus'

const dialogVisible = ref(true)
const checked = ref(true)
const handleClose = (done: () => void) => {
  ElMessageBox.confirm('Are you sure to close this dialog?')
    .then(() => {
      done()


})
    .catch(() => {
      // catch error
    })
}
let timer: any = null
let show: any = ref(false)
let count: any = ref(0)
const showBtn = () => {
  console.log('123')
  show.value = false
  const TIME_COUNT = 5
  if (!timer) {
    count.value = TIME_COUNT
    show.value = false
    timer = setInterval(() => {
      if (count.value > 0 && count.value <= TIME_COUNT) {
        count.value--
      } else {
        show.value = true
        clearInterval(timer)
        timer = null
      }
    }, 1000)
  }
}
</script>
<style lang="less">
#app {
  width: 200px;
  height: 200px;
  font-size: 24px;
  margin: 50px 20px;
  .dialog {
    > .el-dialog__header {
      padding: 20px 5px;
      font-size: 16px;
      > .title {
        display: inline-block;
        height: 25px;
        padding-left: 8px;
      }
      > hr {
        text-align: center;
        width: 98%;
      }
    }
    > .el-dialog__body {
      padding: 0px 25px;
    }
    > .el-dialog__footer {
      text-align: center;
      > .dialog-footer {
        position: relative;
        > .el-button {
          width: 30%;
        }
        > .check {
          position: absolute;
          top: 5px;
          left: 120px;
        }
      }
    }
  }
}
</style>
