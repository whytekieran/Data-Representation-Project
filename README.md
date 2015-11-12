# _**Data Representation Project**_

**Student:** Ciaran Whyte </br>
**Module:** Data Representation and Querying </br>
**Lecturer:** Ian Mcloughlin </br>

## _**Introduction**_

This project was designed as part of a Data Representation module. In the project I have to design an API for a data set, not implement it. I will be designing the URLs for querying a particular data set for useful information, keeping in mind the needs of the users that will use the API. These datasets are located at [Data.Gov.ie] (https://data.gov.ie/data) The API will be provided through HTTP. Each of the URLs will be described in detail explaining certain points like what kind of data will be returned from the URL and what kind of HTTP method(s) are being used.

## _**The Data Set**_
The data set i will be using contains data about the variety of public parks located around Galway city. Exercise is vital today for good health from both a physical and mental perspective. Therefore i find it very important that people should be aware of the different facilities they have in their local area. Currently this data set contains data for all the parks in Galway city but this could be expanded to contain all the parks in the county, province or country. It also would not have to just be for parks, it could also contain gyms, sport pitches etc. The main concept of this being all about exercise and awareness of public facilities. The data set contains different information about each park. It is in CSV file format and has 29 rows, each row contains values for a differant park. If you wish to view the actual dataset you can download it here: [Parks In Galway City] (http://opendata.galwaycity.opendata.arcgis.com/datasets/683ff500430447c985f4775a6b5dd112_0.csv)

### _**The Data Set Fields and a Description**_
**FIELD** | **DESCRIPTION**
------------ | -------------
ObjectID | The object id of the park
Number | The parks unique number
Name | The name of the park
Location | The location of the park
AreaOfCity | The area of the city the park is in (east, west, etc)
OpeningHRs | The opening hours of the park
Facilities | The facilities available in the park
Description | Give a general description of the park.
Latitude | Latitude describes the Y co-ordinate of the park on a map.
Longitude | Longitude describes the X co-ordinate of the park on a map.
EastITM | Gives the EastITM of the park.
NorthITM | Gives the NorthITM of the park.
EastIG | Gives the EastIG of the park.
NorthIG | Gives the NorthIG of the park.

## _**Common HTTP Methods and a brief description**_

**HTTP METHOD** | **DESCRIPTION**
------------ | -------------
GET | The GET method means retrieve whatever information is identified by the Request-URL. If the Request URL itself refers to some sort of a data producing process the produced data will be returned as the object in the response. An interesting note about the GET method is that it can be both a silent GET method or a parameterized GET. All the examples in this documentation use the silent version of GET which is structred like the following: <br/> ```/[:SomeSearchData]``` <br/> This version is becoming increasingly more common today. The parametized version has a structure like this: <br/> ```/parks/parks_form?key1=value1&key2=value2```
POST | The POST method is used to send data to the server. The actual function that the POST method performs is determined by the server and is most likely dependent on the URL.
HEAD | The HEAD method is very similar to the GET method in what it does. The main difference being it only returns the response header.
PUT | Sets the data in the URL to that of the requested data. PUT will put a file or resource at a specific URL. If there is a file or resource already at the URL, PUT replaces that file or resource. If there is no file or resource there, PUT creates one
DELETE | Deletes the data specified in the URL

## _**Handling the URLs**_
Much of the Javascript that we write to handle these URLs can be written with a special library in Javascript we call JQuery.
JQuery is sometimes donated by using the $ sign. JQuery contains functions which allow us to work with a URL. Here is a general example using the JQuery get() method to retrieve data in a URL.
 
```
<script type="text/javascript" src="jquery-2.1.4.min.js"></script>//We need to import the JQuery Library

 var url = "urlWithSomething/file/someData";
  $.get( url, function(data1){ //We can use JQuery.get() to retrieve data from the URL
    var url1 = "urlWithSomethingMoreSpecific/file/data1";
     $.get(url1, function(data2){ //Inner .get() on more specific URL
       console.log(data2.attribute);//Output the data to the console, the data can come in many different models, eg XML,JSON
        $('#id').html(data2.attribute); //Using another JQuery method .html() to get a handle on a DOM object
     });
     console.log(data1); //Output less specific data to the console
});
```

This function uses the get() method from the JQuery library. It retrieves a URL and the data it contains. These get() methods are nested inside one another. We use the first get() method to grab data from the URL which could be something general like all the parks ID numbers. Then using the nested get() method we can pass in the URL containing more specific data. This could be the name of the park, its location etc. 

If we had many parks which is most likely, then we will have many ID numbers. So to get information about every park we could loop through the data supplied from the anonymous function inside the first get() method like the following:

```javascript
   for(var i = 0; i < 5; ++i)
   {
     var url2 = "urlWithSomethingMoreSpecific/file/data1[i]";
      $.get(url2,function(data2){ //Inner .get() on more specific URL
        $('#id').html(data2.attribute); //Using another JQuery method .html() to get a handle on a DOM object
     });
    }
 ```
 This for loop would go inside the first get() method that was shown in the previous code example and could be used to loop over every parks id and then get more specific information about each park. JQuery is a very useful library when creating HTTP API's providing a quick and efficient way to retrieve useful data from a URL. More information about JQuery and their documentation can be found at their website [JQuery.com] (https://jquery.com) <br/>
JQuery is just one way we can grab data from a URL, another way is to use Node.js which is also based on javascript. We can build web servers quickly and easily using Node.js or even better we can use Node Express which a web framework created for Node.js. Later in this documentation we will show some examples of how we can use Node Express to work with data in URLs.
 
### _**The Client Side URLs**_

#### 1. _**Entering the site**_ 
On entering the site, if your a member you may need to provide some authentication details. The following is an example of the URL

**_THE URL:_** *```http://galwayparks.com/login```* </br>

The code below is an example of the massage body sent by the HTTP POST method.  

 ```
   POST /entry/login_form HTTP/1.1
   User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)
   Host: GalwayParks.com
   user=Mark&password=enter123
 ```
**_METHOD:_** POST (The HTTP POST method is used for sending this information, as you can see in the first line of the HTTP message body we say POST. The last line refers to the information we are sending. This information is sensitive and therefore would not be sent in the URL, which is why we use the POST method.)

#### 2. _**List of available parks by general city area (East, West etc)**_
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
       AREA | The area of the city in which the park is located
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

#### 3. _**List of available parks by city location**_
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

#### 4. _**List of available parks by the facilities they possess**_
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

### _**The Administrative URLs**_

#### 1. _**Adding a new park to the dataset**_ 
There may be a time when a new park is built in Galway. If this occurs the administrators of the site should be able to add that new park to the dataset. The following is an example of the URL to add a new park.

**_THE URL:_** *```http://galwayparks.com/newpark```* </br>

The code below is an example of the massage body sent by the HTTP POST method. As you can see the values are inside quotes, this isnt mandatory. In place of the spaces between the words inside these quotes we will get a % symbol which is produced by HTML. We can then search for the % symbol when we want to split these words apart and do some processing with them. 

 ```
   POST /addpark/newpark_form HTTP/1.1
   User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)
   Host: GalwayParks.com
   parkname="Galway Park"&location="Main St"&cityarea="City-West"&openinghours="No restricted hours"&facilities="TennisCourts BasketballCourts"&description="Local neighbourhood park"
 ```
**_METHOD:_** POST (The HTTP POST method is used for sending this information. The last line refers to the information we are sending. This information is sensitive, when we are adding a new park on the administrative side we do not want this information to be inside the URL like in the HTTP GET method. If it was everybody could see it. Therefore for security reasons we use the HTTP POST method)

#### 2. _**List all the parks in the dataset**_
The following URL provides a list of all the Galway parks contained in the dataset and their unique ID's. On the client side getting the parks ID may not be very important, but being able to view a parks unique ID may be very useful from an administrative point of view. An example may be if we want to remove a specific park. We could then use the ID to specify which park we want to remove.

**_THE URL:_** *```http://galwayparks.com/parks```* </br>

**_METHOD:_** GET (The HTTP GET method is used for retrieving this information)

      **PROPERTY** | **DESCRIPTION**
      ------------ | ------------
       NUMBER | The unique ID given to the park
       NAME | The name of the park
       LOCATION| The location of the park, contains information like the street name.

An example of the json response would be:
 ```json
    [{"NUMBER": "5",
    "NAME": "Salthill Park", 
    "LOCATION": "Salthill, Galway"}]
```

Earlier in this documentation we showed how JQuery can be used and briefly discussed Node Express, here we will show an example of how Node Express can be used to retrieve data depending on the URL described above. If you wish to learn more about Node Express and its capabilities you can visit their website [Node Express] (http://expressjs.com)

 ```
   //Creates an Express application. The express() function is a top-level function exported by the express module.
    var express = require('express');
    var app = express();
    
    //Listening for a particular URL and HTTP GET method. If URL format is something like GalwayParks/parks then
    //the anonymous function would execute
    app.get('/parks', function (req, res) {
     //Do stuff, gets all the parks in the dataset
     //This data could be in a relational database.
});

var server = app.listen(80); //Listen on port 80
```
This code would correspond to the URL for the previous HTTP GET method shown above in the example URL

#### 3. _**Deleting a park from the dataset**_ 
Just as there may be parks built in Galway city there may also be parks that are being removed, for example a park may be taken down and housing built in its place. In this situation the administator may want to remove a park from the dataset.
The following is an example of a URL to delete a park from the dataset.

**_THE URL:_** *```http://galwayparks.com/removepark/[:id]```* </br>
The [:id] represents the part of the URL being replaced depending on which id is provided for a particular park.

**_METHOD:_** DELETE (The HTTP DELETE method is used for deleting certain information from the dataset)

**_EXAMPLE:_** *```http://galwayparks.com/removepark/1```* </br>
This URL would be used to remove the park with an ID of 1, every park in the dataset has a unique ID that we use to identify it. In relational databases we would refer to this ID as a primary key. This URL would not retrieve any data, simply remove the data we specify. 

Here is another example using Node Express. Using Express we can specify what to do when using a HTTP DELETE method and if this method gets a specific url. Below is an example of the possible server side code.

 ```
    var express = require('express');
    var app = express();
    
    app.delete('/1', function (req, res) {
     //Do stuff, delete the park with ID of 1, gets the value of one from the URL
     //This data could be in a relational database.
});

var server = app.listen(80); //Listen on port 80
```
This code would correspond to the URL for the HTTP DELETE method shown above in the example URL.

### _**Closing Statements**_
As you can see above we have provided sample URLs for both the client side and administrative side of the application. I personally believe that an application like above would promote healthy living and hopefully encourage people to get out and exercise by providing the public with information about the local services that are availible to them.
