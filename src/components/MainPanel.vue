<template>
  <el-container>
    <el-header>佛山房屋销售曲线图</el-header>
    <el-main>
      <curve-component :chartdata="chartData" />
    </el-main>
  </el-container>
</template>

<script>
import CurveComponent from './CurveComponent'

export default {
  components: {
    CurveComponent
  },
  data () {
    return {
      chartData: {
        labels: ['January', 'February'],
        datasets: [
          {
            label: 'Data One',
            data: [40, 20]
          }
        ]
      }
    }
  }
}
</script>
