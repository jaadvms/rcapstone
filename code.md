# rcapstone

## WEEK 1 - Complete the Data Collection with OpenWeather API Notebook
# API key: f19f7db711ce7396a57ed8f19e5cc13e
# Load httr package
install.packages("httr")
library(httr)
library(dplyr)

# URL for Current Weather API
current_weather_url <- 'https://api.openweathermap.org/data/2.5/weather'

# need to be replaced by your real API key
your_api_key <- "f19f7db711ce7396a57ed8f19e5cc13e"

# Input `q` is the city name
# Input `appid` is your API KEY, 
# Input `units` are preferred units such as Metric or Imperial
current_query <- list(q = "Seoul", appid = your_api_key, units="metric")


# make a HTTP request to the current weather API
response <- GET(current_weather_url, query=current_query)
# check response type 
http_type(response)

# read the JSON HTTP response
json_result <- content(response, as="parsed")

# use the class() function to see it is a R List object
class(json_result)

# print result 
json_result


# Create some empty vectors to hold data temporarily
weather <- c()
visibility <- c()
temp <- c()
temp_min <- c()
temp_max <- c()
pressure <- c()
humidity <- c()
wind_speed <- c()
wind_deg <- c()


# $weather is also a list with one element, its $main element indicates the weather status such as clear or rain
weather <- c(weather, json_result$weather[[1]]$main)
# Get Visibility
visibility <- c(visibility, json_result$visibility)
# Get current temperature 
temp <- c(temp, json_result$main$temp)
# Get min temperature 
temp_min <- c(temp_min, json_result$main$temp_min)
# Get max temperature 
temp_max <- c(temp_max, json_result$main$temp_max)
# Get pressure
pressure <- c(pressure, json_result$main$pressure)
# Get humidity
humidity <- c(humidity, json_result$main$humidity)
# Get wind speed
wind_speed <- c(wind_speed, json_result$wind$speed)
# Get wind direction
wind_deg <- c(wind_deg, json_result$wind$deg)

# Combine all vectors
weather_data_frame <- data.frame(weather=weather, 
                                 visibility=visibility, 
                                 temp=temp, 
                                 temp_min=temp_min, 
                                 temp_max=temp_max, 
                                 pressure=pressure, 
                                 humidity=humidity, 
                                 wind_speed=wind_speed, 
                                 wind_deg=wind_deg)

# Check the generated data frame
print(weather_data_frame)


## TASK: Get 5-day weather forecasts for a list of cities using the OpenWeather API
# TO DO: Write a function to return a data frame containing 5-day weather forecasts for a list of cities

# Create some empty vectors to hold data temporarily and declare data types
# City name column
city <- c()
city <- character()

# Weather column, rainy or cloudy, etc
weather <- c()
weather <- character()

# Sky visibility column
visibility <- c()
visibility <- numeric()

# Current temperature column
temp <- c()
temp <- numeric()

# Max temperature column
temp_min <- c()
temp_min <- numeric()

# Min temperature column
temp_max <- c()
temp_max <- numeric()

# Pressure column
pressure <- c()
pressure <- numeric()

# Humidity column
humidity <- c()
humidity <- numeric()

# Wind speed column
wind_speed <- c()
wind_speed <- numeric()

# Wind direction column
wind_deg <- c()
wind_deg <- numeric()

# Forecast timestamp
forecast_datetime <- c()
forecast_datetime <- character()

# Season column
# Note that for season, you can hard code a season value from levels Spring, Summer, Autumn, and Winter based on your current month.
season <- c("Winter")



# Get forecast data for a given city list
get_weather_forecast_by_cities <- function(city_names){
  df <- data.frame()
  for (city_name in city_names){
    # Forecast API URL
    forecast_url <- 'https://api.openweathermap.org/data/2.5/forecast'
    # Create query parameters
    forecast_query <- list(q = city_name, appid = "f19f7db711ce7396a57ed8f19e5cc13e", units="metric")
    # Make HTTP GET call for the given city
    response <- get(forecast_url, forecast_query)
    # Note that the 5-day forecast JSON result is a list of lists. You can print the response to check the results
    json_list <- content(response, as="parsed")
    results <- json_list$list
    
    # Loop the json result
    for(result in results) {
      city <- c(city, city_name)
      weather <- c(weather, json_list$weather)
      visibility <- c(visibility, json_list$visibility)
      temp <- c(temp, json_list$temp)
      temp_min <- c(temp_min, json_list$temp)
      temp_max <- c(temp_max, json_list$temp)
      pressure <- c(pressure, json_list$pressure)
      humiditiy <- c(humiditiy, json_list$humiditiy)
      wind_speed <- c(wind_speed, json_list$wind_speed)
      wind_deg <- c(wind_deg, json_list$wind_deg)
      forecast_datetime <- c(forecast_datetime, json_list$forecast_datetime)
      season <- c('Winter')
      
    }
    
    # Add the R Lists into a data frame
    df <- data.frame(city = city, 
                    weather = weather, 
                    visibility = visibility, 
                    temp = temp, 
                    temp_min = temp_min, 
                    temp_max = temp_max, 
                    pressure = pressure, 
                    humidity = humidity, 
                    wind_speed = wind_speed, 
                    wind_deg = wind_deg, 
                    forecast_datetime = forecast_datetime, 
                    season = season)
  }
  
  # Return a data frame
  return(df)
  
}

# Complete and call `get_weather_forecaset_by_cities` function with a list of cities, 
# and write the data frame into a csv file called `cities_weather_forecast.csv`
cities <- c("Seoul", "Washington, D.C.", "Paris", "Suzhou")
cities_weather_df <- get_weather_forecast_by_cities(cities)
