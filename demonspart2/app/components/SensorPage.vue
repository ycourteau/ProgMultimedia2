<template>
  <Page @navigatedTo="onNavigatedTo" @loaded="onLoaded">
    <ActionBar title="Demo NS part 3">
      <NavigationButton text="Go back" android.systemIcon="ic_menu_back"/>
      <ActionItem
        ios.systemIcon="2"
        ios.position="right"
        android.systemIcon="ic_menu_info_details"
      />
    </ActionBar>
    <StackLayout>
      <Label class="message" :text="msg" col="0" row="0"/>
      <StackLayout>
        <Label id="x"/>
        <Label id="y"/>
        <Label id="z"/>
      </StackLayout>
    </StackLayout>
  </Page>
</template>

<script >
import { EventData } from "data/observable";
import { Page } from "ui/page";
import {
  startAccelerometerUpdates,
  AccelerometerData
} from "nativescript-accelerometer";
import { Observable } from "data/observable";

export default {
  mounted() {
    console.log("MOUNTED SensorPage");
  },
  data: function() {
    return {
      msg: "Sensor Page",
      isListening: false,
      x: 1.0,
      y: 1.0,
      z: 1.0
    };
  },
  methods: {
    onNavigatedTo(args) {
      console.log("onNavigatedTo SensorPage " + this.x);
      // Get the event sender
      let page = args.object;
      let context = new Observable({
        myX: 0,
        myY: 0,
        myZ: 0
      });
      page.bindingContext = context;

      if (!this.isListening) {
        this.isListening = true;
        startAccelerometerUpdates(
          function(data) {
            let myLabelx = page.getViewById("x");
            let myLabely = page.getViewById("y");
            let myLabelz = page.getViewById("z");
            myLabelx.text = data.x;
            myLabely.text = data.y;
            myLabelz.text = data.z;

            //console.log("x: " + data.x + "y: " + data.y + "z: " + data.z);
          },
          { sensorDelay: "ui" }
        );
      }
    },
    onLoaded() {
      console.log("ONLOADED SensorPage");
    },
    getSensor: function(event) {
      console.log("Get Sensor");
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
