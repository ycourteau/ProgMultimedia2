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
      <Button text="Go to List" @tap="onButtonTap"></Button>
    </StackLayout>
  </Page>
</template>

<script >
import listpage from "./ListPage";

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

    onButtonTap: function(event) {
      console.log("Button tapped");
      this.$navigateTo(listpage);
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
