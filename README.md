# weather-with-cityid
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>Weather App</title>
  </head>
  <body>

    <h1>Weather API Results</h1>

    <form action="/" method="post">
        <label for="cityId">City ID: </label>
        <input id = "cityId" type="text" name="cityId">
        <button type="submit">Go</button>
    </form>

  </body>
</html>
const express = require("express");
const https = require("https");
const bodyParser = require("body-parser");

const app = express();

app.use(bodyParser.urlencoded({extended: true}));

//displays index.html of root path
app.get("/", function(req, res) {
  res.sendFile(__dirname + "/index.html")
});

//invoked after hitting go in the html form
app.post("/", function(req, res) {
    
    // takes in the zip from the html form, display in // console. Takes in as string, ex. for zip 02139
        var zip = String(req.body.zipInput);
        console.log(req.body.zipInput);
    
    //build up the URL for the JSON query, API Key is // secret and needs to be obtained by signup 
        const units = "imperial";
        const apiKey = "18fc8e4d05d9cbbbc359023d0f76ead1";
        const url = "https://api.openweathermap.org/data/2.5/weather?id=" + 2172797 +  "&units=" + units + "&APPID=" + apiKey;
    
    // this gets the data from Open WeatherPI
    https.get(url, function(response){
        console.log(response.statusCode);
        
        // gets individual items from Open Weather API
        response.on("data", function(data){
            const weatherData = JSON.parse(data);
            const temp = weatherData.main.temp;
            const city = weatherData.name;
            const wind = weatherData.wind.speed;
            const windDirection = weatherData.wind.deg;
            const clouds = weatherData.clouds.all;
            const weatherDescription = weatherData.weather[0].description;
            const icon = weatherData.weather[0].icon;
            const imageURL = "http://openweathermap.org/img/wn/" + icon + "@2x.png";
            
            // displays the output of the results
            res.write("<h1> The weather is " + weatherDescription + "<h1>");
            res.write("<h2>The Temperature in " + city + " " + zip + " is " + temp + " Degrees Fahrenheit<h2>");
            res.write("<h2> Wind speed " + wind + " " + " wind direction is " + windDirection + " Degrees <h2/>")
            res.write("<h2> Cloudiness is " + clouds + " <h2/>");
            res.write("<img src=" + imageURL +">");
            res.send();
        });
    });
})


//Code will run on 3000 or any available open port
app.listen(process.env.PORT || 3000, function() {
console.log ("Server is running on port")
});
