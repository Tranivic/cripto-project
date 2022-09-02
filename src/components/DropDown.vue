<template >
    <li>{{ drop.name }}</li>
</template>

<script>
export default {
    props: ['drop'],
    data() {
        return {

        }
    },
}
</script>

<style>
.drop-list {
    margin-top: -.4rem;
    height: 100%;
}
</style>