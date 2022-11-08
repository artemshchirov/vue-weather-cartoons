# VUE X WEATHER-CARTOONS

## Have a look at the project

<https://artemshchirov.github.io/vue-weather-cartoons>

![preview image](./src/assets/vue-weather-cartoons-preview.png 'home page of vue-weather-cartoons')

## Description

Best web application that helps you find What To Watch When You Don't Know What To Watch. If you want to get popular, grown-up-friendly cartoon titles like Horse Bo Jack, Final Space or Solar Opposites depends on weather in your location.
This is a Vue-based project

The application was deployed on GitHub pages.

## Functionality

- Fetch weather data for cities in the openweathermap under key words
- Fetch and show user location
- Adaptive layout - optimized for different screen resolutions (mobile, tablet, desktop)

## API used

- [external API](https://openweathermap.org/) provided by OpenWeather - weather forecasts, nowcasts and history in a fast and elegant way

## Technologies used

- HTML, CSS, JavaScript
- Vue.js, JSX
- Flexbox, media queries, adaptive layout
- Creating a new project in Vue.js

## Some of the skills used are

- Getting started with Vue.js
- SPA project structures
- Debugging

## Start project locally

```bash
git clone https://github.com/artemshchirov/vue-weather-cartoons.git
cd vue-weather-cartoons
npm install
npm run serve
```

## Logic and list of movies

```Javascript
      switch (true) {
        case this.temp > 35:
          return 'BoJack Horseman';
        case this.temp > 15:
          return 'Final Space';
        case this.temp > 0:
          return 'Solar Opposites';
        case this.temp < 0:
          return 'Love Death + Robots';
        default:
          return 'Ed, Edd n Eddy';
      }
```
