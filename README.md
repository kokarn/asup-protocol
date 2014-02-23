#ASUP-Specification
===

## General

* All communication to the server must take place over HTTP POST

* All responses from the server must be JSON encoded

* All parameters should be lowercase

* All responses must include a "result" parameter where “success” or “failure” is set, depending on the type of response

    * If the response if "failure", a “message” must be included in the response that tells the client why it failed

* All responses that have data should pass that data back in a variable in the JSON called "data"

* All requests to the server must have a "type" parameter that tells the server what kind of request it is

    * Current paremeters are
        * submitrun
        * categories
        * verifylogin
        * gamelist
        * gamecategories
        * worldrecord

## Authentication

* Both the "username" and the “password” must be submitted on all requests that require authentication so no authentication is stored in the client.

    * The client can store the fields locally if it so chooses, that’s up to the client developer to decide.

## Categories

* The client sends a request to the server with a specific parameter (type=categories)

* The server responds with an array of key/value pairs. The key is what the server want’s in return, the value is what the client should display

    * EG, if we have id’s for categories, the value would be the label (Ocarina of Time Any %) and the key would be the id ( 54 ).

## Games

* See categories with "type=gamelist" instead

## World Records

* The client sends a request to the server with type=worldrecord
    *   required params:
        * category: The category to get the record for     


* The server responds with an array with a name, time and "isAccurate" parameters. 

* isAccurate is set to a truthy value if the server believes the time sent is the current world record, else it's set to a falsy value.


# Submission

* The client sends a request to the server with a set of values that the server then handles however it wants.

    * Required: "runtime", “category”, “username”, “password”, “type”

        * type=submitrun

        * Time format: hh:mm:ss.mmm

    * Optional: "version", “region”, “comment”, “video”

* The server must respond to requests, not just insert/handle them.

## Examples

Verify Login:

```
{
    type: verifylogin, 
	username: "user1", 
	password:"12345"
}
```

which returns either

```
{
	result: success 
} 
```

or 

```
{
    result: fail, 
    message: "Invalid login info"
}
```

Submit Time:

```
{
    type:submitrun, 
    username: "user1", 
    password: "12345", 
    game: "smw", 
    category: "96 Exit", 
    version: "SNES NTSC", 
    runtime: "01:29:59",
    comment: "Good run",
    splits: [00:15:23, 00:41:34, 01:13:53, 01:29:59] 
}
```
which returns either 

```
{
	result: success
} 
```
or 

```
{
	result: fail, 
	message: "Error message"	
}
```

Game list: {type:gamelist}
Returns a list with the key as whatever unique key the server uses, and the name as the value

```
{
	result: success, 
	data: {
		mm1: "Megaman 1", 
		smw: "Super Mario World"
		}
	}
```
or

```
{
	result: fail, 
	message: "error message"
}
```

Game category list: {type:gamecategories, game:smw}
Returns a list with the key as whatever unique key the server uses, and the description as the value

```
{
	result: success, 
	data: {
		1: "96 Exit", 
		2: "any%"
	}
}
```
or

```
{
	result: fail, 
	message: "error message"
}
```

## Thoughts

* Should we have the client detect if a site supports it? Maybe just a meta tag on the start page with an url to the submission endpoint?

## Known endpoints

Finished

[http://allspeedruns.com/asup/](http://allspeedruns.com/asup/)

In development

[http://www.pbtracker.net/](http://www.pbtracker.net/)??????

## Testing

[Test page](http://kokarn.com/assptest.php) A simple test page to simulate HTTP POST requests
