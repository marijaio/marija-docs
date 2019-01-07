---
layout: article
title: ElasticSearch example
---

In this article we will:
* Install Marija, ElasticSearch and Kibana using Docker Compose.
* Populate ElasticSearch with sample data (movie recommendations).
* Configure Marija to connect with ElasticSearch, using it as a datasource.

Prerequisites:
* Docker. For more info on how to install see [docker.com](https://docs.docker.com/install/).
* Python3.

## Running ElasticSearch, Marija and Kibana
We will download and run the above three applications using Docker Compose.

Create the file `docker-compose.yml`. This is the configuration file for
Docker Compose, where we specify which services we will run. Paste the following
contents into it:
```
---
version: '2'
services:

  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:5.6.1
    volumes:
      - /usr/share/elasticsearch/data
    ports:
      - 9200:9200

  kibana:
    image: docker.elastic.co/kibana/kibana:5.6.1
    links:
      - elasticsearch
    depends_on:
      - elasticsearch
    ports:
      - 5601:5601

  marija:
    image: nl5887/marija-enterprise
    links:
      - elasticsearch
    depends_on:
      - elasticsearch
    ports:
      - 8080:8080
    volumes:
      - ./marija-config.toml:/config/config.toml
```

Next we will create the configuration file for Marija. Here we specify which
ElasticSearch datasources will be searchable from Marija. Create the file
`marija-config.toml` and paste the following contents into it:
```
[datasource]

[datasource.movie_lens_users]
type="elasticsearch"
url="http://elasticsearch:9200/movie_lens_users"
username="elastic"
password="changeme"
```

To start all containers run:
```
docker-compose up
```

Now all three applications should be up and running. Try them out in your browser:
- Kibana: [localhost:5601](http://localhost:5601). Default login: **elastic** / **changeme**
- ElasticSearch: [localhost:9200](http://localhost:9200) Default login: **elastic** / **changeme**
- Marija: [localhost:8080](http://localhost:8080) Default login: **admin** / **admin**

Note that Marija might be working, but it does not have any datasources.
This is because we have not stored our sample data in ElasticSearch yet.

> If you experience problems, it might be because Docker is configured by default
> to limit its memory usage to 2 GB. These applications together will use more than
> that. You can edit the memory limit in the Docker preferences pane, under `Advanced`.
> Try setting it to 5GB.

## Populating ElasticSearch with movie recommendations

We will be using the data from the [ElasticSearch examples repo](https://github.com/elastic/examples/tree/master/Graph/movie_recommendations).
The used files have been modified to make them work with a more recent version of ElasticSearch.

Download the required files:
```
$ curl -O https://docs.marija.io/assets/articles/elasticsearch-example/download_data.py -O https://docs.marija.io/assets/articles/elasticsearch-example/index_users.py -O https://docs.marija.io/assets/articles/elasticsearch-example/movie_lens.json -O https://docs.marija.io/assets/articles/elasticsearch-example/requirements.txt
```

Install the Python dependencies:
```
$ pip install -r requirements.txt
```

Download the movie data:
```
$ python3 download_data.py
```

Store the data in your ElasticSearch instance:
```
$ python3 index_users.py
```

To see if it worked, go to
[localhost:9200/movie_lens_users/_count](http://localhost:9200/movie_lens_users/_count).
You should see a count of 138493.

## Restarting Marija

Because we have added a new datasource, we will need to restart Marija.
Do this with the following command:
```
$ docker-compose restart marija
```

When the restart is complete, visit Marija in your browser again
([localhost:8080](http://localhost:8080)), and refresh
the page.

Now you should be able to search the movie recommendations database!