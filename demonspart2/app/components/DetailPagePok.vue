<template>
  <Page @loaded="onLoaded" @navigatedTo="onNavigatedTo">
    <ActionBar title="Demo NS part 3">
      <NavigationButton text="Go back" @tap="$navigateBack" android.systemIcon="ic_menu_back"/>
      <ActionItem
        ios.systemIcon="2"
        ios.position="right"
        android.systemIcon="ic_menu_info_details"
      />
    </ActionBar>
    <GridLayout rows="*, 8*" column="*, *">
      <GridLayout row="0" col="0">
        <Label :text="this.pokemon.name" class="pokName" backgroundColor="#43b883"/>
        <Switch
          backgroundColor="#1c6b48"
          @loaded="onLoaded"
          :checked="isFavPok"
          @checkedChange="onCheckedChange"
        ></Switch>
      </GridLayout>
      <GridLayout row="1">
        <ListView for="sprite in pokSpritesURL" class="list-group" heigth="100%">
          <v-template>
            <StackLayout orientation="horizontal" class="list-group-item">
              <Image col="0" :src="sprite" class="spriteImg"></Image>
              <Label col="1" :text="sprite"/>
            </StackLayout>
          </v-template>
        </ListView>
      </GridLayout>
    </GridLayout>
  </Page>
</template>

<script >
import * as http from "http";
import * as localStorage from "nativescript-localstorage";

export default {
  props: ["pok"],

  mounted() {
    console.log("DetailPagePok mounted");

    //localStorage.clear();
    // aller chercher les informations pour ce pokemon en particulier
    http.getJSON(this.pok.url.toString()).then(
      result => {
        // placer les informations du pokemon dans un tableau
        this.pokemon = result;
        console.log("http.getJSON this.pokemon" + JSON.stringify(this.pokemon));
        // aller chercher toutes les images de ce pokemon particulier
        this.getSprites(this.pokemon);
      },
      error => {
        console.log(error);
        console.log("erreur load json");
      }
    );
    var storedList = JSON.parse(localStorage.getItem("favPokList"));
    this.favPokList = storedList;

    // s'il y a des pokemons dans le local storage
    if (this.favPokList !== null) {
      for (var i = 0; i < this.favPokList.length; i++) {
        //console.log("this.favPokList[i].name = " + this.favPokList[i].name);
        //console.log("this.pokemon.name = " + JSON.stringify(this.pokemon));
        if (this.pok.name.localeCompare(this.favPokList[i].name) == 0) {
          this.isFavPok = true;
          console.log(this.isFavPok);
        } else {
          this.isFavPok = false;
          console.log("this.isFavPok = " + this.isFavPok);
        }
      }
    }
  },

  data() {
    return {
      msg: "Detail Page",
      pokemon: [],
      pokSpritesURL: [],
      favPokList: [],
      isFavPok: false
    };
  },
  methods: {
    onNavigatedTo(args) {
      console.log("onNavigatedTo in methods");
      //console.log(JSON.stringify(this.pok.url.toString()));
    },
    onCheckedChange(event) {
      // si toggle a ON et qu'on le met a OFF pour enlever un pokemon de la liste
      if (this.isFavPok == true) {
        //retrouver la liste dans le local storage
        var storedList = JSON.parse(localStorage.getItem("favPokList"));
        this.favPokList = storedList;

        // chercher le pokemon a supprimer de la liste
        for (var i = 0; i < this.favPokList.length; i++) {
          if (this.favPokList[i].name == this.pok.name) {
            this.favPokList.splice(i);
          }
        }
        // recreer la liste avec le pokemon supprime et re-enregistrer la liste
        var test = JSON.stringify(this.favPokList);
        localStorage.setItem("favPokList", test);
      } else {
        // si toggle a OFF et qu'on le change a ON pour ajouter un pokemon aux favoris

        var pokList = [];

        // s'il y a deja des elements dans la liste des favoris copier la liste
        if (this.favPokList) {
          pokList = this.favPokList;
        }
        console.log("pokList = " + pokList);
        console.log("name = " + this.pok.name + " url = " + this.pok.url);
        // ajouter l'element
        pokList.push({
          name: this.pok.name,
          url: this.pok.url
        });

        // re-enregistrer la liste dans le local storage et mettre a jour la variable de liste pour l'affichage
        var test = JSON.stringify(pokList);
        localStorage.setItem("favPokList", test);
        this.favPokList = pokList;
      }
    },
    onLoaded() {
      console.log("onLoaded in methods");
    },
    /* getSprites(args)
     * YC march 13 2019
     *
     * params :
     *    args : JSON object pokemon:{name, url}
     *
     * return:
     *    sprites: array of images url
     *
     * description:
     *    get all sprites url for a specified pokemon
     */
    getSprites(args) {
      console.log("getSprites in function " + args.sprites.back_default);
      var spritesURL = [];

      // push les sprites dans un tableau 2 dimensions
      for (var i in args.sprites) {
        spritesURL.push([i, args.sprites[i]]);
      }

      for (var i = 0; i < spritesURL.length; i++) {
        for (var j = 0; j < spritesURL[0].length; j++) {
          // debug : afficher console les url de chaque sprite disponible pour ce pokemon
          //console.log(
          //  "spritesURL[" + i + "][ " + j + "] = " + spritesURL[i][j]
          //);
        }
        // certains pokemons ne possedent pas toutes les images par defaut
        // je mets dans un tableau tous les sprites qui ne sont pas null
        // sert pour l'affichage: sinon le listview a des lignes vides
        if (spritesURL[i][1] !== null) {
          this.pokSpritesURL.push(spritesURL[i][1]);
          //console.log("pokeSpriteURL = " + this.pokSpritesURL[i]);
        }
      }
    }
  }
};
</script>

<style scoped>
ActionBar {
  background-color: #53ba82;
  color: #ffffff;
}

.message {
  vertical-align: center;
  text-align: center;
  font-size: 20;
  color: #333333;
}
.spriteImg {
  border-width: 1px;
  border-color: black;
  height: auto;
  width: 35%;
}
.pokName {
  font-size: 30vw;
}
</style>
