---
layout: article
title: Connectors
---

Connectors are a special type of nodes in Marija. They create the links between
objects.

Connectors are always displayed as square nodes, while objects are
always displayed as round nodes.

![Connectors explained](/assets/articles/how-to-use-marija/connectors-explained.png)

There are 2 purple round nodes here. These are objects. There is also 1 square
blue node. This is a connector.

The blue connector in this example has the data `to: liam@email.com`. 2 Purple object nodes are
connected to it. This means that both of these object nodes are emails
that are written to liam@email.com.

## Defining connectors

Marija will automatically attempt to define connectors for you. It does this
mainly by searching for values that are the same for many objects. When it
appears that using a certain field as a connector would create many relations
between objects, Marija automatically selects it.

You can also manually define fields as connectors. Open the `Config` to do this.

Scroll down to the `Fields` section and click on the plus icon to add it as
a connector. For greater control over your graph you can also disable
automatically defining connectors by unticking the box.

{% include loopingVideo.html src="/assets/articles/connectors/defining-connectors.mp4" %}

## Advanced connector settings

Click on the `Settings` button to open up the settings for a connector.
Different types of fields will give you different options.

### Text fields
![Text connector](/assets/articles/search/text-connector.png)

For connectors with text fields you have the option to select a similarity.
Strings that are very similar, for example `crocodile` and `crocodil`, can be
matched if you select a minimum similarity of let's say 80%.

You can also add a regular expression with a replace value. If you would enter
`big apple` as a regular expression, and `new york` as a replace value, you
would create a match between nodes with the name `new york` and `big apple`.

### Location fields
![Location connector](/assets/articles/search/location-connector.png)

For connectors with location fields you can select a maximum distance.

### IP fields
![IP connector](/assets/articles/search/ip-connector.png)

For connectors with IP fields you have the option to select a subnet mask in
which both IP addresses need to be.

## Combining fields in 1 connector

You can combine multiple fields into a single connector. Drag fields from a connector
into a different connector to combine them. In the connector settings
you will get a choice to either **Match at least one** or **Match all**.

#### Match at least one
A good example for when you would want to use this is if your data objects look
like this:
```
from: william@email.com
to: john@email.com
subject: important message
```

```
from: john@email.com
to: ben@email.com
subject: another message
```

Here you could choose to make a connector out of the `from` and `to` field.
When one of those would match, a relation to the same connector would be created.
This will give you a graph where every email occurs just once, regardless of
whether it is used in the context of sending or receiving emails.

#### Match all
A good example for when you would want to use this is if your data objects look
like this:
```
city: amsterdam
street: damrak
number: 10
```

```
city: amsterdam
street: damrak
number: 30
```

If you would create a connector for the fields `city`, `street`, and `number`,
and select **Match all**, a connector would not be created between these 2
objects, because their `numbers` are different.