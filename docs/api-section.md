---
layout: default
title: API Section
nav_order: 2
---
# **Getting Started**
<br>
Welcome to our API.

This API document is designed for those interested in developing for our platform.

This API is still under development and will evolve.
<div class="edit-text">
You’ll succeed if you do this.
</div>
{: .alert .alert-success}
<br>
<div class="edit-text">
Here’s some useful information.
</div>
{: .alert .alert-info}
<br>
<div class="edit-text">
Something may not happen if you try and do this.
</div>
{: .alert .alert-warning}
<br>
<div class="edit-text">
Something bad will happen if you do this.
</div>
{: .alert .alert-danger}
<style>
  .alert-success{
    border-left: 5px solid #5cb85c;
    height:50px;
    border-top-left-radius:2px;
    border-bottom-left-radius:2px;
    background-color: rgb(248, 248, 248);
  }
  .alert-info{
    border-left: 5px solid #5bc0de;
    height:50px;
    border-top-left-radius:2px;
    border-bottom-left-radius:2px;
    background-color: rgb(248, 248, 248);
  }
  .alert-warning{
    border-left: 5px solid #f0ad4e;
    height:50px;
    border-top-left-radius:2px;
    border-bottom-left-radius:2px;
    background-color: rgb(248, 248, 248);
  }
  .alert-danger{
    border-left: 5px solid #d9534f;
    height:50px;
    border-top-left-radius:2px;
    border-bottom-left-radius:2px;
    background-color: rgb(248, 248, 248);
  }
  .edit-text{
    padding: 7px 0px 0px 20px;
    font-size:20px;
  }
  table, td, th{
    font-size:30px;
    border-collapse: collapse;
  }
  td{
    font-size: 30px;
  }
  th{
    background-color:rgb(248, 248, 248);
  }
  
  </style>

<hr>

# **Authentication**
<br>
You need to be authenticated for all API requests. You can generate an API key in your developer dashboard.

Add the API key to all requests as a GET parameter.

<div class="edit-text">
Nothing will work unless you include this API key
</div>
{: .alert .alert-danger}

<hr>

# **Error**

<table>
<th>Code</th>
<th>Name</th>
<th>Description</th>

<tr>
<td>200</td>
<td>OK</td>
<td>Success</td>
</tr>
<tr>
<td>201</td>
<td>Created</td>
<td>Creation Successful</td>
</tr>
<tr>
<td>400</td>
<td>Bad Request</td>
<td>We could not process that action</td>
</tr>
<tr>
<td>403</td>
<td>Forbidden</td>
<td>We couldn’t authenticate you</td>
</tr>
</table>

 All errors will return JSON in the following format:

### [Response](#)

```json
{
  "error": true,
  "message": "error message here"
}
```
<style>
.color-success{
    color:#5cb85c;
    font-size: 23px;
  }
  .color-info{
    color:#5bc0de;
    font-size: 23px;
  }
  .color-rebeccapurple{
    color:rebeccapurple;
    font-size: 23px;
  }
  .color-warning{
    color:#f0ad4e;
    font-size: 23px;
  }

</style>

# **/books   <span class="color-success">GET</span>**
List all books

### Parameters


 **offset**      |              Offset the results by this amount
                 | 
**limit**        |             Limit the number of books returned
<div class="edit-text">
This call will return a maximum of 100 books
</div>
{: .alert .alert-info}

Lists all the photos you have access to. You can paginate by using the parameters listed above.

## jQuery
```js
$.get("http://api.myapp.com/books/", { "token": "YOUR_APP_KEY"}, function(data) {
  alert(data);
});
```
## Python
```py
r = requests.get("http://api.myapp.com/books/", token="YOUR_APP_KEY")
print r.text
```

## Node.js
```js
request("http://api.myapp.com/books?token=YOUR_APP_KEY", function (error, response, body) {
if (!error && response.statusCode == 200) {
  console.log(body);
}
```

## Curl
```c
curl http://sampleapi.readme.com/orders?key=YOUR_APP_KEY
```
<style>
.grey{
  opacity:0.7;
  font-size:20px;
}
</style>
# **/books   <span class="color-info">POST</span>**
<span class="grey">Create Book</span>

### Parameters


 **offset**   |                 Offset the results by this amount
              |
**limit**     |                Limit the number of books returned

<div class="edit-text">
The book will automatically be added to your reading list
</div>
{: .alert .alert-success}

Adds a book to your collection.

## **jQuery**

```js
$.post("http://api.myapp.com/books/", {
  "token": "YOUR_APP_KEY",
  "title": "The Book Thief",
  "score": 4.3
}, function(data) {
  alert(data);
});
```


# **/books/:id   <span class="color-success">GET</span>**
<span class="grey">Get Book</span>

Returns a specific book from your collection

## **jQuery**

```js
$.get("http://api.myapp.com/books/3", {
  token: "YOUR_APP_KEY",
}, function(data) {
  alert(data);
});
```

# **/books/:id   <span class="color-rebeccapurple">PUT</span>**

<span class="grey">Update Book</span>

### Parameters


  **title**|                 The title for the book
           |           
 **score** |                 The book's score between 0 and 5

Returns a specific book from your collection

## **jQuery**

```js
$.ajax({
  "url": "http://api.myapp.com/books/3",
  "type": "PUT",
  "data": {
    "token": "YOUR_APP_KEY",
    "score": 5.0,
    "title": "The Book Stealer"
  },
  "success": function(data) {
    alert(data);
  }
}); 
```

# **/books/:id   <span class="color-warning">DELETE</span>**
<span class="grey">Deletes a book</span>

Deletes a book in your collection.

## **jQuery**

```js
$.ajax({
  "url": "http://api.myapp.com/books/3",
  "type": "DELETE",
  "data": {
    "token": "YOUR_APP_KEY"
  },
  "success": function(data) {
    alert(data);
  }
});
```


