<template>
  <div class="home">
      <Row>
        <Col span="12">
            <DatePicker type="datetime" :options="options3" @on-change="changeValue" placeholder="Select date" style="width: 200px"></DatePicker>
        </Col>
        <Col span="12">
            <DatePicker type="datetime" :options="options4" placeholder="Select date" style="width: 200px"></DatePicker>
        </Col>
    </Row>
  </div>
</template>
<script>
    export default {
      name: 'Home',
        data () {
            return {
                options3: {
                    disabledDate (date) {
                      console.log(date)
                        return date && date.valueOf() < Date.now() - 86400000;
                    }
                },
                options4: {
                    disabledDate (date) {
                        const disabledDay = date.getDate();
                        return disabledDay === 15;
                    }
                }
            }
        },
        methods: {
          changeValue(a) {
            // console.log(a)
          }
        }
    }
</script>
