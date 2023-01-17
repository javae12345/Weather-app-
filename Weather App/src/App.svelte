<script>
import { time } from './lib/stores.js';
import Geolocation from "svelte-geolocation";


let coords;
let loaded = false
let kelvin = 273.15;
let cityInput = "calgary";


const formatter = new Intl.DateTimeFormat('en', {

	hour12: true,
	hour: 'numeric',
});


let weather
$:{
	if(!loaded&&coords&&coords[0]!=-1){

		weather = getWeather(coords)
		loaded=true
	}
}


async function getWeather(c){
	if(c){
		let latitude = c[1];
		let longitude = c[0];

	let key = "74a2137d285b88947071a935be501d4d";
		let api = `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${key}`;
		const res = await fetch(api);
		const data = await res.json();
console.log(data)
   
        return data;
   } 
}


async function cityCoord(){
	let key = "74a2137d285b88947071a935be501d4d";
		let api = `http://api.openweathermap.org/geo/1.0/direct?q=${cityInput}&limit=1&appid=${key}`;
		const res = await fetch(api);
		const data = await res.json();
console.log(data)

   
        return data;
   } 


async function handleClick() {
let newCoords = await cityCoord()

console.log(newCoords);

 weather = getWeather([newCoords[0].lon, newCoords[0].lat])
}


</script>
{#await weather then w}
<h1 class = "title">This is the Weather Time</h1>
<div id = main>
	<div>
		<p>{formatter.format($time)}</p>
		
		<Geolocation getPosition bind:coords />

		<button on:click = {handleClick}>
			Get a new city
		  </button>

		<input bind:value={cityInput} placeholder="enter a city">

		<p class = city> The Weather in {w?.name} </p>

		<p class = temperature>{Math.floor(w?.main.temp-kelvin)}°</p>

		<p class = description> {w?.weather[0].description} </p>
		
	</div>
</div> 
{/await}

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

.description {
	font-size: 20px;
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