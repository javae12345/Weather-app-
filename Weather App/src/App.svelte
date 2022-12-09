<script>
import { time } from './lib/stores.js';
 import Geolocation from "svelte-geolocation";

let coords;

const formatter = new Intl.DateTimeFormat('en', {
	hour12: true,
	hour: 'numeric',
});

const weather = {
	value: -6,
	description: " Mostly Cloudy",
	city: "calgary"
}


async function getWeather(){
	let latitude = coords[1];
	let longitude = coords[0];
	let key = "74a2137d285b88947071a935be501d4d";
		let api = `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${key}`;
	console.log(api);
	const res = await fetch(api);
    const data = await res.json();
    
    if (res.ok) {
        return data;
    } 
}
let promise;
function handleClick() {
  promise = getWeather();
}

</script>

<h1 class = "title">This is the Weather Time</h1>

<div id = main>
	<div>
		<p>{formatter.format($time)}</p>
		
		<Geolocation getPosition bind:coords />
		
		<button on:click = {handleClick}>
			weather data
		  </button>
		<p class = city> The Weather in {weather.city} </p>
		<p class = temperature>{weather.value}</p>
		<p class = description> {weather.description} </p>
		<p> Your Longitude and Latitude is {JSON.stringify(coords)}</p>
		
	
	</div>
</div> 

<style>

.title {
	display: flex;
	justify-content: center;
}
.city {
	font-size: 26px;
}
.temperature {
	font-size: 40px;
}

#main {
	display: flex;
	text-align: center;
	justify-content: center;
	align-items: center;
	height: 500px;
}

#main > div {
	padding: 2rem;
	border-radius: 20px;
	background-color: white;
	box-shadow: 5px 5px;
	width: fit-content;
}

</style>