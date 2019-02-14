<template>
    <div style="width:800px;height:600px;margin:0 auto;">
         <div id="chartColumn1" style="width:100%; height:400px;">
         
         </div>
    </div>
</template>

<script>
import echarts from "echarts";
export default {
  data() {
    return {
          chartColumn: null,
    }
  },
  mounted:function(){
     $("#chartColumn1").css('width',$(".item_one_child").width())
     
     this.chartColumn = echarts.init(document.getElementById('chartColumn1'));
     this.drawColumnChart()

  },
  
  // activated () {
  // // 由于给echart添加了resize事件, 在组件激活时需要重新resize绘画一次, 否则出现空白bug
  // if (this.chartColumn) {
  //   this.chartColumn.resize()
  // }
  // },

  methods: {
    drawColumnChart() {
      this.chartColumn = echarts.init(document.getElementById("chartColumn1"));
      this.chartColumn.setOption({
        title: {
          text: "在线节点数"
        },
        tooltip: {
          trigger: "axis"
        },
        legend: {
          data: ["在线节点数"]
        },
        grid: {
          left: "3%",
          right: "4%",
          bottom: "3%",
          containLabel: true
        },
        xAxis: {
          type: "category",
          boundaryGap: false,
          data: [1,2,3,4,5]
        },
        yAxis: {
          type: "value"
        },
        series: [
          {
            name: "在线节点数",
            type: "line",
            stack: "总量",
            data: [6,7,8,9,10]
          }
        ]
      });
      window.addEventListener('resize', () => {
        
   this.chartColumn.resize()
})
     
    }
  }
};
</script>

<style>
</style>
 