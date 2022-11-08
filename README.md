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

###







