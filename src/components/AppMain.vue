<script>
    import axios from 'axios'
    import {store} from '../store.js'
    import AppMainCardsList from './AppMainCardsList.vue'

    export default {
        name: "AppMain",
        components: { 
            AppMainCardsList, 
        },
        data() {
            return {
                store,
                apiUrl: "https://db.ygoprodeck.com/api/v7/cardinfo.php",
                nPage: 0,
                nPageToGet: 25,
                nCardFound: 0,
                archetypes:['-- All --', 'Alien', 'Laval', 'Vylon', 'Inzektor', 'Umi', 'Gusto'],
                archetypeSelected:undefined,
            };
        },
        methods: {
            getData() {
                axios.get(this.apiUrl, {
                    params: {
                        sort: "name",
                        archetype : this.archetypeSelected,
                        num: this.nPageToGet,
                        offset: this.nPageToGet * this.nPage
                    }
                })
                .then((response) => {
                    this.store.cardsRaw = response.data.data;
                })
                .catch((error) => {
                    console.error(`something went wrong :${error}`);
                })
                .then(() => {
                    this.nCardFound = this.store.cardsRaw.length;
                    console.log(this.store.cardsRaw);
                });
            },
        },
        created() {
            this.getData();
        },
    }
</script>

<template>
    <main>
        <div class="container">
            <div class="select-container">
                <select v-model="archetypeSelected" @change="getData()">
                    <option v-for="archetype,i in archetypes" :value="(i===0) ? undefined : archetype">{{ archetype }}</option>
                </select>
            </div>
            <section>
                <h4>Found {{ nCardFound }} cards</h4>
                <!-- list -->
                <AppMainCardsList :archetype="archetypeSelected"/>
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
        }
        section{
            background-color: white;
            padding: 3rem 3rem 2rem;
        }
    }
</style>