# 一、font-awesome

```js
npm install font-awesome --save

//在main.js中
import 'font-awesome/css/font-awesome.min.css'
```

# 二、vue-baidu-map

```jsx
npm install vue-baidu-map --save

//在main.js中,这是全局注册，将会增加打包的体积
import BaiduMap from 'vue-baidu-map'
Vue.use(BaiduMap,{
  ak:'HMsRLrPGidU6hIisM4HYgx0APRKhpm6p'
});


//局部注册,按需引入组件，单独写ak
<template>
  <baidu-map class="bm-view" ak="YOUR_APP_KEY">
  </baidu-map>
</template>

<script>
import BaiduMap from 'vue-baidu-map/components/map/Map.vue'
export default {
  components: {
    BaiduMap
  }
}
</script>

<style>
.bm-view {
  width: 100%;
  height: 300px;
}
</style>
```

# 三、echarts

```js
npm install echarts --save

//画图步骤
//step1.基于准备好的dom，初始化echarts实例（这个dom一定要指定宽高才行）
var myChart = echarts.init(document.getElementById('main'));

//step2.指定图表的配置项和数据 该部分查阅官方文档https://echarts.apache.org/zh/option.html#title
var option = {
    title: {
        text: 'ECharts 入门示例'
    },
    tooltip: {},
    legend: {
        data:['销量']
    },
    xAxis: {
        data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
    },
    yAxis: {},
    series: [{
        name: '销量',
        type: 'bar',
        data: [5, 20, 36, 10, 10, 20]
    }]
};

//step3.使用刚指定的配置项和数据显示图表。
myChart.setOption(option);
```

## 3.1 重要的option

* dataset

  ```js
  {
      'product': ['Matcha Latte', 'Milk Tea', 'Cheese Cocoa', 'Walnut Brownie'],
      'count': [823, 235, 1042, 988],
      'score': [95.8, 81.4, 91.2, 76.9],
       ......
  }
  ```

* series

  ```js
  //画柱状图
  series: [
      {
          type: 'bar',
          encode: {
              // 将 "amount" 列映射到 X 轴。
              x: 'score',
              // 将 "product" 列映射到 Y 轴。
              y: 'product'
          }
       }，
      ...
  ]
  
  //画折线图
  series: [
      {
          type: 'line',
          name:'xxx',//用于tooltip的显示，legend 的图例筛选
          encode: {
              seriesName:[],
              // 将 "amount" 列映射到 X 轴。
              x: 'score',
              // 将 "product" 列映射到 Y 轴。
              y: 'product'
          }
       }，
      ...
  ]
  
  //画饼图
  series: [
      {
          type: 'pie',
          name:'xxx',//用于tooltip的显示，legend 的图例筛选
          encode: {
              value:'score',
              itemName:'xxx'
          }
       }，
      ...
  ]
  ```

  