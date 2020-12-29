<template>
     <div class="app">  
          <router-view></router-view> 
     </div>          
</template>

<script>

export default {
  data() {
        return {
            startTime: '',
            endTime: '',
            startTimeOption: {},
            endTimeOption: {}
        }

},
 mounted() {
          this.startTime = new Date()
          this.endTime = new Date((this.startTime/1000+(86400*5))*1000) 
          // this.onStartTimeChange(this.startTime)
          this.onEndTimeChange(this.endTime)
      },
      methods: {
          /**
           * 开始时间发生变化时触发,设置结束时间不可选择的日期
           * 结束时间应大于等于开始时间,且小于等于当前时间
           * @param {string} startTime 格式化后的日期
           * @param {string} type 当前的日期类型
           */
          onStartTimeChange(startTime, type) {
              this.endTimeOption = {
                  disabledDate(endTime) {
                      return !( endTime > new Date(startTime) && (endTime - startTime < 432000000)  ) || endTime < new Date()
                  }
              }
          },
          /**
           * 结束时间发生变化时触发,设置开始时间不可选择的日期
           * 开始时间小于等于结束时间,且小于等于当前时间
           * @param {string} date 格式化后的日期
           * @param {string} type 当前的日期类型
           */
          onEndTimeChange(endTime, type) {
              this.startTimeOption = {
                  disabledDate(startTime) {
                      return  startTime < new Date(endTime) || startTime < Date.now()
                  }
              }
          } 
      }
      }    
</script>

<style>

</style>