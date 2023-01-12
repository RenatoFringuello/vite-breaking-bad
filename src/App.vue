<script>
  import axios from 'axios'
  import {store} from './store.js'

  import AppHeader from './components/AppHeader.vue';
  import AppMain from './components/AppMain.vue';
  export default{
    components: { 
      AppHeader,
      AppMain,
    },
    data() {
      return {
        store,
        apiUrl: "https://db.ygoprodeck.com/api/v7/cardinfo.php",
        nPage: 0,
        nMaxPage: 0,
        nPageToGet: 10,
        nCardFound: 0,
        archetypeSelected:undefined,
      }
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
          this.store.cardsMetaRaw = response.data.meta;
        })
        .catch((error) => {
          console.error(`something went wrong :${error}`);
        })
        .then(() => {
          this.nCardFound = this.store.cardsRaw.length;
          this.nMaxPage = Math.floor(this.store.cardsMetaRaw.total_rows / this.nPageToGet);
        });
      },
      setArchetypeSelected(archetype){
        this.archetypeSelected = archetype;
        this.nPage = 0;
        this.getData();
      },
      setPage(direction){
        this.nPage += direction;
        this.nPage = (this.nPage < 0) ? this.nMaxPage : (this.nPage > this.nMaxPage) ? 0 : this.nPage;        
        this.getData();
      }
    },
    created() {
      this.getData(undefined);
    },
  }
</script>

<template>
  <AppHeader />
  <AppMain 
    :nCardFound="nCardFound" 
    :nPage="nPage" 
    @filterData="setArchetypeSelected"
    @changePage="setPage"/>
</template>

<style lang="scss">
  @use './style/general.scss' as *;

</style>
