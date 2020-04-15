# Vlad's simple Weather app

## Project overview
This small project is an result of a follow along and some tweeks added by me. The original project by TylerPottsDev is  [here](https://cli.vuejs.org/config/).

The original project consisted of a search field where you can search for a city. Upon hitting enter, the current weather information will be displayed. The styling consists of plain CSS and two background images that would change based on the weather.

From my side, I altered the forecast to display not just the current weather, but also the forecast for the next 4 days. As well as added a few minor things:
* Clear the input field alter a successfull call.
* Do not do a call if user calls twice on the same query
* Added a simple animation for the reveal of the forecast
* Instead of using images for background, I used plain colors
* The 4 day forecast shows the max degree number and its weather status.