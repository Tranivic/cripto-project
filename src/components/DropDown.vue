<template >
<ul id="drop-list" class="drop-list bg-stone-800 text-white shadow-lg max-w-2xl w-3/5 py-3 pl-5 rounded-lg rounded-t-none text-left">
    <li>Teste</li>
</ul>
</template>

<script>
export default {
    props: ['drop-list'],
    data() {
        return {
         
        }
    },
}
</script>

<style>
    .drop-list{
        margin-top: -.4rem;
        height: 100%;
    }
</style>