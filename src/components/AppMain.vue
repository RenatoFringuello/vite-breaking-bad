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
                apiUrl : 'https://db.ygoprodeck.com/api/v7/cardinfo.php',
                nPage:0,
                nPageToGet:25,
                // nMaxPage : 0,
                nCardFound : 0,
                // archetypes:['-- All --', '-- No Archetype --'],
                // archetypeSelected:'',
            }
        },
        methods: {
            getData(){
                axios.get(this.apiUrl,{
                        params:{
                            sort:'name',
                            num:this.nPageToGet,
                            offset:this.nPageToGet * this.nPage
                        }
                    })
                    .then((response)=>{
                        // this.nMaxPage = Math.floor(response.data.meta.total_rows / this.nPageToGet);
                        // // da rimuovere
                        // this.archetypes = ['-- All --', '-- No Archetype --'];
                        this.store.cardsRaw = response.data.data;

                        // //replace this with a real getArchetypes
                        // this.store.cardsRaw.forEach((card)=>{
                        //     let archetype = card.archetype;
                        //     if(archetype === undefined){
                        //         archetype = this.archetypes[1];
                        //     }
                        //     if(!this.archetypes.includes(archetype)){
                        //         this.archetypes.push(archetype);
                        //     }
                        // });
                    })
                    .catch((error)=>{
                        console.error(`something went wrong :${error}`);
                    })
                    .then(()=>{
                        // this.archetypes.sort();
                        this.nCardFound = this.store.cardsRaw.length;
                    })

            },
            // setArchetype(archetype){
            //     //set archetype to undefined if '-- none --' or empty string if '-- all --'
            //     this.archetypeSelected = (archetype === this.archetypes[1]) ? undefined : (archetype === this.archetypes[0]) ? '' : archetype;
            //     // filter by archetype to get the number 
            //     const cards = this.store.cardsRaw.filter(card => (card.archetype === this.archetypeSelected));
            //     //set the number to max if '-- all --' selected
            //     this.nCardFound = (this.archetypeSelected === '') ? this.store.cardsRaw.length : cards.length;
            // },
            // setPage(direction){
            //     this.nPage += (direction === 'prev') ? (this.nPage === 0) ? this.nMaxPage : -1 : (this.nPage === this.nMaxPage) ? -this.nMaxPage : 1;
            //     this.getData();
            // }
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
                <select>
                    <option v-for="archetype in archetypes" :value="archetype"
                            @change="setArchetype(archetype)">{{ archetype }}</option>
                </select>
                <!-- <div class="set-page-container">
                    <button @click="setPage('prev')">Prev</button>
                    <span>{{ nPage }}</span>
                    <button @click="setPage('next')">Next</button>
                </div> -->
            </div>
            <section>
                <h4>Found {{ nCardFound }} cards</h4>
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
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding:1.5rem;
            select{
                width: 150px;
                padding: .5rem;
            }
            .set-page-container{
                *{
                    margin-left: 1rem;
                }
                
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