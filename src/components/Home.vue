<template>
    <div class="img-backdrop" style="">

        <div class="custom-container">
            <h3 @click.stop="toggleLinks">Links</h3>
            <links-window v-if="linksActive" v-click-outside="toggleLinks"/>
            <div class="metrics-nav"></div>
            <div class="todo-nav"></div>
            <div class="settings-nav"></div>        
            
            <!-- <h1>Blood Lake Fart</h1> -->
        </div>

    </div>
</template>
<script>
import LinksWindow from './LinksWindow.vue'

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
    }
}
</script>