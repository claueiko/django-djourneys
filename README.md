# DJANGO DJOURNEYS

## Python - Django Project

![DjangoDjourneys](https://user-images.githubusercontent.com/93624439/199771948-bca01777-3709-4f18-a833-15002177c5f9.gif)

Not sure where to travel next? Use our Quiz and get ideas for your next adventure. Or browse through the list of recommended places, read and write reviews, etc.

Django Djourneys was built in one week using Python and Django, PostreSQL, and Bootstrap, as well as Google Geolocation API and OpenWeather API.

[You can check out the deployed version here](https://djangodjourney.herokuapp.com) 

## Django Djourneys
Was built in one week as part of General Assembly Software Engineering Immersive Course. This was my 3rd (out of 4) project, in collab with Alexander Ward and Milos Jocic.

## Technologies used
Built on Python and Django, we also used PostgreSQL, Pillow, Bootstrap, jQuery, OpenWeather API and Google Geolocation API.

```
asgiref==3.5.2
dj-database-url==1.0.0
Django==4.1.1
django-heroku==0.3.1
gunicorn==20.1.0
Pillow==9.2.0
psycopg2==2.9.3
psycopg2-binary==2.9.3
python-dotenv==0.21.0
sqlparse==0.4.3
whitenoise==6.2.0
```


## Requirements 
A full stack web app with at least 2 related models, including *users* with authentication and authorization, and CRUD functions.

## General
My cohort mates and I decided to create a database with destinations and its details such as country, city or town and images. With the use of OpenWeather and Google Geolocation APIs, visitors can see the current weather data and the exact map location. Users can leave reviews and ratings if they're logged in. Visitors can also browse through all 250 destinations or take a fun quiz to see our recommendations.

## My contribution
We worked very well as a group and each one of us played a fundamental part in this project. I am very happy with the way it looks in general and I used Bootstrap and well as FontAwesome. I also believe I did a good job with the reviews and ratings, the use of APIs and pagination.

## Pagination
### Pagination index.html:
```
<div class="pagination-div">
    {% if dest.has_previous %}
<a class="pagination" href="?page=1">&laquo First</a>
<a class="pagination" href="? page={{ dest.previous_page_number }}">Previous</a>
{% endif %}
<p  class="pagination"> Page {{ dest.number }} of {{ dest.paginator.num_pages }}</p>
{% if dest.has_next %}
<a class="pagination" href="?page={{ dest.next_page_number }}">Next</a>
<a class="pagination" href="?page={{ dest.paginator.num_pages }} ">Last &raquo</a>
{% endif %}
</div>
```
### Pagination views.py (pip i paginatior)
```
destinations = Destination.objects.all()
    # PAGINATION
        p = Paginator(Destination.objects.all(), 15)
        page = request.GET.get('page')
        dest = p.get_page(page)
        return render(request, 'destinations/index.html', {'dest': dest, 'destinations': destinations})
```
![image](https://user-images.githubusercontent.com/93624439/200589187-1f80573a-5ebc-4afa-9857-141c5f2e8442.png)


## Ratings and Reviews
### Ratings and Reviews -> models.py

```
# AVERAGE REVIEWS AND COUNT:
    def averageReview(self):
        reviews = Review.objects.filter(destination=self).aggregate(average=Avg('rating'))
        avg = 0
        if reviews['average'] is not None:
            avg = int(reviews['average'])
        return avg

    def countReview(self):
        reviews = Review.objects.filter(destination=self).aggregate(count=Count('id'))
        count = 0
        if reviews['count'] is not None:
            count = int(reviews['count'])
        return count
    # END OF AVERAGE REVIEWS AND COUNT
```

### Ratings and Reviews -> detail.html

```
<!-- AVERAGE STARS -->
                <span class="show-ratings">
                <i class="fa fa-star{% if destination.averageReview < 0.5 %}-o{% elif destination.averageReview >= 0.5 and destination.averageReview < 1 %}-half-o {% endif %}" aria-hidden="true"></i>
                <i class="fa fa-star{% if destination.averageReview < 1.5 %}-o{% elif destination.averageReview >= 1.5 and destination.averageReview < 2 %}-half-o {% endif %}" aria-hidden="true"></i>
                <i class="fa fa-star{% if destination.averageReview < 2.5 %}-o{% elif destination.averageReview >= 2.5 and destination.averageReview < 3 %}-half-o {% endif %}" aria-hidden="true"></i>
                <i class="fa fa-star{% if destination.averageReview < 3.5 %}-o{% elif destination.averageReview >= 3.5 and destination.averageReview < 4 %}-half-o {% endif %}" aria-hidden="true"></i>
                <i class="fa fa-star{% if destination.averageReview < 4.5 %}-o{% elif destination.averageReview >= 4.5 and destination.averageReview < 5 %}-half-o {% endif %}" aria-hidden="true"></i>
                <span class="average-title">Average Rating: {{ destination.averageReview }} <span class="based-on-reviews">based on {{destination.countReview}} reviews</span></span>
                </span> <br><br><br>
<!-- END OF AVERAGE STARS -->
```

### Ratings and Reviews -> style.css

```
.rate > input {
    display:none;
}
.rate {
    display: inline-block;
    border: 0;
}
.rate > label {
    float: right;
}
/* Showing stars */
.rate > label:before{
    display: inline-block;
    font-size: 1.1rem;
    font-family: FontAwesome;
    content: "\f005";
    margin: 0;
    padding: 0.3rem .2rem;
    cursor: pointer;
}
/* half stars */
.rate .half:before{
    content: "\f089";
    position: absolute;
    padding-right: 0;
}
/* Click on hover */
input:checked ~ label, label:hover ~ label{
    color: #ffb503;
}
/* HOVER HIGHLIGHT */ 
input:checked + label:hover, input:checked ~ label:hover, input:checked ~ label:hover ~ label, label:hover ~ input:checked ~ label {
    color: #ffb503;
}
.fa-star {
    color: #ffb503;
}
.show-ratings {
    font-size: 22px;
}
.based-on-reviews {
    font-size: 18px;
    margin-left: 20px;
}
.average-title {
    margin-left: 20px;
}
```

![image](https://user-images.githubusercontent.com/93624439/200589026-ffb04063-532e-4b86-ba8a-7f23fe22281e.png)


## Weather and Google Maps API
### Weather -> script.js

```
// WEATHER API
function info() {
    let apiKey = "02b0e8be55f7e16deac161d58d733f1f";
    let unit = "metric";
    let city = document.getElementById("city");
    city = city.innerText;
    console.log(city);
    let apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=${unit}`;
    
    axios.get(apiUrl).then(updateApp);
}
function updateApp(response) {
    // celsiusTemperature = response.data.main.temp;
    let city = document.getElementById("city")
    city = city.innerText;
    console.log(city)
    let currentTemperature = Math.round(response.data.main.temp);
    let temperature = document.querySelector("#weather-temp");
    temperature.innerHTML = `${currentTemperature}°C`;
    let currentFeel = Math.round(response.data.main.feels_like);
    let feel = document.querySelector("#weather-feel");
    feel.innerHTML = `${currentFeel}°C`;
    let currentDescription = response.data.weather[0].description;
    let description = document.querySelector("#weather-desc");
    description.innerHTML = `${currentDescription.charAt(0).toUpperCase() + currentDescription.slice(1)}`;
    let iconElement = document.querySelector("#icon");
    iconElement.setAttribute("src", `/static/images/weather-icons/${response.data.weather[0].icon}.svg`)
}
info()
```

### Google Maps -> script.js
```
// MAP
function initMap() {
  var map = new google.maps.Map(document.getElementById("map"), {
    zoom: 8,
    center: { lat: -34.397, lng: 150.644 },
  });
  var geocoder = new google.maps.Geocoder();
geocodeAddress(geocoder, map);
};

function geocodeAddress(geocoder, resultsMap) {
  var address = document.getElementById("city");
  address = address.innerText;
  geocoder.geocode({ address: address }, function (results, status) {
    if (status === "OK") {
      resultsMap.setCenter(results[0].geometry.location);
      var marker = new google.maps.Marker({
        map: resultsMap,
        position: results[0].geometry.location,
      });
    } else {
      alert("Geocode was not successful for the following reason: " + status);
    }
  });
}
```

![image](https://user-images.githubusercontent.com/93624439/200589951-15be8659-b91e-4d10-be04-1436140d6b0c.png)


## ERD
![image](https://user-images.githubusercontent.com/93624439/200590850-d9f77241-1b71-484c-85b5-388104605356.png)

## Wireframes

### Homepage
![image](https://user-images.githubusercontent.com/93624439/200590976-07c2ab41-e60d-4dd2-a251-78a1342704a7.png)

### Results - Index Page
![image](https://user-images.githubusercontent.com/93624439/200591070-8a4c1be7-ab42-47d5-b62b-51f7e2044370.png)

### Destination Details Page
![image](https://user-images.githubusercontent.com/93624439/200591161-d260fc5b-f809-49d7-bba0-8662ce59ee99.png)

### Quiz Page
![image](https://user-images.githubusercontent.com/93624439/200591244-f81d13b4-2584-45be-a6ee-739afee75af3.png)




