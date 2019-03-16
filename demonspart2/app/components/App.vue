<script src="http://localhost:8098"></script>
<template>
  <Page>
    <ActionBar title="Demo NS part 3">
      <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="goBack"/>
      <ActionItem
        @tap="onTapEdit"
        v-show="!isEditing"
        ios.systemIcon="2"
        ios.position="right"
        android.systemIcon="ic_menu_info_details"
      />
    </ActionBar>
    <StackLayout>
      <Label class="message" :text="msg" col="0" row="0"/>
      <Button text="Go to Pokemon List" @tap="onButtonTapPok"></Button>
      <Button text="Go to Favorites Pokemons" @tap="onButtonTapFav"></Button>
    </StackLayout>
  </Page>
</template>

<script >
import listpagepok from "./ListPagePok";
import listpagefav from "./ListPageFav";

export default {
  data() {
    return {
      msg: "Hello World!"
    };
  },
  methods: {
    //onButtonTap: function(event) {
    // console.log("Button tapped");
    //this.$navigateTo(navigationpage);
    //},

    onButtonTapPok: function(event) {
      console.log("Button tapped");
      this.$navigateTo(listpagepok);
    },
    onButtonTapFav: function(event) {
      console.log("Button tapped");
      this.$navigateTo(listpagefav);
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
</style>
