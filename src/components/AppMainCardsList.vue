<script>
    import {store} from '../store.js'
    import AppMainCard from './AppMainCard.vue'

    export default {
        name:'AppMainCardsList',
        components:{
            AppMainCard,
        },
        data() {
            return {
                store,
            }
        }
    }
</script>

<template>
    <div class="cards-container">
        <AppMainCard 
            v-for="card in store.cardsRaw"
            :cardName="card.name"
            :cardArchetype="card.archetype"
            :cardImgSrc="card.card_images[0].image_url"/>
    </div>
</template>

<style lang="scss" scoped>
    @use '../style/partials/variables' as *;

    h4{
        background-color: $card-found-bg;
        color: white;
        padding: 1.2rem;
    }
    .cards-container{
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
    }
</style>