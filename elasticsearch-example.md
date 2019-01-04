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
* Docker. For more info on how to install see [docker.com](https://docs.docker.com/install/).
* Python3.

## Installing Marija
Run the following command:
```
$ docker pull nl5887/marija-enterprise
```

## Installing ElasticSearch
Run the following command:
```
$ docker pull docker.elastic.co/elasticsearch/elasticsearch:5.6.1
```

Now start ElasticSearch:
```
$ docker run -d -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:5.6.1
```
> This creates and runs a temporary ElasticSearch database. The data stored in it will not be persistent and deleted when it is stopped.

Verify that ElasticSearch is working by visiting [localhost:9200](http://localhost:9200) in your browser.

## Populating ElasticSearch with movie recommendations

We will be using the data from the [ElasticSearch examples repo](https://github.com/elastic/examples/tree/master/Graph/movie_recommendations).
The used files have been modified to make them work with a more recent version of ElasticSearch.

Download the required files:
```
$ curl -O https://docs.marija.io/assets/articles/elasticsearch-example/download_data.py -O https://docs.marija.io/assets/articles/elasticsearch-example/index_users.py -O https://docs.marija.io/assets/articles/elasticsearch-example/movie_lens.json -O https://docs.marija.io/assets/articles/elasticsearch-example/requirements.txt
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

To see if it worked, go to
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
$ docker run -d -p 8080:8080 -v $(pwd)/config.toml:/config/config.toml nl5887/marija-enterprise
```

Visit Marija with your browser on [localhost:8080](http://localhost:8080).
Because we're using the enterprise version, you will need to login. Use the
username `admin` and the password `admin`. You should now be able to search movies!

If anything went wrong, try looking at the logs of the Marija container. Run
`docker ps` to find your container ID, and then run `docker logs CONTAINERID`.