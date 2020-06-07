<template>
    <div>
        <fieldset>
            <legend>일정 등록</legend>
            <div class="inputDiv"> 제 목 : <input type="text" v-model="title"></div>
            <div class="inputDiv"> 시작일 : <input type="date" v-model="start"></div>
            <div class="inputDiv"> 종료일 : <input type="date" v-model="end"></div>
            <div>                
                <button @click="createEvent">등록</button>&nbsp;
                <button @click="closeEvent">닫기</button>
            </div>
        </fieldset>
    </div>
</template>

<script>
//import { mapGetters } from 'vuex'

export default {
    data() {
        return {
            title: '',
            start: {},
            end: {}
        }
    },
    props: {
        text: String,
        event: Object
    },
    mounted() {
        this.start = this.event.start.formatDate();
        this.end = this.event.end.formatDate();
    },
    methods: {
        createEvent() {
            this.$emit("event-create", {
                //id: this.event.id,
                title: this.title,
                start: this.start,
                end: this.end
            })
            this.$emit('close')
        },
        closeEvent() {
            this.$emit('close')
        }
    }
}

Date.prototype.formatDate = function() {
  var mm = this.getMonth() + 1; // getMonth() is zero-based
  var dd = this.getDate();

  return [this.getFullYear(),'-',
          (mm>9 ? '' : '0') + mm,'-',
          (dd>9 ? '' : '0') + dd
         ].join('');
};
</script>

<style scoped>
.inputDiv {
    text-align:left;
    margin-bottom: 5px;;
}
</style>