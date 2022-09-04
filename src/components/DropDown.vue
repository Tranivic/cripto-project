<template >
    <li class="drop-item cursor-default h-full rounded-md pl-5 pt-1" @mouseover="isHovering = true" @mouseout="isHovering = false"
        :class="{hovering: isHovering}" @click="passDrop(drop.name)">{{ drop.name }}</li>
</template>

<script>
export default {
    props: [
        'drop', 'newCoins'
    ],
    data() {
        return {
            isHovering: false,
        }
    },
    

    methods: {
    toggleBgVisibility() {
      this.bgIsVisible = !this.bgIsVisible;
      console.log(this.bgIsVisible)
        },
    },
    setup (){
        // Function to push the coin on click
        const passDrop = (dropName) => {
            console.log(dropName)
        }
        return {
            passDrop,
        };
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