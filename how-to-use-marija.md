---
layout: article
title: How to use Marija
---

# How to use Marija
Marija is a data visualisation tool. It visualises the data through network graphs,
a map and a timeline. It can help you find meaningful relationships in large amounts of data.

In this article we will explore the different features of Marija. We will import
a small CSV containing some emails. We will try to find out who is sending the
most emails and which people are often communicating with each other.

## Getting our CSV
You can download the CSV from [docs.marija.io/assets/resources/emails.csv](/assets/resources/emails.csv).

Let's take a quick look at our CSV:

![Emails CSV](/assets/images/emails-csv.png)

The CSV contains a list of emails. Each email has a sender, recipient, date,
subject and body. The subjects and bodies are automatically generated random text.

> Note that no relations are explicitly defined in the CSV. Marija will automatically identify these relations for us.

## Importing the CSV into Marija

First click on `Config`, then scroll down until you see `CSV Datasources`.
Then click on `Create CSV datasource`.
![Create CSV datasource](/assets/images/create-csv-datasource.png)