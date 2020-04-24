<template>
  <div class="page">
   <map id="map" 
   class="page__map"
   :scale="scale"
   :latitude="latitude"
   :longitude="longitude"
   show-location
   enable-zoom="false"
   enable-rotate="false"
   :markers="markers"
   @markertap="markerTap"
   ></map>
   <div 
    v-for="(store, storeIndex) in stores" 
    :class="[activeStore === store.id ? 'page__store-info--active' : 'page__store-info']"
    :key="storeIndex" 
    @click="selectStore(store)"
   >
    <h2 class="page__store-info-title">{{ store.data.name }}</h2>
    <p class="page__store-info-field">Address: {{ store.data.address }}</p>
    <p class="page__store-info-field">Hours: {{ store.data.hours }}</p>
    <p class="page__store-info-field">Phone: {{ store.data.phone }}</p>
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
      latitude: "",
      longitude: "",
      activeStore: null,
      markers: [{
        iconPath: "/static/images/mapPin.png",
        id: 0,
        latitude: null,
        longitude: null,
        width: 20,
        height: 30
      }],
      stores: [
        {
          id: 0,
          data: {name: 'Xuhui Store', address: '165 Changshu Lu', hours: 'Tue-Sat 10AM-6PM', phone: '1281791514'},
          latitude: 31.1686,
          longitude: 121.5265
        },
        {
          id: 1,
          data: {name: 'Minhang Store', address: '66 Xianfeng Lu', hours: 'Mon-Fri 9AM-5PM', phone: '13817915142'},
          latitude: 31.1128,
          longitude: 121.4817
        },
        {
          id: 2,
          data: {name: 'Pudong Store', address: '501 Yincheng Middle Rd', hours: 'Mon-Fri 9AM-5PM', phone: '11827915142'},
          latitude: 31.235556,
          longitude: 121.607935
      }]
    }
  },
  methods: {
    markerTap(e) {
      const tappedMarker = this.markers.filter(marker => (e.mp.markerId === marker.id))[0];
      const { latitude, longitude } = tappedMarker;
      const { name, address, hours, phone } = tappedMarker.data;
      this.storeDisplay = { name, address, hours, phone };
      const coords = { latitude, longitude }
      this.mapCtx.translateMarker({

      });
    },
    selectStore(store) {
      this.activeStore = store.id;
      this.markers[0].latitude = store.latitude;
      this.markers[0].longitude = store.longitude;
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
  width: 90%;
  margin: 15px auto 0px auto;
  box-shadow: 0px 0px 3px 1px rgba(0,0,0,0.75);
}
.page__store-info--active {
  width: 90%;
  margin: 15px auto 0px auto;
  background-color: #dcdcdc;
  box-shadow: 0px 0px 3px 1px rgba(0,0,0,0.75);
}
.page__store-info-title {
  margin-bottom: 10px;
  font-size: 18px;
}
.page__store-info-field {
  margin-bottom: 5rpx;
  font-size: 16px;
}
</style>
