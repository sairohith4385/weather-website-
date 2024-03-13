# weather-website-
weather website using html cssand javascript
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Weather App</title>
        
        <style>
         @import url("https://fonts.googleapis.com/css2?family=Poppins:wght@200;400;600&display=swap");

body {
  background-image: url("https://images.unsplash.com/photo-1483728642387-6c3bdd6c93e5?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2076&q=80");
  background-size: cover;

  font-family: "Poppins", sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 0;
  height: 100vh;
}

::placeholder {
  color: #fff;
}

input {
  background-color: #575757;
  border: none;
  border-radius: 25px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  font-family: inherit;
  font-size: 1rem;
  padding: 1rem;
  min-width: 300px;
  color: #fff;
  transition: 0.2s;
}

input#search:hover {
  background: #65747b;
}

input:focus {
  outline: none;
  align-items: center;
  margin-bottom: 0;
  padding-left: 85px;
}

.weather {
  font-size: 2rem;
  text-align: center;
}

.weather h2 {
  display: flex;
  align-items: center;
  margin-bottom: 0;
  padding-left: 90px;
}

.card {
  background: #000000b2;
  width: 100%;
  max-width: 470px;
  height: 70%;
  padding: 2.5em;
  text-align: center;
  border-radius: 30px;
  color: white;
}

.more-info p {
  font-size: 20px;
}
.more-info span {
  font-weight: 600;
}
        </style>
       
    </head>
    <body>
      <div class="card">
        <form id="form">
            <input
                type="text"
                id="search"
                placeholder="Search by location"
                autocomplete="off"
            />
        </form>
        <main id="main"></main>
        </div>
    </body>
</html>
<script>
    const apikey = "d306ffc4f94bc2ef2c8be1cdc61d8eb9";

const main = document.getElementById("main");
const form = document.getElementById("form");
const search = document.getElementById("search");

const url = (city) =>
  `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apikey}`;

async function getWeatherByLocation(city) {
  const resp = await fetch(url(city), { origin: "cors" });
  const respData = await resp.json();

  console.log(respData);

  addWeatherToPage(respData);
}

function addWeatherToPage(data) {
  const temp = KtoC(data.main.temp);
  const humidity = data.main.humidity;
  const windSpeed = data.wind.speed;

  const weather = document.createElement("div");
  weather.classList.add("weather");

  weather.innerHTML = `
        <h2><img src="https://openweathermap.org/img/wn/${
          data.weather[0].icon
        }@2x.png" /> ${temp}°C <img src="https://openweathermap.org/img/wn/${
    data.weather[0].icon
  }@2x.png" /></h2>
        <small>${data.weather[0].main}</small>
        <div class="more-info">
        <p>Humidity : <span>${humidity}%</span></p>
        <p>Wind speed : <span>${+Math.trunc(windSpeed * 3.16)}km/h</span></p>
        </div>
    `;

  // cleanup
  main.innerHTML = "";

  main.appendChild(weather);
}

function KtoC(K) {
  return Math.floor(K - 273.15);
}

form.addEventListener("submit", (e) => {
  e.preventDefault();

  const city = search.value;

  if (city) {
    getWeatherByLocation(city);
  }
});
</script>
