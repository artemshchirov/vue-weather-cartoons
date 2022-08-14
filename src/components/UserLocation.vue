<template>
  <div id="user-location" v-cloak>
    <div v-if="errorStr">
      Sorry, but the following error occurred: {{ errorStr }}
    </div>

    <div v-if="gettingLocation">
      <i>Getting your location...</i>
    </div>

    <div v-if="location">
      Your location is
      {{ location.Address.City }}
      {{ location.Address.Street }}
      {{ location.Address.HouseNumber }}
    </div>
  </div>
</template>

<script>
export default {
  el: '#user-location',
  data() {
    return {
      platform: null,
      apikey: 'gU56sivzmwGYIQ9nYkHQgcfFjjxUCrCeqJr358UgHpw',
      location: null,
      gettingLocation: false,
      errorStr: null,
    };
  },

  mounted() {
    const platform = new window.H.service.Platform({
      apikey: this.apikey,
    });
    this.platform = platform;

    const geocoder = platform.getGeocodingService();
    this.geocoder = geocoder;
  },

  methods: {
    async getLocation() {
      return new Promise((resolve, reject) => {
        if (!('geolocation' in navigator)) {
          reject(new Error('Geolocation is not available.'));
        }

        navigator.geolocation.getCurrentPosition(
          (pos) => {
            let reverseGeocodingParameters = {
              prox: `${pos.coords.latitude},${pos.coords.longitude}`,
              mode: 'retrieveAddresses',
              maxresults: 1,
            };
            this.geocoder.reverseGeocode(
              reverseGeocodingParameters,
              (res) => {
                let results = res.Response.View;
                if (results.length === 0) {
                  resolve('No match.');
                } else {
                  resolve(results[0].Result[0].Location);
                }
              },
              (e) => reject(e)
            );
          },
          (err) => {
            reject(err);
          }
        );
      });
    },

    async locateMe() {
      this.gettingLocation = true;
      try {
        this.gettingLocation = false;
        this.location = await this.getLocation();
      } catch (e) {
        this.gettingLocation = false;
        this.errorStr = e.message;
      }
    },
  },

  created() {
    if (!('geolocation' in navigator)) {
      this.errorStr = 'Geolocation is not available.';
      return;
    }

    this.locateMe();
  },
};
</script>

<style>
* {
  color: white;
}
</style>
