## PianoSolo Weather Bundle

The PianoSolo Weather Bundle is a Symfony2 project to get data from weather services. Currently bringing data from 
OpenWeatherMap api. PianoSolo Weather Bundle has got different way of using the weather data in your project. One 
as a Twig Extension and the second one as a service which brings all weather information from api as object.

[![Latest Stable Version](https://poser.pugx.org/pianosolo/weather-bundle/v/stable)](https://packagist.org/packages/pianosolo/weather-bundle)
[![License](https://poser.pugx.org/pianosolo/weather-bundle/license)](https://packagist.org/packages/pianosolo/weather-bundle)

### Installation 

1-)Tell composer to download by running the command:

    ```bash
    composer require pianosolo/weather-bundle
    ```
 
2-)Add the bundle to AppKernel

	``` php
	<?php
	// app/AppKernel.php
	
	public function registerBundles()
	{
	    $bundles = array(
	        // ...
	        new PianoSolo\WeatherBundle\PianoSoloWeatherBundle(),
	    );
	}
	```
	
3-)Add configuration to config.yml file

	```yaml
	piano_solo_weather:
    	api_key: "c2388fd461bcbaec4448d227e27cc8d1" # Required
    	options:
        	download_csv: false # Default True
        	cache: true # Default False (To use cache the system needs Php-Apc cache)
	```
	
	To install Php-Apc rund this command:
	
	```bash
    sudo apt-get install php-apc
    ```
	
4-)Add the routing for CSV Downloading to routing.yml

	```yaml
	pianosolo_weather:
    	resource: "@PianoSoloWeatherBundle/Resources/config/routing.yml"
	```

### Usage

#### As Twig Extension

	It brings the daily forecasts as html formatted in bootstrap table. 

	```twig
    {{ pianosolo_get_forecast('Istanbul', 5) }}
    ```
    
    This brings 5 days weather information of Istanbul ( Date, City, Temperature, Description )
    
    ```twig
    {{ pianosolo_get_weather('Istanbul') }}
    ```
    
    This brings 1 day of weather reasults of Istanbul.
    
### As Weather Service

1-) Get weather response as stdclass object

	``` php
	$weatherService = $this->get('pianosolo.weather');
	$weather = $weatherService->getWeather('Istanbul');
	```
2-) Get weather as weather object

	``` php
	$weatherService = $this->get('pianosolo.weather');
	$weathersArray = $weatherService->getWeatherObject('Istanbul');
	$weatherObject = $weathersArray[0];
	$city = $WeatherObject->getCity();
	$date = $WeatherObject->getWdate();
	$temperatarue = $WeatherObject->getTemperetare();
	$description = $WeatherObject->getDescription();
	```	
1-) Get forecast response as stdclass object

	``` php
	$weatherService = $this->get('pianosolo.weather');
	$weather = $weatherService->getForecast('Istanbul', 2); // 2 days weather results of the city
	```
2-) Get forecast as weather object array

	``` php
	$weatherService = $this->get('pianosolo.weather');
	$weathersArray = $weatherService->getWeatherObject('Istanbul');
	foreach($weathersArray as $weatherObject){
		$city = $WeatherObject->getCity();
		$date = $WeatherObject->getWdate();
		$temperatarue = $WeatherObject->getTemperetare();
		$description = $WeatherObject->getDescription();
	}
	```
