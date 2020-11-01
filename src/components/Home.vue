<template>
    <div class="img-backdrop" style="">

        <div class="custom-container">
            <h3 @click.stop="toggleLinks">Links</h3>
            <links-window v-if="linksActive" v-click-outside="toggleLinks"/>
            <div class="weather"></div>
            <!-- absolute center -->
            <div class="banner-time">
                <h3>{{this.currentTime}}</h3>
                <h4>{{this.dateToday}}</h4>
            </div>
            <!-- sidebar right -->
            <div class="metrics-nav"></div>
            <!-- bottom right -->
            <div class="todo-nav"></div>
            <!-- bottom left -->
            <div class="settings-nav"></div>        
            
        </div>

    </div>
</template>
<script>
import LinksWindow from './LinksWindow.vue'
import dayjs from 'dayjs'
// import utcPlugin from 'dayjs/plugin/utc'
// import isBetween from 'dayjs/plugin/isBetween'
// https://day.js.org/docs/en/display/format

// dayjs.extend(utcPlugin)


export default {
    components: {
        LinksWindow
    },

    data () {
        return {
            linksActive: false
        }
    },
    methods: {
        toggleLinks() {
            console.log("Hello world!");
            this.linksActive = !this.linksActive;
        }
    },
    computed: {
        dateToday() {
            return dayjs().format('MMMM D') 
            // debugger // eslint-disable-line
        },
        currentTime() {
            return dayjs().format('h:mm')
        }
    },
    mounted() {
        this.dateToday();
    }
}
</script>