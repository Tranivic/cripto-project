<template >
    <li class="drop-item cursor-default h-full rounded-lg pl-5 pt-1" @mouseover="isHovering = true"
        @mouseout="isHovering = false" :class="{hovering: isHovering}" @click="passDrop(drop)">{{ drop.name }}</li>
</template>

<script>
export default {
    nCoins: { type: Array, required: true },
    props: ['drop'],
    
    data() {
        return {
            isHovering: false,
            coinClicked: {},
        }
    },
    

    methods: {

    toggleBgVisibility() {
        this.bgIsVisible = !this.bgIsVisible;
        console.log(this.bgIsVisible)
    },
   
    // Function to push the coin on click
    passDrop(dropSelected)  {
        this.$emit('drop-clicked', dropSelected);
    },

    },
}
</script>

<style>
.drop-list {
    margin-top: -.4rem;
}

/* Javascript related - Hover in autocomplete */
.hovering {
    background-color: rgb(168 162 158);
}
</style>