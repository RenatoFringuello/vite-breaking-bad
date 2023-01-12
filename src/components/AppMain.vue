<script>
    import AppMainCardsList from './AppMainCardsList.vue'

    export default {
        name: "AppMain",
        components: { 
            AppMainCardsList, 
        },
        props:{
            nCardFound:Number,
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