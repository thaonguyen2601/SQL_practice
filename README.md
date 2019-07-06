# Project with SQL (MySQL, SQL Server) & Excel for Data Analysis & Visualization


These are exercises to practice and enhance my self-learn SQL skills and advanced Excel.

# Exercises 1:
  Working on Instagram's clone database (100 users), with 7 relational tables, using MySQL to answer business requirements
  
  Table schema is as follows: 
  
  (insert picture here?)
  
  
  1/ Find out Top 5 users that have most followees:
  
    SELECT users.username, follows.follower_id, COUNT(*) as total
    FROM follows
    INNER JOIN users ON users.id = follows.follower_id
    GROUP BY follows.follower_id
    ORDER BY total DESC LIMIT 5;
    
  2/ Find out Top 5 period hours that have most interation:
  
    SELECT HOUR(created_at) AS hour, COUNT(*) AS total 
    FROM likes 
    GROUP BY hour 
    ORDER BY total DESC LIMIT 5;
    
  3/ Find out Top 10 photos with specific hashtags, the users who posted those photos and the following likes (for ex: to identify the winners of a marketing contest)
  In this case we use 2 hashtags: fun (id: 13), party (id: 17) 

    SELECT users.username, photos.id, photos.image_url,
           COUNT(likes.created_at) AS total_likes, photo_tags.tag_id
    FROM photos
    JOIN users ON users.id = photos.user_id 
    JOIN likes ON photos.id = likes.photo_id
    JOIN photo_tags ON photo_tags.photo_id = photos.id
    WHERE photo_tags.tag_id IN (13,17)
    GROUP BY photos.id 
    ORDER BY total_likes DESC LIMIT 10;
    
  4/ Count the percentage of users who have never commented or commented on every photo
  (for ex: to cluster our customers and see the portion of each group)

- Percentage of users that have never commented:

        SELECT ( SELECT ( (
                SELECT COUNT(*) FROM users 
                WHERE users.id NOT IN 
                    ( SELECT comments.user_id FROM comments	
                      GROUP BY comments.user_id ) )
                    ) / 
                ( SELECT COUNT(*) FROM users) ) * 100 AS percent_no_cmt ; 
 
 - Percentage of users that have commented on every photo: 
 
        SELECT COUNT(*) FROM comments ;
    --> to see how many comments there are in our database, then use that number to calculate the user (in this case: [14976][df1])

        SELECT ( SELECT ( (	
                SELECT COUNT(*) FROM users 
                WHERE users.id IN 
                 ( SELECT user_id FROM comments HAVING COUNT(id) = 14976 ) )  ) )    / 
        ( SELECT COUNT(*) FROM users) ) * 100 AS percent_cmt_all ; 



> The overriding design goal for Markdown's
> formatting syntax is to make it as readable
> as possible. The idea is that a
> Markdown-formatted document should be
> publishable as-is, as plain text, without
> looking like it's been marked up with tags
> or formatting instructions.

This text you see here is *actually* written in Markdown! To get a feel for Markdown's syntax, type some text into the left window and watch the results in the right.

### Tech

Dillinger uses a number of open source projects to work properly:

* [AngularJS] - HTML enhanced for web apps!
* [Ace Editor] - awesome web-based text editor
* [markdown-it] - Markdown parser done right. Fast and easy to extend.
* [Twitter Bootstrap] - great UI boilerplate for modern web apps
* [node.js] - evented I/O for the backend
* [Express] - fast node.js network app framework [@tjholowaychuk]
* [Gulp] - the streaming build system
* [Breakdance](http://breakdance.io) - HTML to Markdown converter
* [jQuery] - duh

And of course Dillinger itself is open source with a [public repository][dill]
 on GitHub.

### Installation

Dillinger requires [Node.js](https://nodejs.org/) v4+ to run.

Install the dependencies and devDependencies and start the server.

```sh
$ SELECT dillinger
$ npm install -d
$ node app
```

For production environments...

```sh
$ npm install --production
$ NODE_ENV=production node app
```

### Plugins

Dillinger is currently extended with the following plugins. Instructions on how to use them in your own application are linked below.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| Github | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |


### Development

Want to contribute? Great!

Dillinger uses Gulp + Webpack for fast developing.
Make a change in your file and instantanously see your updates!

Open your favorite Terminal and run these commands.

First Tab:
```sh
$ node app
```

Second Tab:
```sh
$ gulp watch
```

(optional) Third:
```sh
$ karma test
```
#### Building for source
For production release:
```sh
$ gulp build --prod
```
Generating pre-built zip archives for distribution:
```sh
$ gulp build dist --prod
```
### Docker
Dillinger is very easy to install and deploy in a Docker container.

By default, the Docker will expose port 8080, so change this within the Dockerfile if necessary. When ready, simply use the Dockerfile to build the image.

```sh
cd dillinger
docker build -t joemccann/dillinger:${package.json.version} .
```
This will create the dillinger image and pull in the necessary dependencies. Be sure to swap out `${package.json.version}` with the actual version of Dillinger.

Once done, run the Docker image and map the port to whatever you wish on your host. In this example, we simply map port 8000 of the host to port 8080 of the Docker (or whatever port was exposed in the Dockerfile):

```sh
docker run -d -p 8000:8080 --restart="always" <youruser>/dillinger:${package.json.version}
```

Verify the deployment by navigating to your server address in your preferred browser.

```sh
127.0.0.1:8000
```

#### Kubernetes + Google Cloud

See [KUBERNETES.md](https://github.com/joemccann/dillinger/blob/master/KUBERNETES.md)


### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT


**Free Software, Hell Yeah!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>


