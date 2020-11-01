<template>
    <div>
        <div class="dropdown-triangle" style="position: absolute; top: 30px; left: 30px;"></div>
        <div style="position: absolute; top: 40px;">
            <div class="links-window">
                <!-- <transition name="slide"> -->
                <div class="links-list" :class="{'offscreen-left': addLinkActive, 'onscreen-left': !addLinkActive}">
                    <ul style="color: black;">
                        <li>cool link 1</li>
                        <li>cool link 2</li>
                        <li>cool link 3</li>
                        <li>cool link 4</li>
                        <li>cool link 5</li>
                        <li>cool link 6</li>
                        <li>cool link 1</li>
                        <li>cool link 2</li>
                        <li>cool link 3</li>
                        <li>cool link 4</li>
                        <li>cool link 5</li>
                        <li>cool link 6</li>
                        <li>cool link 1</li>
                        <li>cool link 2</li>
                        <li>cool link 3</li>
                        <li>cool link 4</li>
                        <li>cool link 5</li>
                        <li>cool link 6</li>
                        <li>cool link 1</li>
                        <li>cool link 2</li>
                        <li>cool link 3</li>
                        <li>cool link 4</li>
                        <li>cool link 5</li>
                        <li>cool link 6</li>
                    </ul>
                    
                </div>
                <!-- </transition> -->
                
                    <div class="funky-container">
                        <!-- <transition name="slide"> -->
                        <div class="add-link-menu" :class="{'onscreen-left': addLinkActive, 'offscreen-left': !addLinkActive}">
                            <!-- <label>Title</label> -->
                            <div @click.stop="deactivateAddLinkMenu" class="back-btn" style="color: black;">&larr;</div>
                            <form action="">
                                <label for="Title">Title</label>
                                <input type="text" name="Title">
                                <label for="Title">Link</label>
                                <input type="text" label="Link">
                                <a href="#" class="btn">Create</a>
                            </form>
                        </div>
                        <!-- </transition> -->
                    </div>
                
            </div>
            <div v-if="!addLinkActive" class="links-window__add-link-button" :class="{'onscreen-left': addLinkActive, 'offscreen-left': !addLinkActive}">
                <h3 style="" @click.stop="activateAddLinkMenu">+ Add Link</h3>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            linksListActive: true,
            addLinkActive: false
        }

    },
    methods: {
        activateAddLinkMenu() {
            this.addLinkActive = true;
            this.calculateMenuHeight();
        },
        deactivateAddLinkMenu() {
            this.addLinkActive = false;
            this.calculateMenuHeight();
        },
        calculateMenuHeight() {
            // debugger // eslint-disable-line
            const parent = document.querySelector('.links-window');
            const child1 = document.querySelector('.links-list');
            const child2 = document.querySelector('.add-link-menu');
            if (child1.offsetHeight > child2.offsetHeight && !this.addLinkActive) {
                parent.style.height = `${child1.offsetHeight}px`;
            } else {
                parent.style.height = `${child2.offsetHeight}px`;
            }
        }
    },

    mounted() {
        this.calculateMenuHeight();
    }
    
}
    
</script>
