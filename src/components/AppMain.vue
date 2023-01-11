<script>
    import AppMainCard from './AppMainCard.vue'
    import axios from 'axios'
    import {store} from '../store.js'

    export default {
        name:'AppMain',
        components:{
            AppMainCard,
        },
        data() {
            return {
                store,
            }
        },
        methods: {
            getData(){
                axios.get('https://db.ygoprodeck.com/api/v7/cardinfo.php?num=30&offset=0')
                     .then((response)=>{
                        store.cardsRaw = response.data.data;
                     })
                     .catch((error)=>{
                        console.error(`something went wrong :${error}`);
                     })

            }
        },
        created(){
            this.getData();
        }
    }
</script>

<template>
    <main>
        <div class="container">
            <div class="select-container">
                <select name="" id=""></select>
            </div>
            <section>
                <h4>Found {{ store.cardsRaw.length }} cards</h4>
                <div class="cards-container">
                    <AppMainCard 
                        v-for="card in store.cardsRaw"
                        :cardName="card.name"
                        :cardArchetype="card.archetype"
                        :cardImgSrc="card.card_images[0].image_url"/>
                </div>
            </section>
        </div>
    </main>

</template>

<style lang="scss" scoped>
    @use '../style/partials/variables' as *;

    main{
        background: $main-bg;
        padding-bottom: 3rem;

        .select-container{
            padding:1.5rem;
            select{
                width: 150px;
                padding: .5rem;
            }
        }
        section{
            background-color: white;
            padding: 3rem 3rem 2rem;
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
        }
    }
</style>