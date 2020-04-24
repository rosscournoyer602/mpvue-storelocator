<template>
  <div class="page">
   <map id="map" 
   class="page__map"
   :scale="scale"
   :latitude="latitude"
   :longitude="longitude"
   enable-zoom="false"
   enable-rotate="false"
   :markers="markers"
   @markertap="markerTap"
   ></map>
   <div v-if="storeDisplay" class="page__store-info">
     <h2 class="page__store-info-title">{{ storeDisplay.name }}</h2>
     <p class="page__store-info-field">Address: {{ storeDisplay.address }}</p>
     <p class="page__store-info-field">Hours: {{ storeDisplay.hours }}</p>
     <p class="page__store-info-field">Phone: {{ storeDisplay.phone }}</p>
   </div>
  </div>
</template>

<script>
// Use Vuex
import store from './store'

export default {
  mounted() {
    this.mapCtx = wx.createMapContext('map');
    this.mapCtx.includePoints({
      padding: [30],
      points: [{
        latitude: 31.1686,
        longitude: 121.5265,
      }, {
        latitude: 31.1128,
        longitude: 121.4817,
      },
      {
        latitude: 31.235556,
        longitude: 121.607935,
      }
      ]
    });
    this.mapCtx.getRegion({
      success(sw, ne) {
        console.log(sw, ne)
      }
    })
  },
  data() {
    return {
      scale:"",
      latitude: "",
      longitude: "",
      storeDisplay: null,
      markers: [
        {
          iconPath: "/static/images/mapPin.png",
          id: 0,
          data: {name: 'Xuhui Store', address: '165 Changshu Lu', hours: 'Tue-Sat 10AM-6PM', phone: '1281791514'},
          latitude: 31.1686,
          longitude: 121.5265,
          width: 20,
          height: 20,
          markerTap: this.markerTap
        },
        {
          iconPath: "/static/images/mapPin.png",
          id: 1,
          data: {name: 'Minhang Store', address: '66 Xianfeng Lu', hours: 'Mon-Fri 9AM-5PM', phone: '13817915142'},
          latitude: 31.1128,
          longitude: 121.4817,
          width: 20,
          height: 20
        },
        {
          iconPath: "/static/images/mapPin.png",
          id: 2,
          data: {name: 'Pudong Store', address: '501 Yincheng Middle Rd', hours: 'Mon-Fri 9AM-5PM', phone: '11827915142'},
          latitude: 31.235556,
          longitude: 121.607935,
          width: 20,
          height: 20
      }],
    }
  },
  methods: {
    markerTap(e) {
      const tappedMarker = this.markers.filter(marker => (e.mp.markerId === marker.id))[0];
      const { latitude, longitude } = tappedMarker;
      const { name, address, hours, phone } = tappedMarker.data;
      this.storeDisplay = { name, address, hours, phone };
      const coords = { latitude, longitude }
      this.latitude = latitude;
      this.longitude = longitude;
    }
  }
}
</script>

<style>
.page {
  text-align: center;
  margin-top: 20px;
}
.page__map {
  top: 0px;
  height: 30vh;
  width: 100%;
}
.page__store-info {
  margin-top: 30px;
}
.page__store-info-title {
  margin-bottom: 10px;
  font-size: 25px;
}
.page__store-info-field {
  margin-bottom: 5rpx;
  font-size: 18px;
}
.home {
  display: inline-block;
  margin: 100px auto;
  padding: 5px 10px;
  color: blue;
  border: 1px solid blue;
}
</style>
