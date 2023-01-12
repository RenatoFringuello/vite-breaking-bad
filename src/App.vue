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
        nPageToGet: 25,
        nCardFound: 0,
      }
    },
    methods: {
      getData(archetype) {
        console.log(archetype);
        axios.get(this.apiUrl, {
            params: {
              sort: "name",
              archetype : archetype,
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
        });
      },
    },
    created() {
      this.getData(undefined);
    },
}
</script>

<template>
  <AppHeader />
  <AppMain :nCardFound="nCardFound" @filterData="getData"/>
</template>

<style lang="scss">
  @use './style/general.scss' as *;

</style>
