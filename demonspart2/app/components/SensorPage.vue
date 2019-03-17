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
        <Label text="Accelerometer"></Label>
        <Label id="x"/>
        <Label id="y"/>
        <Label id="z"/>
      </StackLayout>
      <StackLayout>
        <Button text="enable Location" @tap="enableLocationTap"/>
        <Button text="Get Current Location" @tap="buttonGetLocationTap"/>
        <Button text="Take Picture" @tap="takePicture"/>
        <Image :src="img" width="75" height="75"/>
      </StackLayout>
    </StackLayout>
  </Page>
</template>

<script >
import { EventData } from "data/observable";
import { Page } from "ui/page";

import { Observable } from "data/observable";
import {
  startAccelerometerUpdates,
  AccelerometerData
} from "nativescript-accelerometer";
import * as geolocation from "nativescript-geolocation";
import * as camera from "nativescript-camera";

export default {
  mounted() {
    console.log("MOUNTED SensorPage");
  },
  data: function() {
    return {
      msg: "Sensor Page",
      isListening: false,
      isEnabled: false,
      x: 1.0,
      y: 1.0,
      z: 1.0,
      img: ""
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
    enableLocationTap(args) {
      console.log("enableLocationTap SensorPage");
      geolocation.isEnabled().then(
        function(isEnabled) {
          if (!this.isEnabled) {
            geolocation.enableLocationRequest().then(
              function() {},
              function(e) {
                console.log("Error: " + (e.message || e));
              }
            );
          }
        },
        function(e) {
          console.log("Error: " + (e.message || e));
        }
      );
    },
    buttonGetLocationTap(args) {
      console.log("buttonGetLocationTap SensorPage");
      var location = geolocation
        .getCurrentLocation({
          desiredAccuracy: 3,
          updateDistance: 10,
          maximumAge: 20000,
          timeout: 20000
        })
        .then(
          function(loc) {
            if (loc) {
              console.log("Current location is: " + loc.longitude);
            }
          },
          function(e) {
            console.log("Error: " + e.message);
          }
        );
    },
    takePicture() {
      camera
        .requestPermissions()
        .then(() => {
          camera
            .takePicture({
              width: 300,
              height: 300,
              keepAspectRatio: true,
              saveToGallery: true
            })
            .then(imageAsset => {
              this.img = imageAsset;
            })
            .catch(e => {
              console.log("error:", e);
            });
        })
        .catch(e => {
          console.log("Error requesting permission");
        });
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
