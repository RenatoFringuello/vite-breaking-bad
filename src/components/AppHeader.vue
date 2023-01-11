<script>
    export default {
        name:'AppHeader',
    }
</script>

<template>
    <header>
        <img src="../assets/logo.webp" alt="yu-gi-oh logo">
        <span>Yu-Gi-Oh API</span>
    </header>
</template>

<style lang="scss" scoped>
    header{
        padding: 1rem 1rem 0;
        img{
            width: 100px;
            padding-bottom: 1rem;
            vertical-align: middle;
        }
        span{
            font-size: 2.5rem;
        }
    }
</style>