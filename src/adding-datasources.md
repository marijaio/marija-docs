---
layout: article
title: Adding datasources
---

Datasources are sources that Marija can search. Marija will make a direct
connection to your datasources, so there is no need to import or transform data.

The following types are supported:
* ElasticSearch
* Splunk
* Twitter
* Blockchain
* Censys

To add a datasource, you need to configure it in your `config.toml`.

### ElasticSearch
```
[datasource.emails]
type="elasticsearch"
url="http://172.17.0.1:9200/emails"
username="elastic" # Optional
password="changeme" # Optional
```

### Splunk
todo

### Twitter
```
[datasource.twitter]
type="twitter"
consumer_key=""
consumer_secret=""
token=""
token_secret=""
```

### Blockchain
```
[datasource.blockchain]
type="blockchain"
```

### Censys
todo

## Complete `config.toml` example
```
[datasource]

[datasource.spamtrap]
type="elasticsearch"
url="http://51.15.37.73:9200/spamtrap"

[datasource.censys]
type="censys"
api-id=""
api-secret=""

[datasource.honeytrap]
type="elasticsearch"
url="http://51.15.37.73:9200/honeytrap-nos"
username="elastic"
password="changeme"

[datasource.flow]
type="elasticsearch"
url="http://127.0.0.1:9209/flow"

[datasource.twitter]
type="twitter"
consumer_key=""
consumer_secret=""
token=""
token_secret=""

[datasource.blockchain]
type="blockchain"

[[logging]]
output = "stdout"
level = "debug"
```

## Running Marija

To run Marija with your `config.toml` use the following command:
```
docker run -d -p 8080:8080 -v $(pwd)/config.toml:/config/config.toml marija/marija
```

Now you can visit Marija on `localhost:8080` and you should be able to search
your newly added datasources.

## Further reading

If you need more help with the installation of Marija, check out the
[installation guide](/installation.html).

For an in-depth guide of how to setup both ElasticSearch and Marija and how to make
your ElasticSearch instance searchable by Marija, check out the
[ElasticSearch example](/elasticsearch-example.html).