<template>
  <Page @loaded="onLoaded" @navigatedTo="onNavigatedTo">
    <ActionBar title="My App">
      <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="goBack"/>
    </ActionBar>
    <ActionBar title="Demo NS part 3">
      <NavigationButton text="Go back" @tap="$navigateBack" android.systemIcon="ic_menu_back"/>
      <ActionItem
        ios.systemIcon="2"
        ios.position="right"
        android.systemIcon="ic_menu_info_details"
      />
    </ActionBar>
    <GridLayout>
      <GridLayout row="0" col="0">
        <Label :text="this.pokemon.name" class="pokName"/>
      </GridLayout>
      <GridLayout row="1" col="0">
        <ListView for="sprite in pokSpritesURL" class="list-group" heigth="100%">
          <v-template>
            <StackLayout orientation="horizontal" class="list-group-item">
              <Image :src="sprite" class="spriteImg"></Image>
              <Label :text="sprite"/>
            </StackLayout>
          </v-template>
        </ListView>
      </GridLayout>
    </GridLayout>
  </Page>
</template>

<script >
import * as http from "http";

export default {
  props: ["pok"],

  mounted() {
    console.log("DetailPage mounted");

    http.getJSON(this.pok.url.toString()).then(
      result => {
        this.pokemon = result;
        //console.log("mounted " + JSON.stringify(this.pokemon));
        console.log("sprites: " + this.pokemon.sprites.size);
        this.getSprites(this.pokemon);
      },
      error => {
        console.log(error);
        console.log("erreur load json");
      }
    );
  },

  data() {
    return {
      msg: "Detail Page",
      pokemon: [],
      pokSpritesURL: []
    };
  },
  methods: {
    onNavigatedTo(args) {
      console.log("onNavigatedTo in methods");
      //console.log(JSON.stringify(this.pok.url.toString()));
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
      console.log("getSprites in function");
      var spritesURL = [];

      console.log(" args = " + JSON.stringify(args.sprites));
      // push les sprites dans un tableau 2 dimensions
      for (var i in args.sprites) {
        spritesURL.push([i, args.sprites[i]]);
        console.log("test");
      }
      console.log("after for");
      for (var i = 0; i < spritesURL.length; i++) {
        for (var j = 0; j < spritesURL[0].length; j++) {
          // debug : afficher console les url de chaque sprite disponible pour ce pokemon
          //console.log(
          //  "spritesURL[" + i + "][ " + j + "] = " + spritesURL[i][j]
          //);
        }
        if (spritesURL[i][1] !== null) {
          this.pokSpritesURL.push(spritesURL[i][1]);
          //console.log("pokeSpriteURL = " + this.pokSpritesURL[i]);
        }
      }
      //for (var i = 0; i < this.pokSpritesURL.length; i++) {
      //console.log("i = " + this.pokSpritesURL[i]);
      //}
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
