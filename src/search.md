---
layout: article
title: Search
---

If you open Advanced search by clicking on the `Advanced` button, you will see
there are some additional features available for search. In this article we will
explain these search features.

## Date filter

![Date filter](/assets/articles/search/date-filter.png)

It's important to note the paragraph at the bottom here. `Using the fields:...`
Because an object in your datasource can have multiple date fields, you will
need to specify which of those date fields you would like to use to filter on
dates. Marija automatically selects a date field for you, one per object type.

To change the date fields, click on the config link to open the config:

![Date field config](/assets/articles/search/date-field-config.png)

Here you can change the date field for every object type.

## Searching an area on the map

To search an area on the map you will first need to activate the map. The video
below illustrates how to do this:

{% include loopingVideo.html src="/assets/articles/search/search-on-map.mp4" %}

Similar as described above for the date filter, you need to specify which
location field Marija is using to filter objects based on location. Marija
automatically selects a location field if one is available. If you have multiple
location fields, you can change the used one in the config.

## Grouping

For large datasets it can be useful to group your results.

Let's say we have 2 users available in our dataset:
```
first_name: john
last_name: smith
count: 1
```

```
first_name: ben
last_name: smith
count: 1
```

If we would now group on the field `last_name`, we would only see 1 result:
```
first_name: [john, ben]
last_name: smith
count: 2
```

The count property reflects how many objects a node represents. The count is
displayed in the node. The size of the node will also increase if it has a
higher count:

![Grouped node](/assets/articles/search/grouped-node.png)




## Query syntax

For every datasource type specific query syntaxes are available. Here we will
list some examples per datasource type.

### ElasticSearch

```
(new york city) OR (big apple)
(new york city) AND (police)
(last_name:Smith) AND (first_name:John)
```

For more information see the [guide on elastic.co](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax).