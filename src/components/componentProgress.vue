<template>
  <div class='progress-container'>
    <svg
      style='transform: rotate(135deg)'
      :width='width'
      :height='width'
    >
    <!-- 渐变色 -->
      <defs>
        <linearGradient id="grad1" x1="0%" y1="0%" x2="100%" y2="0%">
          <!-- 进度结束颜色 -->
          <stop :offset="(100 - progress) / 100" :style="{'stop-color': progress === 0 ? '#D8D8D8' : barColor.barColorEnd}" />
          <!-- 进度开始颜色 -->
          <stop offset="100%" :style="{'stop-color': progress === 0 ? '#D8D8D8' : barColor.barColorStart}" />
        </linearGradient>
      </defs>
      <!-- 内圆 -->
      <circle
        :r='(width - radius) / 2'
        :cy='width / 2'
        :cx='width / 2'
        :stroke-width='radius'
        :stroke='backgroundColor'
        fill='none'
        :stroke-linecap="isRound ? 'round' : 'square'"
        :stroke-dasharray='(width - radius) * 3.14 * 0.75'
      />
      <!-- 外圆 -->
      <circle
        ref='$bar'
        :r='(width - radius) / 2'
        :cy='width / 2'
        :cx='width / 2'
        stroke="url(#grad1)"
        :stroke-width='radius'
        :stroke-linecap="isRound ? 'round' : 'square'"
        :stroke-dasharray='(width - radius) * 3.14'
        :stroke-dashoffset='
          isAnimation
            ? (width - radius) * 3.14
            : ((width - radius) * 3.14 * (100 - progress)) / 100
        '
        fill='none'
      />
    </svg>
    <!-- 文字说明 -->
    <div class='progress-text'>
      <p class='progress-number'>{{number}}</p>
      <p class='progress-title'>{{title}}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ComponentProgress',
  props: {
    width: [Number, String], // 圆的大小
    radius: [Number, String], // 进度条厚度
    progress: [Number, String], // 进度条百分比
    barColor: Object, // 进度条颜色
    backgroundColor: String, // 背景颜色
    number: Number,
    title: String,
    isAnimation: {
      // 是否是动画效果
      type: Boolean,
      default: true
    },
    isRound: {
      // 是否是圆形画笔
      type: Boolean,
      default: true
    },
    id: {
      // 组件的id，多组件共存时使用
      type: [String, Number],
      default: 1
    },
    duration: {
      // 整个动画时长
      type: [String, Number],
      default: 1000
    },
    delay: {
      // 延迟多久执行
      type: [String, Number],
      default: 50
    },
    timeFunction: {
      // 动画缓动函数
      type: String,
      default: 'cubic-bezier(0.99, 0.01, 0.22, 0.94)'
    }
  },
  mounted () {
    if (this.isAnimation) {
      // 生成动画样式文件
      let style = document.createElement('style')
      style.type = 'text/css'
      style.innerHTML = `
      @keyframes circle_progress_keyframes_name_${this.id} {
      from {stroke-dashoffset: ${(this.width - this.radius) * 3.14}px}
      to {stroke-dashoffset: ${((this.width - this.radius) *
        3.14 *
        (100 - this.progress)) /
        100}px}}
      .circle_progress_bar${this.id} {animation: circle_progress_keyframes_name_${this.id} ${this.duration}ms ${this.delay}ms ${this.timeFunction} forwards}`

      // 添加新样式文件
      document.getElementsByTagName('head')[0].appendChild(style)

      // 往svg元素中添加动画class
      this.$refs.$bar.classList.add(`circle_progress_bar${this.id}`)
    }
  }
}
</script>
<style scoped>
.progress-container{
  width: 100%;
  text-align: center;
  position: relative;
  margin-top: 18px;
}
.progress-text{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
.progress-number{
    height:20px;
    font-size:20px;
    font-family:DINAlternate-Bold,DINAlternate;
    font-weight:bold;
    color:rgba(20,117,213,1);
    line-height:20px;
    letter-spacing:1px;
    margin: 10px auto;
  }
  .progress-title{
    height:28px;
    font-size:20px;
    font-family:PingFang-SC-Bold,PingFang-SC;
    font-weight:bold;
    color:rgba(153,153,153,1);
    line-height:28px;
    margin: 10px auto;
  }
</style>
