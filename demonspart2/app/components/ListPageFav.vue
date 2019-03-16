<template>
  <Page @navigatedTo="onNavigatedTo">
    <ActionBar title="Fav List">
      <NavigationButton text="Go back" android.systemIcon="ic_menu_back"/>
      <ActionItem
        ios.systemIcon="2"
        ios.position="right"
        android.systemIcon="ic_menu_info_details"
      />
    </ActionBar>
    <StackLayout>
      <SearchBar @textChange="onTextChanged"/>
      <ListView
        for="poke in favPokList"
        class="list-group"
        @itemTap="onItemTap"
        @longPress="onLongPress"
      >
        <v-template>
          <StackLayout class="list-group-item">
            <Label :text="poke.name"/>
            <Label :text="poke.url"/>
          </StackLayout>
        </v-template>
      </ListView>
    </StackLayout>
  </Page>
</template>
<script>
import DetailPagePok from "./DetailPagePok";
import * as http from "http";
import * as localStorage from "nativescript-localstorage";

export default {
  props: ["name", "url"],

  mounted() {
    // debug dummy favorite pokemon list
    //localStorage.clear();
    /*var pokList = [];
    pokList.push({
      name: "bulbasaur",
      url: "https://pokeapi.co/api/v2/pokemon/1/"
    });
    pokList.push({
      name: "venusaur",
      url: "https://pokeapi.co/api/v2/pokemon/3/"
    });
    pokList.push({
      name: "charmeleon",
      url: "https://pokeapi.co/api/v2/pokemon/5/"
    });
    pokList.push({
      name: "squirtle",
      url: "https://pokeapi.co/api/v2/pokemon/7/"
    });
    
    var test = JSON.stringify(pokList);
    localStorage.setItem("favPokList", test);
*/
    // debug : faire afficher la liste des pokemons favoris
    /*  if (this.favPokList.length != null) {
      for (var i = 0; i < this.favPokList.length; i++) {
        console.log(
          " MOUNTED : GET ITEM = " + JSON.stringify(this.favPokList[i])
        );
      }
    } */
  },

  data() {
    return {
      msg: "ListPage",
      searchedText: "",
      pokemon: [],
      favPokList: []
    };
  },

  methods: {
    onNavigatedTo() {
      console.log("ListPageFav onNavigatedTo");
      if (localStorage.length != 0) {
        var storedList = JSON.parse(localStorage.getItem("favPokList"));
        this.favPokList = storedList;

        // debug : faire afficher la liste des pokemons favoris
        /*  if (this.favPokList.length != null) {
      for (var i = 0; i < this.favPokList.length; i++) {
        console.log(
          " onNavigatedTo : GET ITEM = " + JSON.stringify(this.favPokList[i])
        );
      }
    } */
      }
    },
    onItemTap({ index, e }) {
      var myPok = this.favPokList[index];

      // navigation a la page details
      this.$navigateTo(DetailPagePok, {
        props: {
          pok: this.favPokList[index]
        }
      });
    },
    onLongPress(index, e) {
      console.log("onLongPress");
    },
    onTextChanged(args) {
      console.log(args.value);
      this.searchedText = args.value;
    }
  }
};
</script>
<style>
</style>


