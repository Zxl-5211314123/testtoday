<template>
  <div class="hello">
    <div id="myChart" ref="imageWrapper" style="height:300px;width:600px;">
    </div>
    <button @click="screenshot">点击截图</button>
    <img id="new_img" :src="img" v-if="img" />
    <div id="leida">

    </div>
  </div>
</template>

<script>
  import echarts from 'echarts'
  // import html2canvas from 'html2canvas';
  export default {
    name: 'HelloWorld',
    data() {
      return {
        img: ''
      }
    },
    mounted() {
      this.drawEcharts();
      this.getcategory();
    },
    methods: {
      screenshot() {
        console.log(window, "html2canvas")
        html2canvas(this.$refs.imageWrapper, {
          backgroundColor: '#BDDDF2',
          width: 750,
          height: 1334,
        }).then((canvas) => { // 第一个参数是需要生成截图的元素,第二个是自己需要配置的参数,宽高等
          this.img = canvas.toDataURL('image/png');
          console.log(this.img, "this.img")
        })
      },
      getcategory() {
        let bookCategoryChart = this.$echarts.init(document.getElementById('leida'));
        let option = {
          tooltip: {
            trigger: 'item',
            position: function (p) { //其中p为当前鼠标的位置
              return [p[0], p[1]];
            },
          },
          legend: {
            orient: 'vertical',
            right: '5%',
            textStyle: {
              color: '#DFE0E5',
              fontWeight: 'bold'
            },
            data: ['书刊类别分布', '借阅类别分布']
          },
          radar: {
            splitNumber: 2, // 雷达图圈数设置
            center: ['50%', '50%'],
            radius: '65%',
            name: {
              textStyle: {
                color: '#DFE0E5',
                backgroundColor: '#121E36'
              }
            },
            indicator: [{ name: '', max: 50},
            { name: '', max: 50},
            { name: '', max: 50},
            { name: '', max: 50},
            { name: '', max: 50},
            { name: '', max: 50}],
            splitArea: {
              show: false,
              areaStyle: {
                color: 'rgba(255,0,0,0)', // 图表背景的颜色
              },
            },
          },
          series: [{
            name: '书刊类别 vs 借阅类别',
            type: 'radar',
            data: [{
                value: [1, 2, 3, 4, 5, 6, 7],
                name: '书刊类别分布',
                itemStyle: {
                  normal: {
                    color: '#F75325' //显示颜色与填充颜色对应
                  }
                },
                areaStyle: {
                  normal: {
                    color: '#F75325' //填充的颜色
                  }
                }
              },
              {
                value: [1,2,3,4,5,6],
                name: '借阅类别分布',
                itemStyle: {
                  normal: {
                    color: '#7B52CC'
                  }
                },
                areaStyle: {
                  normal: {
                    color: '#7B52CC'
                  }
                }
              }
            ]
          }]
        };
        bookCategoryChart.setOption(option)
      },
      drawEcharts() {
        let myChart = this.$echarts.init(document.getElementById('myChart'));
        myChart.setOption({
          xAxis: {
            type: 'category',
            data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
          },
          yAxis: {
            type: 'value'
          },
          series: [{
            data: [820, 932, 901, 934, 1290, 1330, 1320],
            type: 'line',
            smooth: true
          }]
        });
        window.onresize = myChart.resize;
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1,
  h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
