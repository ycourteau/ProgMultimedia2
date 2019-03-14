<template>
  <Page>
    <ActionBar title="Demo NS part 3">
      <NavigationButton text="Go back" android.systemIcon="ic_menu_back"/>
      <ActionItem
        ios.systemIcon="2"
        ios.position="right"
        android.systemIcon="ic_menu_info_details"
      />
    </ActionBar>
    <StackLayout>
      <SearchBar v-model="searchQuery" @textChange="onTextChanged" @submit="onSubmit"/>
      <ListView for="poke in pokemon" class="list-group" @itemTap="onItemTap">
        <v-template>
          <StackLayout class="list-group-item">
            <Label :text="poke.url"/>
            <Label :text="poke.name"/>
          </StackLayout>
        </v-template>
      </ListView>
    </StackLayout>
  </Page>
</template>
<script>
import detailpage from "./DetailPage";
import * as http from "http";

export default {
  props: ["id", "url"],

  mounted() {
    // recevoir la liste des pokemons (objet JSON) a partir d<un url
    http.getJSON("https://pokeapi.co/api/v2/pokemon/?limit=50").then(
      result => {
        this.pokemon = result.results;
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
      pokemon: []
    };
  },

  methods: {
    onItemTap({ index, e }) {
      var myPok = this.pokemon[index];
      //console.log("ListPage " + this.pokemon[index]);
      //console.log(JSON.stringify(this.pokemon[index].url));

      // navigation a la page details
      this.$navigateTo(detailpage, {
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


