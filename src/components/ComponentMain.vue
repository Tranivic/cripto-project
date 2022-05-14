<template>
    <main>
        <div class="pt-5 justify-center flex search-bar">
            <input placeholder="Search for currency..."
                class="bg-zinc-100 shadow-lg max-w-2xl w-3/5 py-3 pl-5 rounded-lg" type="search" required>
            <button></button>
        </div>
    </main>
</template>

<script>
export default {

}
</script>

<style>
input {
    transition: 0.2s;
}

input:focus,
input:valid {
    color: #fff;
    background-color: #313131;
}
</style>