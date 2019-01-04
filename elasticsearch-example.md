---
layout: article
title: ElasticSearch example
---

In this article we will:
* Install Marija using Docker.
* Install ElasticSearch using Docker.
* Populate ElasticSearch with sample data (movie recommendations).
* Configure Marija to connect with ElasticSearch, using it as a datasource.

Prerequisites:
* Docker. For more info see this [guide on docker.com](https://docs.docker.com/get-started/).
* Python3.

## Installing Marija
Run the following command:
```
$ docker pull marija/marija
```

## Installing ElasticSearch
Run the following command:
```
$ docker pull docker.elastic.co/elasticsearch/elasticsearch:6.5.4
```

Now start ElasticSearch:
```
$ docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:6.5.4
```

## Populating ElasticSearch with movie recommendations

We will be using the data from the [ElasticSearch examples repo](https://github.com/elastic/examples/tree/master/Graph/movie_recommendations).

Download the required files:
```
$ curl -O https://raw.githubusercontent.com/elastic/examples/master/Graph/movie_recommendations/download_data.py -O https://raw.githubusercontent.com/elastic/examples/master/Graph/movie_recommendations/index_users.py -O https://raw.githubusercontent.com/elastic/examples/master/Graph/movie_recommendations/movie_lens.json -O https://raw.githubusercontent.com/elastic/examples/master/Graph/movie_recommendations/requirements.txt
```

Install the Python dependencies:
```
pip install -r requirements.txt
```

Download the movie data:
```
python3 download_data.py
```

Store the data in your ElasticSearch instance:
```
python3 index_users.py
```

To see if it worked, open your browser and go to
[localhost:9200/movie_lens_users/_count](http://localhost:9200/movie_lens_users/_count).
You should see a count of 138493.

## Configuring Marija

First we will need to find the IP address of ElasticSearch. Run `docker ps` to find
the container ID of your ElasticSearch container. Copy the container ID and then
run (while replacing below ID with your own container ID):
```
docker inspect a247ca20f6d1
```

You will find the IP address at `NetworkSettings` > `IPAddress`

Create a new file and name it `config.toml`. This is the Marija configuration
file. Paste the following into it:
```
[datasource]

[datasource.movie_lens_users]
type="elasticsearch"
# Replace the IP address below with the IP you found with docker inspect
url="http://172.17.0.3:9200/movie_lens_users"
username="elastic"
password="changeme"
```

## Trying it out!

Start Marija with:
```
$ docker run -d -p 8080:8080 -v $(pwd)/config.toml:/config/config.toml marija/marija
```

Visit Marija with your browser on [localhost:8080](http://localhost:8080). You
should now be able to search movies!

If anything went wrong, try looking at the logs of the Marija container. Run
`docker ps` to find your container ID, and then run `docker logs CONTAINERID`.