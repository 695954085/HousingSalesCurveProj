<template>
  <el-container>
    <el-header>佛山房屋销售曲线图</el-header>
    <el-main>
      <el-row type="flex" justify="center">
        <el-col :span="12">
          <curve-component :chartdata="chartData" :options="lineOptions" />
        </el-col>
        <el-col :span="6" :offset="2">
          <el-row
            type="flex"
            justify="center"
            v-for="item in yoyDatas"
            :key="item.value"
          >
            <p style="fontSize: 30px">
              {{ item.value }} <i :class="item.icon"></i>
            </p>
          </el-row>
        </el-col>
      </el-row>
    </el-main>
  </el-container>
</template>

<script>
import CurveComponent from './CurveComponent'
import BarComponent from './BarComponent'
import axios from 'axios'
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
        datasets: []
      },
      colors: ['yellow', 'red', 'black', 'pink', 'blue'],
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
  },
  methods: {
    createDateSet (key, data) {
      const values = Object.keys(data).sort((a, b) => {
        return Number.parseInt(a) >= Number.parseInt(b)
      }).reduce((preV, curValue) => {
        preV.push(data[curValue])
        return preV
      }, [])
      return {
        label: `In ${key}`,
        fill: false,
        borderColor: this.colors[Math.floor(Math.random() * 5)],
        data: values
      }
    }
  },
  mounted () {
    axios.get('http://localhost:8080/housingSalesData').then(data => {
      let datas = this.createDateSet('2018', data.data['2018'])
      this.yoyDatas.push(datas)
    })
  }
}
</script>
