# CS-425-Currency-Conversion-Calculator-2
This assignment is intended to give you additional practice with the AJAX methods available through jQuery, as well as additional practice with JSON data structures and dynamic HTML.  For this assignment, you will be creating a simple Web-based calculator application for converting U.S. Dollar (USD) values to other currencies, based on the most current exchange rates.  You will implement this application in two parts: in the first part, your application will simply convert from USD to GBP (British pound sterling), and in the second part, you will improve the application so that it can convert from USD to any available target currency as selected by the user.

To retrieve the current currency exchange rates, your program will be using a simple Web service which provides an API for retrieving the current rates as JSON data, using the HTTP protocol.  There are many such APIs in existence, usually provided as paid services by banks and other financial institutions.  The API that we will use is backed by data provided by the European Central Bank:

https://testbed.jaysnellen.com:8443/JSUExchangeRatesServer/rates (Links to an external site.)

https://testbed.jaysnellen.com:8443/JSUExchangeRatesServer/currencies (Links to an external site.)

The rates URL supports a date parameter which can be added to the URL in a query string to change the date, to retrieve historical rates, like so:

https://testbed.jaysnellen.com:8443/JSUExchangeRatesServer/rates?date=2022-09-21 (Links to an external site.)

This API accepts GET requests to the URLs given above.  The first URL returns the exchange rate data, and the second URL provides a list of the recognized currency codes and their respective descriptions.  The results are returned in JSON format.

Try opening the first URL in your browser.  The Firefox browser, and possibly others, will display the returned JSON data using syntax highlighting, so that you can expand and collapse the nested JSON data structures.  Note that the rates are provided as key/value pairs inside the "rates" object, and that the exchange rate date is available through the "date" value.  To see the original JSON data as it was returned by the server as plain text, including all nested element(s) and key/value pairs, select "Raw Data" from the menu of options at the top of the screen.

 

Part One

In the first part of this assignment, create a simple Web page containing a "Currency Conversion Calculator" form.  (You can use the HTML from your number guessing game as a starting point if you wish.)  Inside the form, add the following paragraphs:

<p>Convert to: <span id="currencymenu">GBP (Pound sterling)</span></p>

(Note that the span element contains a "hard-coded" target currency selection; later, you will be filling this span with a menu created from the JSON currency list data.)

To complete Part One, use the JavaScript and HTML files attached to this assignment as a starting point.  Your program should accept a value in USD from the user as input; when the user clicks the "Convert" button, the program should compute the equivalent value in GBP, using the latest available rate from the JSON data, and display this value in a new paragraph as the output.

(NOTE: The API currently uses the Euro as the default base currency.  So, in performing conversions from US dollars to another currency, your calculator will need to perform two steps: convert the specified value from US dollars to Euros first, and then convert it from Euros to the target currency, using the corresponding rate from the JSON data.)

When the first part is complete, your application should appear similar to the following:

Screenshot_Lab4_Part1.png

Note that in the output, numeric values are formatted as currency, according to the currency code selected by the user.  Here is a short example of how this should be done in JavaScript; in this example, the variable usd_value is the numeric value entered by the user, and the variable usd_value_string is the string representation formatted as currency:

var usd_value_string = (usd_value).toLocaleString('en-US', {
    style: 'currency',
    currency: 'USD',
});

(Of course, the currency code 'USD' should be replaced with the currency code selected by the user when formatting the converted value.)

The JavaScript program file also includes a number of "placeholder" functions for you to complete.  The first is onClick(), which is called when the user clicks the "Convert" button in the input form.  The program stores its exchange rate data in the rates object; if the data has already been retrieved for the selected date, it will call the convert() function, which should implement the logic to perform the currency conversion using the exchange rate data in the rates object.  If this object has not yet been initialized, or if the user selects a different date, the getRatesAndConvert(); function will be called instead; this function should retrieve the current rates with a jQuery AJAX call; the callback function should save the received data (simply assign this result object to the rates variable), and then call convert() to complete the conversion.

Refer to the example AJAX calls given in the lecture notes, and be sure to set the URL to the first Web service URL shown earlier.  Remember to keep the data format as JSON so that the data is properly parsed into an object after it is received.  When the date and starting value are set to match those shown in the screenshot, your program's results (shown in the placeholder output <div> element, directly below the form) should also match.

 

Part Two

After completing the first part of this assignment—and don't move on to this second part until you do!—save a complete copy of your completed program from Part One, and then begin work on an improved version which will allow the user to specify the target currency, which was "hard-coded" to "GBP (Pound sterling)" in Part One.  Note the paragraph that was given in the input form in Part One:

<p>Convert to: <span id="currencymenu">GBP (Pound sterling)</span></p>

Your program should replace the text inside the <span> element in this paragraph with a dynamically-generated drop-down list of currencies, created from the collection of currency codes and descriptions in the JSON data provided by the second API link shown earlier.  Your program should retrieve this data, copy it into a drop-down list, and then add this list to the page as a child of the <span> element shown earlier, overwriting any other text in this element.  Uncomment the call to the getCurrenciesList() function in the HTML file so that the currency list is presented as a drop-down list as soon as the page is loaded.  (This should not need to be a large function; my implementation is less than twenty lines long.)

Remember that drop-down lists are created in HTML with a <select> element, which should contain one <option> element for each selection.  Here is a partial example of how your HTML should be structured:

<select name="target_currency" id="target_currency">
   <option value="CAD">CAD (Canadian dollar)</option>
   <option value="EUR">EUR (Euro)</option>
   <option selected value="GBP">GBP (Pound sterling)</option>
   <option value="MXN">MXN (Mexican peso)</option>
</select>

(In your application, this list must be generated dynamically from the JSON data!  It should not be entered into your page as static HTML!)

Note that each option in the drop-down list is represented by an <option> element, and that the "value" parameter, and the currency code shown within the element, should be the same.  In your menu, GBP should be selected as the default option with the selected attribute, as shown in this example.  To create the menu, begin by creating the <select> element, then create the <option> elements by looping through the currencies in the JSON data, appending one <option> element for each currency found in the object.  Finally, append the <select> element to the <span> element.  This menu should be created and displayed for the user immediately after the currency data is fetched from the Web service.

Finally, adjust your conversion function to convert the specified amount to the currency selected by the user.  To identify the selected currency, you can retrieve the selected value of the <select> form element, just as you retrieved the value from the <input> form element.  I recommend using the val() function provided by jQuery; see the lecture notes for an example of how to use this function.

When you are finished, your program should appear similar to the following:

Screenshot_Lab4_Part2.png
