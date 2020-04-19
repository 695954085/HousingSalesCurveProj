<template>
  <el-container>
    <el-header>佛山房屋销售曲线图</el-header>
    <el-main>
      <el-row type="flex" justify="center">
        <el-col :span="12">
          <curve-component :chartdata="chartData" :options="lineOptions"/>
        </el-col>
        <el-col :span="6" :offset="2">
          <el-row type="flex" justify="center" v-for="item in yoyDatas" :key="item.value">
            <p style="fontSize: 30px">{{item.value}} <i :class="item.icon"></i></p>
          </el-row>
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
      // 同比数据
      yoyDatas: [],
      chartData: {
        labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
        datasets: [
          {
            label: 'In 2018',
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
            label: 'In 2019',
            fill: false,
            borderColor: 'blue',
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
            label: 'In 2020',
            fill: false,
            borderColor: 'yellow',
            data: [
              5255,
              1604,
              7865,
              5518
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
          text: 'Housing Sales Curve'
        },
        tooltips: {
          mode: 'index',
          intersect: false,
          callbacks: {
            // Use the footer callback to display year-on-year comparative data of the items showing in the tooltip
            footer: (tooltipItems, data) => {
              // tooltipItems.forEach(function (tooltipItem) {
              //   console.log('tooltipItem.datasetIndex = ' + tooltipItem.datasetIndex)
              //   console.log(`tooltipItem.index = ${tooltipItem.index}`)
              //   console.log(`data.datasets =  ${data.datasets}`)
              // })
              this.yoyDatas.splice(0, this.yoyDatas.length)
              const lastElement = tooltipItems.pop()
              const lastElementDataset = data.datasets[lastElement.datasetIndex]
              const index = lastElement.index
              tooltipItems.forEach(item => {
                const element = data.datasets[item.datasetIndex]
                const differentValue = element.data[index] - lastElementDataset.data[index]
                if (differentValue > 0) { // 同比下降
                  const ratio = (differentValue / element.data[index])
                  const yoyItem = {}
                  yoyItem.icon = 'el-icon-bottom'
                  yoyItem.value = `${lastElementDataset.label},${lastElement.label}同比${element.label}下降: ${differentValue}套, 比例: ${ratio.toFixed(2) * 100}%`
                  this.yoyDatas.push(yoyItem)
                } else if (differentValue <= 0) { // 同比上升
                  const ratio = (-differentValue / element.data[index])
                  const yoyItem = {}
                  yoyItem.icon = 'el-icon-top'
                  yoyItem.value = `${lastElementDataset.label},${lastElement.label}同比${element.label}上升: ${-differentValue}套, 比例: ${ratio.toFixed(2) * 100}%`
                  this.yoyDatas.push(yoyItem)
                }
              })
              this.yoyDatas.reverse()
            }
          }
        },
        hover: {
          mode: 'nearest',
          intersect: true
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
