<template>
    <div>
        <!-- <fieldset>
            <legend>Event Details</legend>
            <b>Title : </b> {{ event.title }} <br/>
            <b>Start : </b> {{ event.start }} <br/>
            <b>End : </b> {{ event.end }} <br/>
            <b>ID : </b> {{ event.id }} <br/>
        </fieldset> -->

        <fieldset>
            <legend>일정 수정</legend>
            <!-- <input type="text" v-model="title">
            <input type="date" v-model="start">
            <input type="date" v-model="end">

            <button @click="updateEvent">UPDATE</button> -->

            <div class="inputDiv"> 제&nbsp;&nbsp;목 : <input type="text" v-model="title"></div>
            <div class="inputDiv"> 시작일 : <input type="date" v-model="start"></div>
            <div class="inputDiv"> 종료일 : <input type="date" v-model="end"></div>
            <div>                
                <button @click="updateEvent">수정</button>&nbsp;
                <button @click="deleteEvent">삭제</button>&nbsp;
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
        this.title = this.event.title;
        this.start = this.event.start.formatDate();
        this.end = this.event.end.formatDate();
    },
    methods: {
        updateEvent() {
            this.$emit("event-update", {
                id: this.event.id,
                title: this.title,
                start: this.start,
                end: this.end
            })
            this.$emit('close')
        },
        deleteEvent() {
            if (confirm("삭제 하시겠습니까?")) {
                this.$emit("event-delete", {
                    id: this.event.id
                })
                this.$emit('close')
            }
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