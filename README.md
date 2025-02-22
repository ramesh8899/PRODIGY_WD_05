# PRODIGY_WD_05
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #6ca7e2, #529fb55c);
            color: #fff;
            text-align: center;
            padding: 20px;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
        }
        input, button {
            padding: 10px;
            border: none;
            border-radius: 5px;
        }
        button {
            background-color: #28a745;
            color: #fff;
            cursor: pointer;
        }
        button:hover {
            background-color: #218838;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Weather App</h1>
        <p>Enter a location or allow access to your location:</p>
        <input type="text" id="locationInput" placeholder="Enter city name">
        <button onclick="getWeatherByCity()">Get Weather</button>
        <button onclick="getWeatherByLocation()">Use My Location</button>
        <div id="weatherInfo"></div>
    </div>

    <script>
        const apiKey = 'cdfd2c2c31d90f2e991b0c441d813006'; // Replace with your OpenWeather API key

        function getWeatherByCity() {
            const city = document.getElementById('locationInput').value;
            fetchWeather(`https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`);
        }

        function getWeatherByLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(position => {
                    const { latitude, longitude } = position.coords;
                    fetchWeather(`https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&units=metric&appid=${apiKey}`);
                });
            } else {
                alert("Geolocation is not supported by this browser.");
            }
        }

        function fetchWeather(url) {
            fetch(url)
                .then(response => response.json())
                .then(data => {
                    const weatherInfo = document.getElementById('weatherInfo');
                    if (data.weather && data.weather.length > 0) {
                        weatherInfo.innerHTML = `
                            <h2>${data.name}</h2>
                            <p>${data.weather[0].description}</p>
                            <p>Temperature: ${data.main.temp}°C</p>
                            <p>Humidity: ${data.main.humidity}%</p>
                            <p>Wind Speed: ${data.wind.speed} m/s</p>
                        `;
                    } else {
                        weatherInfo.innerHTML = `<p>No weather data available for this location.</p>`;
                    }
                })
                .catch(error => {
                    alert("Error fetching weather data. Please check the location and try again.");
                    console.error(error);
                });
        }
    </script>
</body>
</html>
