Lifestreams Node SDK
=========

Lifestreams Node SDK provides a set of methods to interact with Lifestreams API in a NodeJS application.

Prerequisites
-------------
In order to be able to use the SDK and the API, access key and api id need to be generated in the API Management Tool [LINK]

Installation
-----------

```sh
npm install ls-node-sdk
```

Initialization
-------------

```javascript
var LS = require('ls-node-sdk');
LS.init({
    api_key: "YOUR_API_KEY",
    api_id: "YOUR_API_ID"
});
```

Initialization method accepts the following optional parameters:

| param name | description | type
| --- | --- | --- |
| api_user | A user id, which, if provided informs the backend to consider the given user as logged in. | String |
| api_url | API endpoint URL, by default pointing to the latest production instance | String |
| socket_url | Socket endpoint URL, by default pointing to the latest production instance | String |

Usage
-----
## LS.api()
Make an api call to an existing enpoint and handle a reponse in a callback.
For available endpoints and their parameters please refer to Lifestreams Streaming API documentation.

### Parameters
name | description | type | required
--- | --- | --- | ---
url | An API endpoint name | String | true
method | Http request method | ENUM: GET, POST, PATCH, DELETE | true
data | Data object to pass to an api call | Object | false
callback | A JavaScript callback method to handle the response | Function | true
### Examples
Retrieve a list of timelines

```javascript
LS.api("/timeline", "get", function(err, response) {
    // do something with a response
    response.forEach(function(item) {
        console.log("Timeline name: " + item.name);
    }):
});
```

Create a new timeline

```javascript
LS.api("/timeline", "post", {
    "name": "My random Timeline name"
}, function(err, response) {
    if (response.error) {
        // handle error
        console.log(response.error + ": " + response.message);
    } else {
        // handle successful response
        console.log(response.name + " created successfuly!"
    }
});
```
## LS.getUser()
Retrieve details about a user.

### Parameters
name | description | type | required
--- | --- | --- | ---
userId | A user ID string | String | true
callback | A callback to handle errors and response data | Function | true

Example:

```javascript
LS.getUser("527c14962e6dd7c17e000015", function(err, data) {
    if (!err) {
        console.log(data);
    } else {
        throw err;
    }

});
```

## LS.getUsers()
List the users created under the scope of the authenticated application.

### Parameters
name | description | type | required
--- | --- | --- | ---
callback | A callback to handle errors and response data | Function | true

## LS.createUser()
Create an application user.

### Parameters
name | description | type |required
--- | --- | --- | ---
data | A data object with user information, currently supported properties: username, description, email, userAvatar | Object | true
callback | A callback to handle errors and response data | Function | true

Example:

```javascript
LS.createUser(data, function(err, response) {
    if (!err) {
        console.log("User created: " + reponse._id);
    } else {
        // handle errors
    }
});
```

## LS.getMemberships()
Retrieve a list of memberships for a given user.

### Parameters
name | description | type | required
--- | --- | --- | ---
userId | A user ID string | String | true
callback | A callback to handle errors and response data | Function | true

## LS.getMembership()
Retrieve a user membership information in a given timeline scope.

### Parameters
name | description | type | required
--- | --- | --- | ---
userId | A user ID string | String | true
timelineId | A timeline ID string | String | true
callback | A callback to handle errors and response data | Function | true

## LS.updateMembership()
Modify a membership information for a given user and timeline.

### Parameters
name | description | type | required
--- | --- | --- | ---
userId | A user ID string | String | true
timelineId | A timeline ID string | String | true
data | An object containing data to modify | Object | true
callback | A callback to handle errors and response data | Function | true

## LS.getTimelines()
Retrieve a list of timelines available for authenticated user.

### Parameters
name | description | type | required
--- | --- | --- | ---
callback | A callback to handle errors and response data | Function | true

## LS.getTimeline()
Retrieve information about a timeline.

### Parameters
name | description | type | required
--- | --- | --- | ---
timelineID | A timeline ID string | String | true
callback | A callback to handle errors and response data | Function | true

## LS.createTimeline()
Create a new timeline.

### Parameters
name | description | type | required
--- | --- | --- | ---
data | Create a new timeline. The request body should contain the following properties: *name*: A name or title (not necessarily unique) for the timeline (required); *description*: (optional) A description of the timeline | Object | true
callback | A callback to handle errors and response data | Function | true

## LS.getCards()
Obtain cards from a given timeline.
### Parameters
name | description | type | required | default
--- | --- | --- | --- | ---
timelineID | A timeline ID string | String | true | -
ts | Timestamp to use as a reference starting point within the timeline. By default, this takes the value of the current timestamp. | Number | false | now()
direction | Direction to take from the provided starting timestamp. This parameter controls whether to fetch cards from the past, from the future or around the given timestamp. | ENUM: around, before, after | false | around
limit | Maximum amount of cards to return as a result of the streaming call. | Number | false | 20
query | Query string used to filter through the timeline. This allows for textual search and other filtering. | String | false | -
media_urls | Whether the response should contain publicly accessible URLs to media attached in the cards. Also note the parameter urls_ttl | Boolean | false | false
preview_urls | Whether the response should contain publicly accessible URLs to media previews in the cards. Also note the parameter urls_ttl | Boolean | false | false
thumb_urls | Whether the response should contain publicly accessible URLs to media thumbnails in the cards. Also note the parameter urls_ttl | Boolean | false | false
urls_ttl | Only has effect when media_urls or preview_urls are enabled. This parameter allows to specify for how many seconds the publicly accessible URLs to attached media should remain valid. The TTL counter is independent for each response to a streaming request and starts counting down as soon as the response is produced. The maximum allowable value for this parameter is 86400 (equivalent to 24 hours) | Number | false | 300 |
callback | A callback to handle errors and response data | Function | true | -

## LS.getCard()
Retrieve contents of a card.

### Parameters
name | description | type | required
--- | --- | --- | ---
timelineID | A timeline ID string | String | true
cardID | A card ID string | String | true
callback | A callback to handle errors and response data | Function | true

## LS.createCard()
Add card to a timeline.

### Parameters
name | description | type | required
--- | --- | --- | ---
timelineID | A timeline ID string | String | true
data | A data object containing information about a card. | Object | true
callback | A callback to handle errors and response data | Function | true

## LS.updateCard()
Modify contents of a card.

### Parameters
name | description | type | required
--- | --- | --- | ---
timelineID | A timeline ID string | String | true
cardId | A cardID string | String | true
data | A data object containing modified information about a card. | Object | true
callback | A callback to handle errors and response data | Function | true

## LS.getComments()
Retrieve comments for a given card.

### Parameters
name | description | type | required
--- | --- | --- | ---
timelineID | A timeline ID string | String | true
cardID | A card ID string | String | true
callback | A callback to handle errors and response data | Function | true

## LS.createComment()
Create a comment and attach it to a given card.
### Parameters
name | description | type | required
--- | --- | --- | ---
timelineID | A timeline ID string | String | true
cardID | A card ID string | String | true
data | A data object containing modified information about a card. | Object | true
callback | A callback to handle errors and response data | Function | true

Testing
-------

```sh
npm run-script install-dev # depending on your config may ask for sudo
npm test
```