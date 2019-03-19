---
title: "Weather App"
date: 2019-03-18T19:57:32-04:00
draft: false
toc: false
comments: true
categories:
- Development
tags:
- Angular
---
I took up an interview challenge from DegreeWorks and created a bare-bones web app that pulls data from [OpenWeatherMap](https://openweathermap.org/).
<!--more-->
Upon completion of my latest graduate course, I attempted an assignment from a company that interviewed me and found me promising for their Javascript Developer position. The link app can be found [here](https://github.com/hydure/weatherApp). The web application uses Angular 7 as a front end with no backend and has two simple services, one that gets the user's current location and another that gets the current temperature or the five-day forecast for that area.

There are also two main components (exluding the navbar and other basically essential components): the `current-day` component and the `five-day-forecast` component. The `current-day` component fetches and displays the current temperature at the user's current location, while the `five-day-forecast` component fetches and displays the 5-day forecast for the user's current location. All of the 3-hourly forecasts within this 5-day period are displayed on their own rows in a table. Each row displays the forecast's date and time, the temperature in Fahrenheit, and the OpenWeatherMap icon that represents the forecast weather conditions.

I have thought about developing this app further, but am trying to relax these two weeks before my next graduate course begins. But, any and all suggestions are definitely welcome, just reach out to me!