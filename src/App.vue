<template>
	<CarShowroom :cars="cars" :specsData="specsData" :colors="colors" :balance="balance"></CarShowroom>
</template>

<script>
import CarShowroom from "./components/CarShowroom.vue";

import audiLogo from "./assets/img/producers/audi.svg";
import teslaLogo from "./assets/img/producers/tesla.svg";
import kiaLogo from "./assets/img/producers/kia.svg";

import audiImage from "./assets/img/cars/audi-s5.png";
import bentleyImage from "./assets/img/cars/bentley.png";
import teslaImage from "./assets/img/cars/tesla-s.png";

export default {
	name: "App",
	components: {
		CarShowroom,
	},
	data: function () {
		return {
      balance: 25000,
			specsData: {
				maxSpeed: {
					measurementUnit: "km/h",
					label: "Max Speed",
					limit: 500,
				},
				acceleration: {
					measurementUnit: "sec",
					label: "Acceleration 0-100 km/h",
					limit: 5,
				},
				weight: {
					measurementUnit: "kg",
					label: "Weight",
					limit: 3500,
				},
				batteryCapacity: {
					measurementUnit: "l",
					label: "Battery capacity",
					limit: 200,
				},
				brakes: {
					measurementUnit: "$",
					label: "Brakes",
					limit: 40,
				},
			},
			cars: [
				{
					model: "Tesla Model S",
					price: 56000,
					image: teslaImage,
					producer: {
						logo: teslaLogo,
					},
					specs: {
						maxSpeed: 250,
						acceleration: 250,
						weight: 1200,
						batteryCapacity: 100,
						brakes: 100,
					},
				},
        {
					model: "Bentley ABC",
					price: 125522,
					image: bentleyImage,
					producer: {
						logo: audiLogo,
					},
					specs: {
						maxSpeed: 100,
						acceleration: 200,
						weight: 2000,
						batteryCapacity: 90,
						brakes: 100,
					},
				},
        {
					model: "Audi S5",
					price: 56000,
					image: audiImage,
					producer: {
						logo: audiLogo,
					},
					specs: {
						maxSpeed: 250,
						acceleration: 250,
						weight: 250,
						batteryCapacity: 250,
						brakes: 250,
					},
				},
				{
					model: "Audi S5",
					price: 56000,
					image: audiImage,
					producer: {
						logo: audiLogo,
					},
				},
				{
					model: "Audi S5",
					price: 56000,
					image: audiImage,
					producer: {
						logo: audiLogo,
					},
				},
				{
					model: "Audi S5",
					price: 56000,
					image: audiImage,
					producer: {
						logo: audiLogo,
					},
				},
				{
					model: "Audi S5",
					price: 56000,
					image: audiImage,
					producer: {
						logo: audiLogo,
					},
				},
			],
      colors: [
        "#CDCDCD",
        "#FF7A7A",
        "#FFD391",
        "#E2FF91",
        "#05FF00",
        "#91FFB6",
        "#91FFE4",
        "#91EBFF",
        "#91A9FF",
        "#A791FF",
        "#CE91FF",
        "#FD91FF",
      ]
		};
	},
};
</script>

<style lang="scss">
#app {
	background: center/cover url("./assets/img/dev-bg.png");
}

* {
	font-family: "Proxima Nova";
	margin: 0;
	padding: 0;
}
</style>
