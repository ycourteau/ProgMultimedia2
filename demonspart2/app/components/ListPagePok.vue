<template>
  <Page @navigatedTo="onNavigatedTo">
    <ActionBar title="Demo NS part 3">
      <NavigationButton text="Go back" android.systemIcon="ic_menu_back"/>
      <ActionItem
        ios.systemIcon="2"
        ios.position="right"
        android.systemIcon="ic_menu_info_details"
      />
    </ActionBar>
    <StackLayout>
      <SearchBar @textChange="onTextChanged" @submit="onSubmit"/>
      <ListView for="poke in pokemon" class="list-group" @itemTap="onItemTap">
        <v-template>
          <StackLayout class="list-group-item">
            <Label :text="poke.url"/>
            <Label :text="poke.name"/>
          </StackLayout>
        </v-template>
        <v-template if="poke.isFavorite">
          <StackLayout class="list-group-item" color="green">
            <Label :text="poke.url"/>
            <Label :text="poke.name"/>
          </StackLayout>
        </v-template>
      </ListView>
    </StackLayout>
  </Page>
</template>
<script>
import DetailPagePok from "./DetailPagePok";
import * as http from "http";

export default {
  props: ["name", "url"],

  mounted() {
    // recevoir la liste des pokemons (objet JSON) a partir d<un url
    http.getJSON("https://pokeapi.co/api/v2/pokemon/?limit=50").then(
      result => {
        this.pokemon = result.results;
        this.checkIsFavorite();
      },
      error => {
        console.log(error);
      }
    );
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
      //this.checkIsFavorite();
    },
    checkIsFavorite() {
      if (localStorage.length != 0) {
        var storedList = JSON.parse(localStorage.getItem("favPokList"));
        this.favPokList = storedList;
        console.log("this.favPokList = " + JSON.stringify(this.favPokList));

        // si la liste des pokemons favoris n<Est pas vide
        if (this.favPokList.length != 0) {
          //parcourir tous les pokemons
          for (var i = 0; i < this.pokemon.length; i++) {
            for (var j = 0; j < this.favPokList.length; j++) {
              // si un pokemon fait parti de la liste des favoris
              if (this.pokemon[i].name == this.favPokList[j].name) {
                console.log(
                  " checkIsFavorite : GET ITEM = " +
                    JSON.stringify(this.favPokList[j])
                );
                // ajouter la propriete "isFavorite a l'objet"
                Object.assign(this.pokemon[i], { isFavorite: true });
                console.log(
                  "this.pokemon[i] = " + JSON.stringify(this.pokemon[i])
                );
              }
            }
          }
        }
      }
    },
    onItemTap({ index, e }) {
      var myPok = this.pokemon[index];
      //console.log("ListPage " + this.pokemon[index]);
      //console.log(this.pokemon[index].name);

      // navigation a la page details
      this.$navigateTo(DetailPagePok, {
        props: {
          pok: this.pokemon[index]
        }
      });
    },
    onTextChanged(args) {
      console.log(args.value);
      this.searchedText = args.value;
    },
    onSubmit() {
      console.log("submitted text is", this.searchedText);
    }
  }
};
</script>
<style>
</style>


