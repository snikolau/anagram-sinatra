# Anagram web service

Sinatra based web service application which returns anagrams for requested words.
By default, it uses dictionary file from https://github.com/dwyl/english-words repository.

## Local installation
1. Clone this repository
2. Within cloned repository location run:
```bash
   bundle install
```
3. Then run webserver by:
```bash
   rackup
```

## Usage
Application works as simple restful, just call 
```
GET localhost:9292/<list-of-words>
```
This should return anagram matches for a given list of words.

Example:
```bash
curl localhost:9292/blog,post 
```
should return:
```bash
{"blog":["glob"],"post":["opts","stop"]}
```

## Further improvements

This is a basic implementation which has some downsides:
 - Dictionary configuration is defined directly within Sinatra application.
 - Dictionary file is loaded in memory and is transformed every time server is starting, which significantly increases
 starting time. The problem is visible especially when hosted on Heroku free tire, where the application is switching to
 idle mode every 30 minutes.
 - It is not possible to extend dictionary, without editing text file and restarting the server.
 
Possible solutions
 - Create separate configuration module / class responsible for storing / initialisation of system wide available data.
 - One of the solutions would be creating separate rake / bash task to process text file and store intermediate
 preprocessed data in a separate file. This would reduce app start time, as it would only load preprocessed data without 
 processing step, but would not solve a problem with extensibility.
 - More complex but scalable solution might be using in-memory data store as Redis.   
