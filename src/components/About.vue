<template>
  <div class="hello">
    <!-- 初始化echats -->
    <div class="map" ref='mapbox' style='height:800px;width:1200px'></div>
    
  </div>
</template>

<script>
import echarts from 'echarts'
import 'echarts/map/js/china.js'
import jsonp from 'jsonp'
// const option = {
//     xAxis: {
//         type: 'category',
//         data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
//     },
//     yAxis: {
//         type: 'value'
//     },
//     series: [{
//         data: [820, 932, 901, 934, 1290, 1330, 1320],
//         type: 'line'
//     }]
// };

const option = {
  title:{
    text:"中国疫情地图"
  },
  series:[{
    name:'确诊人数',
    type:'map',
    map:'china',
    label:{
      show:true,
      color:'#333',
      fontSize:10,
    },
    zoom:1.2,
    roam:true, 
    itemStyle:{
      areaColor:'#eee',
      // borderColor:'blue'
    },
    emphasis:{
      lable:{
        color:'#fff',
        fontSize:12
      },
      itemStyle:{
        areaColor:'#83b5e7'
      }
    },
    data:[

    ]
  }],
  visualMap:[{
    type:'piecewise',
    show:true,
    pieces:[
      {min:10000},
      {min:1000,max:9999},
      {min:100,max:999},
      {min:10,max:99},
      {min:1,max:9}
    ]
    
  }],
  tooltip:{
    trigger:'item'
  }
}
export default {
  name: 'HelloWorld',
  mounted(){
    this.getData();
    this.mychart = echarts.init(this.$refs.mapbox);
    this.mychart.setOption(option)
  },
  methods:{
    getData(){
      jsonp('https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427',{},(err,data)=>{
        if(!err){
          console.log(data)
          let list = data.data.list.map(item=>({name:item.name,value:item.value}))
          option.series[0].data = list;
          this.mychart.setOption(option)
        }
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
.map{
  position: fixed;
  top: 0px;
  left: 0px;
  right: 0px;
  bottom: 0px;
  margin: auto;
}
</style>
