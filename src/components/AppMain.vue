<script>
    import AppMainCardsList from './AppMainCardsList.vue'

    export default {
        name: "AppMain",
        components: { 
            AppMainCardsList, 
        },
        props:{
            nCardFound:Number,
            nPage:Number,
        },
        data() {
            return {
                archetypes:['-- All --', 'Alien', 'Laval', 'Vylon', 'Inzektor', 'Umi', 'Gusto'],
                archetypeSelected:undefined,
            };
        }
    }
</script>

<template>
    <main>
        <div class="container">
            <div class="select-container">
                <select v-model="archetypeSelected" @change="$emit('filterData', archetypeSelected)">
                    <option v-for="archetype,i in archetypes" :value="(i===0) ? undefined : archetype">{{ archetype }}</option>
                </select>
                <div class="set-page-container">
                    <button @click="$emit('changePage', -1)">Prev</button>
                    <h4>{{nPage}}</h4>
                    <button @click="$emit('changePage', 1)">Next</button>
                </div>
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
            .set-page-container{
                display: flex;
                align-items: center;
                *{
                    margin-left:1rem;
                }
                h4{
                    text-align: center;
                    width: 50px;
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
        }
    }
</style>