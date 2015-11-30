title: 'Updating Data in CouchDb from c#/Asp.Net Mvc'
tags:
  - Asp.Net MVC
  - CouchDb
  - REST
id: 1901
categories:
  - Code Dive
  - Develop Meta
date: 2013-04-18 14:30:23
---

One of my repositories is working against the REST interface of a CouchDb set of data.&nbsp; Reads have been awesome, and the “Paste JSON as classes” feature of Visual Studio 2012 makes getting POCOs up and running a treat.

However I ran into the following error doing a POST to update a document in the database:

<font face="Lucida Console">couchdb {"error":"bad_request","reason":"Referer header required."}</font>

For anyone working in the REST space, this will likely be an error you would never run into as you have a great handle on your HTTP verbs.&nbsp; For us crossover guys – especially relational database, Asp.Net guys – this is a little harder to figure out what is going on.

I was fortunate enough to have had a conversation the other day with our partner on the project who had mentioned that we need to remember to keep our PUTs and POSTs in order.&nbsp; When I saw the “bad request” message above, I knew what I was doing wrong, especially after having a great conversation about REST with [Darrel Miller](https://twitter.com/darrel_miller) earlier this year.

When a document exists in CouchDb you send updates to the document URL with a PUT. To create a new entry, you use a POST to the document container and CouchDb will determine the newly created item’s URL.

Here is a generic method to update a document from c#:

![image](https://jcblogimages.blob.core.windows.net/img/2013/04/image.png "image")

Via Gist:
<script src="https://gist.github.com/MisterJames/5413118.js"></script>