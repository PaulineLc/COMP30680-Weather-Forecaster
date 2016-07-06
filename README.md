# COMP30680-Weather-Forecaster
Project realized for COMP30680 Web Application Development (University College Dublin); Assignment 2 ([brief](COMP30680 Assignment 2.pdf))


Requirements

1. OK
2. OK
3. OK
4. OK - click on a day to diplay that weather
5. OK - click on a time to display that weather

Choice was made to use the 3 hours forecast at all time because it is unclear weather the daily forecast is a paying feature or not:
- The API call reference on the OWM documentation is: api.openweathermap.org/data/2.5/forecast?q={city name},{country code}
- The daily forecast provided by the call: api.openweathermap.org/data/2.5/forecast/daily?q={city name},{country code}&cnt={cnt} 
is listed under the 16 days / daily forecast documentation which is not available to free accounts ("Available for Developer, Professional and Enterprise accounts" - even though, oddly, the call does work with free accounts).
- The 5 day / 3 hour forecast makes no reference to getting the daily forecast ("weather forecast for 5 days with data every 3 hours").
- Due to this ambiguity, I decided against making a call with the parameter "daily".


Error handling

There are 5 error handlers:

1. if the API is not available (return code 4** or 5**): an error message appears explaining that the 'weather-forecasting frogs' are sleeping and prompt the user to try again later. An error in thrown to explain that the API is unavailable.

2. Autocomplete is unabled on the form to make it easier for the user to pick a valid location. This is using the Google Map Autocomplete API.

3. if the user enters no location or an incorrect location then an error message appears explaining that the 'weather-forecasting frogs couldn't understand the request' and prompt the user to enter an incorrect location
4. if the website is load with an HTTPS protocol, an error message appears to explain that the website cannot support HTTPS. The error message explains how to use the website, and an additional web page explains why the website doesn't work with HTTPS.
5. a function checks that there is actually a rain element in the JSON (it is sometimes missing - e.g. if there is no rain) and handles the showing of the rain accordingly.


Going beyond

I decided to go beyond basic requirements and make my website available online. It is available online ([link](http://myforecaster.herokuapp.com/))

In order to make it available online I had to create an additional PHP file. The PHP file make it possible for my website to appear online with Heroku ([source used](http://stackoverflow.com/questions/10551273/is-it-possible-to-upload-a-simple-html-and-javascript-file-structure-to-heroku)).

While testing it, I realised that the website wouldn't work with HTTPS. After an investigation, I realised that it was because the OWM API was called with an HTTP protocol, which would result in a mixed content error, and the website wouldn't work ([source](https://developer.mozilla.org/en-US/docs/Security/Mixed_content)). I therefore initially decided to handle it as follow:

if the website was loaded on HTTPS, I would redirect it on HTTP:

```javascript
if (window.location.protocol == "https:")
    window.location.href = "http:" + window.location.href.substring(window.location.protocol.length);
```

(code adapted from: [link](http://stackoverflow.com/questions/4723213/detect-http-or-https-then-force-https-in-javascript))

However, I realised that widely-used extensions such as HTTPS everywhere would make the website enter an infinite loop (HTTPS -> HTTP -> HTTPS -> HTTP) because they would automatically redirect the website on HTTPS once they detect that it is on HTTP and that HTTPS is available. This happened because Heroku does make all applications available on HTTPS ([source](https://devcenter.heroku.com/articles/ssl-endpoint)).

In order to avoid that infinite loop, I decided that, instead of forcing the user to use an HTTP connection, I would display an error message explaining that the website will not work with an HTTPS connection and how to make the website work. I also offer users the opportunity to understand why the website does not work with HTTPS (cf [httpserror.html](httpserror.html)).

Once this was done, I asked my friends to test it and this result in some change in the UI to make it better for users (eg: hover effects for the days as well as a different background color). I also learnt that way that the website does not work on Safari, even though I was not able to debug that.

The color scheme for the website was reached and several color scheme were tried. The color palette #4 on this article was adopted as a result ([source / inspiration](http://blog.crazyegg.com/2012/07/11/website-color-palettes/))


Code reuse

Non-native code was reference directly in the page comments.
