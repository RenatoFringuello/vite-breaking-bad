<script>
    export default {
        name:'AppMainCard',
        props:{
            cardName: String,
            cardArchetype: String,
            cardImgSrc: String,
        }
    }
</script>

<template>
    <div class="card">
        <img :src="cardImgSrc" :alt="cardName">
        <div class="card-title"><h3>{{ cardName }}</h3></div>
        <div class="card-type">{{ cardArchetype }}</div>
    </div>
</template>

<style lang="scss" scoped>
    @use '../style/partials/variables' as *;
    .card{
        width: calc((100% / 5) - 1rem);
        background-color: $main-bg;
        padding-bottom: 1rem;
        margin-bottom: 1rem;
        img{
            width: 100%;
            object-fit: contain;
        }
        div{
            text-align: center;
            padding-top: 1rem;


            &.card-title{
                color: white;
            }
        }
    }
</style>