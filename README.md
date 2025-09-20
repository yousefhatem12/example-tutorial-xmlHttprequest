# XMLHttpRequest Complete Documentation

## Table of Contents
1. [Introduction](#introduction)
2. [Basic Syntax](#basic-syntax)
3. [Properties](#properties)
4. [Methods](#methods)
5. [Events](#events)
6. [ReadyState Values](#readystate-values)
7. [Status Codes](#status-codes)
8. [Complete Example](#complete-example)
9. [Advanced Examples](#advanced-examples)
10. [Best Practices](#best-practices)
11. [Common Errors](#common-errors)

## Introduction

XMLHttpRequest is a JavaScript object used to make HTTP requests to servers. It allows you to:
- Fetch data from APIs
- Send data to servers
- Update web pages without reloading
- Handle different data formats (JSON, XML, text)

## Basic Syntax

```javascript
// Create XMLHttpRequest object
var xhr = new XMLHttpRequest();

// Configure the request
xhr.open('METHOD', 'URL', async);

// Set up response handler
xhr.onreadystatechange = function() {
    if (xhr.readyState === 4 && xhr.status === 200) {
        // Handle success
        console.log(xhr.responseText);
    }
};

// Send the request
xhr.send(data);
```

## Properties

### `readyState`
Current state of the request:
- `0` - UNSENT: Request not initialized
- `1` - OPENED: Server connection established
- `2` - HEADERS_RECEIVED: Request received
- `3` - LOADING: Processing request
- `4` - DONE: Request finished and response ready

### `status`
HTTP status code:
- `200` - OK
- `404` - Not Found
- `500` - Internal Server Error

### `responseText`
Response data as string

### `responseXML`
Response data as XML document

### `response`
Response data (can be various types)

## Methods

### `open(method, url, async, user, password)`
Initializes the request:
- `method`: HTTP method (GET, POST, PUT, DELETE)
- `url`: Request URL
- `async`: Asynchronous (true/false)
- `user`: Optional username
- `password`: Optional password

```javascript
xhr.open('GET', 'https://api.example.com/data', true);
```

### `send(data)`
Sends the request:
- `data`: Optional data to send (for POST requests)

```javascript
xhr.send(); // GET request
xhr.send(JSON.stringify({name: 'John'})); // POST request
```

### `setRequestHeader(header, value)`
Sets HTTP headers:

```javascript
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.setRequestHeader('Authorization', 'Bearer token123');
```

### `abort()`
Cancels the request:

```javascript
xhr.abort();
```

## Events

### `onreadystatechange`
Fired when readyState changes:

```javascript
xhr.onreadystatechange = function() {
    console.log('Ready state:', xhr.readyState);
};
```

### `onload`
Fired when request completes successfully:

```javascript
xhr.onload = function() {
    if (xhr.status === 200) {
        console.log('Success:', xhr.responseText);
    }
};
```

### `onerror`
Fired when an error occurs:

```javascript
xhr.onerror = function() {
    console.log('Request failed');
};
```

### `ontimeout`
Fired when request times out:

```javascript
xhr.timeout = 5000; // 5 seconds
xhr.ontimeout = function() {
    console.log('Request timed out');
};
```

## ReadyState Values

| Value | State | Description |
|-------|-------|-------------|
| 0 | UNSENT | Request not initialized |
| 1 | OPENED | Connection established |
| 2 | HEADERS_RECEIVED | Headers received |
| 3 | LOADING | Downloading response |
| 4 | DONE | Operation complete |

## Status Codes

### Success Codes
- `200` - OK
- `201` - Created
- `204` - No Content

### Client Error Codes
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found

### Server Error Codes
- `500` - Internal Server Error
- `502` - Bad Gateway
- `503` - Service Unavailable

## Complete Example

Here's the code from the image with detailed explanation:

```javascript
function getApi() {
    // Step 1: Create XMLHttpRequest object
    var xhttp = new XMLHttpRequest();
    
    // Step 2: Set up response handler
    xhttp.onreadystatechange = function() {
        // Check if request is complete and successful
        if (this.readyState == 4 && this.status == 200) {
            // Step 3: Get response as string
            var result = this.responseText; // string
            
            // Step 4: Parse JSON string to JavaScript object
            var mohamed = JSON.parse(result); // string to array of objects
            
            // Step 5: Extract products array
            var ali = mohamed.products;
            
            // Step 6: Initialize HTML string
            var myText = "";
            
            // Step 7: Loop through products
            for (var i = 0; i < ali.length; i++) {
                // Step 8: Create HTML for each product
                var cartoona = `
                    <div class="col-md-4 text-center mb-5">
                        <img src="${ali[i].images[0]}" class="w-100" style="height:350px">
                        <h4>${ali[i].title}</h4>
                        <h6>${ali[i].category}</h6>
                        <h6>${ali[i].rating} of 5</h6>
                        <h6>${ali[i].price}$</h6>
                    </div>
                `;
                
                // Step 9: Add to main HTML string
                myText = myText + cartoona;
            }
            
            // Step 10: Display results
            document.querySelector(".test").innerHTML = myText;
        }
    };
    
    // Step 11: Initialize GET request
    xhttp.open("GET", "https://dummyjson.com/products", true);
    
    // Step 12: Send request
    xhttp.send();
}
```

## Advanced Examples

### POST Request with Data
```javascript
function sendData() {
    var xhr = new XMLHttpRequest();
    
    xhr.onreadystatechange = function() {
        if (xhr.readyState === 4 && xhr.status === 201) {
            var response = JSON.parse(xhr.responseText);
            console.log('Created:', response);
        }
    };
    
    xhr.open('POST', 'https://jsonplaceholder.typicode.com/posts', true);
    xhr.setRequestHeader('Content-Type', 'application/json');
    
    var data = JSON.stringify({
        title: 'My Post',
        body: 'Post content',
        userId: 1
    });
    
    xhr.send(data);
}
```

### Request with Error Handling
```javascript
function getDataWithErrorHandling() {
    var xhr = new XMLHttpRequest();
    
    xhr.onload = function() {
        if (xhr.status >= 200 && xhr.status < 300) {
            // Success
            var data = JSON.parse(xhr.responseText);
            console.log('Success:', data);
        } else {
            // HTTP error
            console.log('HTTP Error:', xhr.status);
        }
    };
    
    xhr.onerror = function() {
        console.log('Network Error');
    };
    
    xhr.ontimeout = function() {
        console.log('Request Timeout');
    };
    
    xhr.timeout = 10000; // 10 seconds
    xhr.open('GET', 'https://api.example.com/data', true);
    xhr.send();
}
```

### Multiple Sequential Requests
```javascript
function getSequentialData() {
    function makeRequest(url, callback) {
        var xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function() {
            if (xhr.readyState === 4 && xhr.status === 200) {
                callback(JSON.parse(xhr.responseText));
            }
        };
        xhr.open('GET', url, true);
        xhr.send();
    }
    
    // First request
    makeRequest('https://api.example.com/users/1', function(user) {
        console.log('User:', user);
        
        // Second request using data from first
        makeRequest(`https://api.example.com/posts?userId=${user.id}`, function(posts) {
            console.log('User Posts:', posts);
        });
    });
}
```

### Loop Through Multiple APIs
```javascript
function getMultipleUsers() {
    var users = [];
    var completed = 0;
    var userIds = [1, 2, 3, 4, 5];
    
    userIds.forEach(function(id) {
        var xhr = new XMLHttpRequest();
        
        xhr.onreadystatechange = function() {
            if (xhr.readyState === 4 && xhr.status === 200) {
                users.push(JSON.parse(xhr.responseText));
                completed++;
                
                // Check if all requests completed
                if (completed === userIds.length) {
                    console.log('All users:', users);
                    displayUsers(users);
                }
            }
        };
        
        xhr.open('GET', `https://jsonplaceholder.typicode.com/users/${id}`, true);
        xhr.send();
    });
}
```

## Best Practices

### 1. Always Check Both ReadyState and Status
```javascript
if (xhr.readyState === 4 && xhr.status === 200) {
    // Process response
}
```

### 2. Handle Errors Properly
```javascript
xhr.onerror = function() {
    console.error('Request failed');
};
```

### 3. Set Appropriate Headers
```javascript
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.setRequestHeader('Accept', 'application/json');
```

### 4. Use Timeout for Long Requests
```javascript
xhr.timeout = 10000; // 10 seconds
xhr.ontimeout = function() {
    console.log('Request timed out');
};
```

### 5. Parse JSON Safely
```javascript
try {
    var data = JSON.parse(xhr.responseText);
} catch (error) {
    console.error('Invalid JSON:', error);
}
```

## Common Errors

### 1. CORS Error
**Problem:** Cross-Origin Resource Sharing blocked
**Solution:** Server must allow your domain or use JSONP

### 2. 404 Not Found
**Problem:** URL doesn't exist
**Solution:** Check URL spelling and endpoint

### 3. Parsing Error
**Problem:** Invalid JSON response
**Solution:** Validate JSON before parsing

### 4. Network Error
**Problem:** No internet connection
**Solution:** Add error handling and retry logic

### 5. Timeout Error
**Problem:** Request takes too long
**Solution:** Increase timeout or optimize server response

## Modern Alternatives

While XMLHttpRequest works well, consider modern alternatives:

### Fetch API
```javascript
fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error('Error:', error));
```

### Axios Library
```javascript
axios.get('https://api.example.com/data')
    .then(response => console.log(response.data))
    .catch(error => console.error('Error:', error));
```

## Summary

XMLHttpRequest is a powerful tool for making HTTP requests in JavaScript. Key points:

- Always check `readyState === 4` and `status === 200`
- Handle errors properly with `onerror` and status code checks
- Parse JSON responses safely
- Set appropriate headers for different request types
- Consider using modern alternatives like Fetch API for new projects

This documentation provides everything you need to work with XMLHttpRequest effectively!
