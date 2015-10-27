# _**Data Representation Project**_

**Student:** Ciaran Whyte </br>
**Module:** Data Representation and Querying </br>
**Lecturer:** Ian Mcloughlin </br>


## _**Introduction**_

This project was designed as part of a Data Representation module. In the project I have to design an API for a data set, not implement it. I will be designing the URLs for querying a particular data set for useful information, keeping in mind the needs of the users that will use the API. These datasets are located at [Data.Gov.ie] (https://data.gov.ie/data) The API will be provided through HTTP. Each of the URLs will be described in detail explaining certain points like what kind of data will be returned from the URL and what kind of HTTP method(s) are being used.

## _**The Data Set**_
The data set i will be using contains data about the variety of public parks located around Galway city. Exercise is vital today for good health from both a physical and mental perspective. Therefore i find it very important that people should be aware of the different facilities they have in their local area. Currently this data set contains data for all the parks in Galway city but this could be expanded to contain all the parks in the country, province or country. It also would not have to just be for parks, it could also contain gyms, sport pitches etc. The main concept of this being all about exercise and awareness of public facilities. The data set contains different information about each park. It is in CSV file format and has 29 rows, each row contains the following values such as the parks name, location, geograpic location, its facilities and more. If you wish to view the actual dataset you can download it here: [Parks In Galway City] (http://opendata.galwaycity.opendata.arcgis.com/datasets/683ff500430447c985f4775a6b5dd112_0.csv)

## _**Common HTTP Methods and a brief description**_

**HTTP METHOD** | **DESCRIPTION**
------------ | -------------
GET | The GET method means retrieve whatever information is identified by the Request-URI. If the Request-URI refers to a data-producing process the produced data will be returned as the object in the response.
POST | The POST method is used to send data to the server. The actual function that the POST method performs is determined by the server and is most likely dependent on the URL.
HEAD | The HEAD method is very similar to the GET method in what it does. The main difference being it only returns the response header.
PUT | Sets the data in the URI to that of the requested data
DELETE | Deletes the data at the URI

## _**The URLs**_
Much of the Javascript that we write to handle these URLs is written with a library in Javascript we call JQuery.
JQuery is sometimes donated by using the $ sign. JQuery contains functions which allow us to work with a URL. Here is a general example,
 
```javascript
 var url = "urlWithSomething/file/someData";
  $.get( url, function(data1){ //We can use JQuery.get() to retrieve data from the URL
    var url1 = "urlWithSomethingMoreSpecific/file/someData";
     $.get(url1, function(data2){ //Inner .get() on more specific URL
       console.log(data2.attribute);//Output the data to the console, the data can come in many different models, eg XML,JSON
        $('#hn').html(data2.attribute); //Using another JQuery method .html() to get a handle on a DOM object
     });
     console.log(data); //Output less specific data to the console
});
```
### 1. _**Entering the site**_ 
On entering the site, if your a member you may need to provide some authentication details. The following is an example

 ```
   POST /entry/login_form.htm HTTP/1.1
   Host: GalwayParks.com
   user=value1&password=value2
 ```
**_METHOD:_** POST (The HTTP POST method is used for sending this information, as you can see in the first line of the HTTP message body we say POST. The last line refers to the information we are sending. This information is sensitive and therefore would not be sent in the URL, which is why we use the POST method.)

### 2. _**List of available parks by general city area (East, West etc)**_
The following URL provides a list of all the Galway parks in a general area. People dont always know the names of their specific sourrounding areas. This allows for a general search.

**_THE URL:_** *```http://galwayparks.com/area/[:area]```* </br>
The [:area] represents the part of the URL being replaced depending on which area is provided.

**_METHOD:_** GET (The HTTP GET method is used for retrieving this information)

**_EXAMPLE:_** *```http://galwayparks.com/area/east```* </br>
This would return a list of of all the parks in galway city that are located in the east of the city, along with some other 
useful information.

The data returned from this URL will be in JSON (Javascript Object Notation) format, with the following data for each park:
      
      **PROPERTY** | **DESCRIPTION**
      ------------ | -------------
       NAME | The name of the park.
       LOCATION | The location of the park, contains information like the street name.
       AREA | The area of the city in which te park is located
       FACILITIES | The different facilities that are available at the park
       OPENING HOURS | The opening hours for the park

An example of the json response would be:
 ```json
    [{"NAME": "Renmore Park", 
    "LOCATION": "Renmore, Galway",
    "AREA": "City-East",
    "FACILITIES": ["2 Gaelic Playing Pitches, 2 Soccer Playing Pitches", "Planting areas with flowers, shrubs, trees"], 
    "OPENING HOURS": "No restricted opening hours"}]
 ```

### 3. _**List of available parks by city location**_
The following URL provides a list of all the Galway parks in a given location. This provides a more specific search than the previous and is based on locations in the city.

**_THE URL:_** *```http://galwayparks.com/location/[:location]```* </br>
The [:location] represents the part of the URL being replaced depending on the location data that is being provided in the URL.

**_METHOD:_** GET (The HTTP GET method is used for retrieving this information)

**_EXAMPLE:_** *```http://galwayparks.com/location/Shantalla```* </br>
This would return a list of of all the parks in galway city that are located in Shantalla, along with some other 
useful information.

The data returned from this URL will be in JSON (Javascript Object Notation) format, with the following data for each park:
      
      **PROPERTY** | **DESCRIPTION**
      ------------ | -------------
       NAME | The name of the park.
       LOCATION | The location of the park, contains information like the street name.
       FACILITIES | The different facilities that are available at the park
       DESCRIPTION | A brief description of the park

An example of the json response would be:
 ```json
    [{"NAME": "Shantalla Park", 
    "LOCATION": "Seamus Quirke Road, Shantalla", 
    "FACILITIES": ["1 Soccer/ Gaa Playing Pitch", "Planting area with flowers", "shrubs and trees"], 
    "DESCRIPTION": "Local Neighbourhood Park"}]
 ```

### 4. _**List of available parks by the facilities they possess**_
The following URL provides a list of all the parks depending on the facilities the have.

**_THE URL:_** *```http://galwayparks.com/facilities/[:facilities]```* </br>
The [:facilities] represents the part of the URL being replaced depending on which information is provided.

**_METHOD:_** GET (The HTTP GET method is used for retrieving this information)

**_EXAMPLE:_** *```http://galwayparks.com/facilities/tenniscourts```* </br>
This will return a list of of all the parks in galway city that have tennis courts, along with other data.

      **PROPERTY** | **DESCRIPTION**
      ------------ | ------------
       NAME | The name of the park
       LOCATION | The location of the park, contains information like the street name.
       FACILITIES | The different facilities that are available at the park

An example of the json response would be:
 ```json
    [{"NAME": "Westside Park",
    "LOCATION": "Seamus Quirke Road, Newcastle, Galway", 
    "FACILITIES": ["2 Gaelic/ 2 Soccer Playing Pitches", "Basketball Court", "Tennis Courts", "Dressing Rooms", "Running  Track", "Skatepark"]}]
 ```
 
 The code above is an example of a single json object (park) returned by the URL. We can have many of these objects, each of these objects can then be stored inside another json object. This makes json a very powerful language in data transfer.

