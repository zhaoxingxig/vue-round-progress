<template>
  <div id="app">
    <component-progress
      class='upgrade-progress'
      :isAnimation='options.isAnimation'
      :isRound='options.isRound'
      :width='options.width'
      :radius='options.radius'
      :progress='options.progressPro * 0.75'
      :barColor='options.barColors'
      :duration='options.duration'
      :timeFunction='options.timeFunction'
      :backgroundColor='options.backgroundColor'
      :title='options.title'
      :number='options.number'
      >
    </component-progress>
  </div>
</template>

<script>
import ComponentProgress from './components/componentProgress'

export default {
  name: 'App',
  components: {
    ComponentProgress
  },
  data () {
    return {
      options: {
        width: 120, // 最大环宽度  直径
        radius: 5, //  环宽
        progressPro: 50, // 进度百分数
        barColors: { // 进度渐变色
          barColorStart: '#3385D0',
          barColorEnd: '#5AB7F1'
        },
        duration: 1000, // 动画时长
        title: '晚',
        number: 166,
        backgroundColor: '#DCDADD', // 进度底色
        // 动画类型
        timeFunction: 'cubic-bezier(0.99, 0.01, 0.22, 0.94)',
        // 是否动画效果
        isAnimation: true,
        // 是否圆形描边
        isRound: true
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
