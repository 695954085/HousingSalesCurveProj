<template>
  <el-container>
    <el-header>佛山房屋销售曲线图</el-header>
    <el-main>
      <el-row type="flex" justify="center" class="Main__Row">
        <el-col :span="12">
          <curve-component :chartdata="chartData" :options="lineOptions"/>
        </el-col>
      </el-row>
    </el-main>
  </el-container>
</template>

<script>
import CurveComponent from './CurveComponent'
import BarComponent from './BarComponent'
// eslint-disable-next-line
Chart.defaults.global.defaultFontSize = 24;

export default {
  components: {
    CurveComponent,
    BarComponent
  },
  data () {
    return {
      chartData: {
        labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
        datasets: [
          {
            label: '2018年销售数字',
            fill: false,
            borderColor: 'red',
            data: [
              8448,
              4129,
              11261,
              9713,
              12936,
              18250,
              15748,
              14684,
              14079,
              18090,
              19832,
              17608
            ]
          }, {
            label: '2019年销售数字',
            fill: false,
            borderColor: 'rgb(10,10,10)',
            data: [
              6738,
              3873,
              12812,
              11948,
              10818,
              10565,
              9341,
              8953,
              8699,
              8745,
              8904,
              10459
            ]
          }, {
            label: '2020年销售数字',
            fill: false,
            borderColor: 'rgb(20,20,20)',
            data: [
              5255,
              1604,
              7865,
              4056
            ]
          }
        ]
      },
      lineOptions: {
        layout: {
          padding: {
            left: 0,
            right: 0,
            top: 0,
            bottom: 0
          }
        },
        title: {
          display: true,
          text: 'Custom Chart Title'
        }
      },
      barChartData: {
        labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
        datasets: [
          {
            label: '# of Votes',
            data: [12, 19, 3, 5, 2, 3],
            backgroundColor: [
              'rgba(255, 99, 132, 0.2)',
              'rgba(54, 162, 235, 0.2)',
              'rgba(255, 206, 86, 0.2)',
              'rgba(75, 192, 192, 0.2)',
              'rgba(153, 102, 255, 0.2)',
              'rgba(255, 159, 64, 0.2)'
            ],
            borderColor: [
              'rgba(255, 99, 132, 1)',
              'rgba(54, 162, 235, 1)',
              'rgba(255, 206, 86, 1)',
              'rgba(75, 192, 192, 1)',
              'rgba(153, 102, 255, 1)',
              'rgba(255, 159, 64, 1)'
            ],
            borderWidth: 1
          }
        ]
      },
      barOptions: {
        scales: {
          yAxes: [
            {
              ticks: {
                beginAtZero: true
              }
            }
          ]
        },
        tooltips: {
          titleFontColor: 'red'
        }
      }
    }
  }
}
</script>
<style scoped>
.Main__Row{
}
</style>
