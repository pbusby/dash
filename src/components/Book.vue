<template>
    <div class="bookshelf__book-container" :class="{'selected-book': animate}" @click="passDataToParent" @animationend="animate = false">
        <img v-if="photo" class="bookshelf__book-cover" :src="photo" @load="imageLoaded" />
        <img v-else class="bookshelf__book-cover" :src="require('../assets/antique_cover_' + 1 + '.jpg')" @load="imageLoaded" />
        <div class="bookshelf__book-caption">
            <p>{{title}}</p>
        </div> 


    </div>
    
</template>
<script>
export default {
    props: {
        title: String,
        author: String,
        photo: String,
    },
    data() {
        return {
            animate: false
        }
    },
    methods: {
        imageLoaded() {
            this.$emit('loaded');
        },
        passDataToParent() {
            this.animate = true;
            this.$emit('select-bestseller', {title: this.title, author_name: this.author, photo: this.photo })
        }

    }
}
</script>