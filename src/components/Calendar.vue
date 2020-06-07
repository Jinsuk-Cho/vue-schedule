<template>
    <div>
        <Fullcalendar
            locale="ko" 
            :plugins="calendarPlugins"
            :header="{
                left: 'title',
                center: 'dayGridMonth, timeGridWeek, timeGridDay, listWeek',
                right: 'prev today next'
            }"
            :buttonText="{
                today: '오늘',
                month: '월',
                week: '주',
                day: '일',
                list: '리스트'
            }"
            :weekends="true"
            :selectable="true"
            :editable="true"
            :events="events"
            @select="handleSelect"
            @eventClick="handleClick"
            @eventResize="updateEvent"
            @eventDrop="updateEvent"
            @eventRender="eventRender"
        />

        <modals-container v-on:event-update="updateEvent" v-on:event-delete="deleteEvent"  v-on:event-create="createEvent"/>
    </div>
</template>

<script>
require('@fullcalendar/core/main.min.css')
require('@fullcalendar/daygrid/main.min.css')
require('@fullcalendar/timegrid/main.min.css')

import Fullcalendar from '@fullcalendar/vue'
import DayGridPlugin from '@fullcalendar/daygrid'
import TimeGridPlugin from '@fullcalendar/timegrid'
import InteractionPlugin from '@fullcalendar/interaction'
import ListPlugin from '@fullcalendar/list'

import { mapGetters } from 'vuex'
import UpdateModal from './UpdateModal'
import CreateModal from './CreateModal'
import CommentsModal from './CommentsModal'

export default {
    data: () => ({
        calendarPlugins: [
            DayGridPlugin,
            TimeGridPlugin,
            InteractionPlugin,
            ListPlugin
        ],
        events: []
    }),
    components: {
        Fullcalendar
    },
    computed: {
        ...mapGetters(["EVENTS"])
    },
    methods: {
        handleSelect(arg) {
            this.$modal.show(CreateModal, {
                text: 'This is from the component',
                event: arg
            })
        },
        handleClick(arg) {
            this.$modal.show(UpdateModal, {
                text: 'This is from the component',
                event: arg.event
            })
        },
        updateEvent(arg) {
            //this.$store.commit("UPDATE_EVENT", arg.event);

            let evnetId = arg.event === undefined ? arg.id : arg.event.id;
            let title = arg.event === undefined ? arg.title : arg.event.title;
            let start = arg.event === undefined ? arg.start : arg.event.start;
            let end = arg.event === undefined ? arg.end : arg.event.end;

            let index = this.events.findIndex(_event => _event.id == evnetId);
            this.events[index].title = title;
            if (start.constructor.toString().indexOf("Date") == -1) {
                this.events[index].start = start;
            } else {
                this.events[index].start = start.formatDate();
            }

            if (end.constructor.toString().indexOf("Date") == -1) {
                this.events[index].end = end;
            } else {
                this.events[index].end = end.formatDate();
            }

            console.log(this.events)
        },
        deleteEvent(arg) {
            let evnetId = arg.event === undefined ? arg.id : arg.event.id;
            let index = this.events.findIndex(_event => _event.id == evnetId);
            this.events.splice(index, 1);
        },
        createEvent(arg) {
            this.events.push({
                id: (new Date()).getTime(),
                title: arg.title,
                start: arg.start,
                end: arg.end,
                allDay: arg.allDay
            })
        },
        eventRender(arg) {
            let span = document.createElement('span')
            span.setAttribute('class', 'fa fa-comments')
            arg.el.appendChild(span)
            span.addEventListener('click', event => {
                event.stopPropagation(); 
                this.$modal.show(CommentsModal, {
                    text: "This is for the comments"
                })
            })
        },
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

</style>