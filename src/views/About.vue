<template>
    <div class="app">   
        <div > 开始时间:
        <date-picker type="datetime" v-model="startTime" placeholder="请选择开始时间" :options="startTimeOption" @on-change="onStartTimeChange"></date-picker>
        </div>
        <div > 结束时间:
        <date-picker type="datetime" v-model="endTime" placeholder="请选择结束时间" :options="endTimeOption" @on-change="onEndTimeChange"></date-picker>
        </div>
        <div>我是about</div>
        <Card title="查询条件" style="width:100%">
            <Form
            class="queryFormCls" 
            ref="queryForm" 
            :model="queryForm" 
            :label-width="70">
                <Row class="form-item">
                    <!-- 日期选择器 -->
                    <Col span="7">
                        <FormItem label="创建时间:" :label-width="100" prop="createTimes">
                            <Row>
                                <Col span="11">
                                    <DatePicker 
                                        type="datetime" 
                                        v-model="queryForm.createTimes.startTime"
                                        placeholder="请选择开始时间"
                                        style="width: 100%"
                                        :value="queryForm.createTimes.startTime"
                                        format="yyyy-MM-dd HH:mm:ss"
                                        placement="bottom-end"
                                        :options="startTimeOptions"
                                        @on-change="startTimeChange">
                                    </DatePicker>
                                    <!-- <DatePicker type="date" :options="startTimeOption" @on-change="startTimeChange" placeholder="开始时间"
                                        v-model="starttime"></DatePicker> -->
                                </Col>
                                <Col span="2" style="text-align: center">至</Col>
                                <Col span="11">
                                    <DatePicker 
                                        type="datetime" 
                                        v-model="queryForm.createTimes.endTime"
                                        placeholder="请选择结束时间"
                                        style="width: 100%"
                                        :value="queryForm.createTimes.endTime"
                                        format="yyyy-MM-dd HH:mm:ss"
                                        placement="bottom-end"
                                        :options="endTimeOptions"
                                        @on-change="endTimeChange"
                                        @on-open-change="openchange($event,'info')">
                                    </DatePicker>
                                    <!-- <DatePicker type="date" :options="endTimeOption" @on-change="endTimeChange" placeholder="结束时间"
                                        v-model="endtime"></DatePicker> -->
                                </Col>
                            </Row>
                        </FormItem>
                    </Col>
                </Row>
            </Form>
        </Card>
     </div>          
</template>

<script>
export default {
    name: 'about',
    data() {
        return {
            startTime: '',
            endTime: '',
            startTimeOption: {},
            endTimeOption: {},
            queryForm: {
                createTimes: {
                    startTime: '',
                    endTime: ''
                } // 创建时间
            },
             // 开始时间操作
            startTimeOptions: {},
            // 结束时间操作
            endTimeOptions: {}
        }
    },
    mounted() {
        this.startTime = new Date()
        this.endTime = new Date((this.startTime/1000+(86400*4))*1000) 
        this.onStartTimeChange(this.startTime)
        // this.onEndTimeChange(this.endTime)
        this.getDates()
        console.log()
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
                    return  !((+new Date(endTime) >= +new Date(startTime)) && ((+new Date(endTime) - (+new Date(startTime))) <= 345600000))  
                }
            }
            this.startTime = startTime
        },
        /**
         * 结束时间发生变化时触发,设置开始时间不可选择的日期
         * 开始时间小于等于结束时间,且小于等于当前时间
         * @param {string} date 格式化后的日期
         * @param {string} type 当前的日期类型
         */
        onEndTimeChange(endTime, type) {
            //   this.startTimeOption = {
            //       disabledDate(startTime) {
            //         // console.log(startTime)
            //           return  startTime < new Date(endTime) || startTime < Date.now()
            //       }
            //   }
            this.onStartTimeChange(this.startTime)
            this.endTime = endTime
        },
        // 设置默认时间
        getDates() {
            // 设置日期默认值
            this.queryForm.createTimes.startTime = this.$moment().startOf('day').format('YYYY-MM-DD HH:mm:ss')
            this.queryForm.createTimes.endTime = this.$moment().endOf('day').format('YYYY-MM-DD HH:mm:ss')
            this.startTimeChange(this.queryForm.createTimes.startTime)
            this.endTimeChange(this.queryForm.createTimes.endTime)
            console.log(this.queryForm.createTimes.startTime, this.queryForm.createTimes.endTime, '--------')
        },
        // 开始日期时间限制
        startTimeChange(startTime, type) {
            this.endTimeOptions = {
                disabledDate(endTime) {
                    // 结束时间限制
                    return endTime < new Date(startTime) || endTime > Date.now() || new Date(endTime).getTime() - new Date(startTime).getTime() > 4 * 24 * 60 * 60 * 1000 
                    // 可选时间周期
                }
            }
        },
        // 结束日期时间限制
        endTimeChange(endTime, type) {
            // 设置结束时间
            this.startTimeOptions = {
                disabledDate(startTime) {
                    // 开始时间限制
                    return startTime > new Date(endTime) || startTime > Date.now() 
                }
            }
        },
        // 打开结束日期弹框
        openchange(e, type) {
            // 打开弹框弹出提示消息
            if (e) {
                this.$Message[type]({
                    background: true,
                    content: '日期范围最大可选择5天'
                })
            }
        }
    }
}    
</script>

<style>

</style>
