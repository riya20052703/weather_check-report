<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Weather App</title>
  <style>
    body {
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(135deg, #89f7fe, #66a6ff);
      color: #fff;
      text-align: center;
      margin: 0;
      padding: 0;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    h1 {
      font-size: 2.5rem;
      margin-bottom: 20px;
    }

    .weather-box {
      background: rgba(255, 255, 255, 0.2);
      border-radius: 20px;
      padding: 30px;
      width: 300px;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
      backdrop-filter: blur(10px);
    }

    input {
      width: 80%;
      padding: 10px;
      border: none;
      border-radius: 8px;
      outline: none;
      text-align: center;
      font-size: 1rem;
    }

    button {
      margin-top: 15px;
      padding: 10px 20px;
      background: #005bea;
      border: none;
      color: white;
      font-size: 1rem;
      border-radius: 8px;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      background: #003f8a;
    }

    .result {
      margin-top: 20px;
    }

    .error {
      color: #ffdddd;
      font-weight: bold;
    }
  </style>
</head>
<body>

  <h1>🌦️ Weather App</h1>

  <div class="weather-box">
    <input type="text" id="locationInput" placeholder="Enter city name" />
    <button onclick="getWeather()">Get Weather</button>

    <div class="result" id="result"></div>
  </div>

  <script>
    async function getWeather() {
      const location = document.getElementById('locationInput').value.trim();
      const resultDiv = document.getElementById('result');

      if (!location) {
        resultDiv.innerHTML = '<p class="error">⚠️ Please enter a location!</p>';
        return;
      }

      // ✅ Your API key added here
      const apiUrl = `http://api.weatherapi.com/v1/current.json?key=516bb37582b44339b3475256251810&q=${location}&aqi=yes`;

      try {
        const response = await fetch(apiUrl);
        if (!response.ok) throw new Error('Location not found');

        const data = await response.json();
        const { temp_c, condition } = data.current;
        const city = data.location.name;
        const country = data.location.country;

        resultDiv.innerHTML = `
          <h2>${city}, ${country}</h2>
          <h3>${temp_c}°C</h3>
          <p>${condition.text}</p>
          <img src="https:${condition.icon}" alt="weather icon">
        `;
      } catch (error) {
        resultDiv.innerHTML = `<p class="error">❌ ${error.message}</p>`;
      }
    }
  </script>

</body>
</html>
