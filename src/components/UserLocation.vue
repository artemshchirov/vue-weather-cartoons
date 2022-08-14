<template>
  <div id="user-location" v-cloak>
    <div v-if="errorStr">
      Sorry, but the following error occurred: {{ errorStr }}
    </div>

    <div v-if="gettingLocation">
      <i>Getting your location...</i>
    </div>

    <div v-if="location">
      Your location data is {{ location.coords.latitude }},
      {{ location.coords.longitude }}
    </div>

  </div>
</template>

<script>


export default {
  el: '#user-location',
  data() {
    return {
      location: null,
      gettingLocation: false,
      errorStr: null,
    };
  },
  created() {

    if (!('geolocation' in navigator)) {
      this.errorStr = 'Geolocation is not available.';
      return;
    }

    this.gettingLocation = true;
    navigator.geolocation.getCurrentPosition(
      (pos) => {
        this.gettingLocation = false;
        this.location = pos;
      },
      (err) => {
        this.gettingLocation = false;
        this.errorStr = err.message;
      }
    );
  },
};
</script>

<style>
* {
  color: white;
}
</style>
