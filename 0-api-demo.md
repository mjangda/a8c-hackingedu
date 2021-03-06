# APIs by Automattic

---

## What is Automattic?

A bunch of cool cats making the web a better place.

<img src=images/cool-cats.jpg />

---

<img src=images/a8c.gif />

Note:
~300 of us in Park City in 2014
~400 of us now

---

<img src=images/a8c-services.png />

---

## WordPress.com

Hosted blogging / CMS platform

<img src=images/wordpresscom.png />

Note:
Hosted blogging and site-building platform; built on WordPress with added BBQ sauce 

100m+ users, 110m+ sites, 800K+ posts everyday
Jetpack let's self-hosted WordPress sites plug in to WP.com

---

## API

- CRUD, site management, and stats for a WordPress.com site (Posts, Taxonomies, Media)
- Social actions for a user (Likes, Following Blogs, etc.)
- Daily hand-picked content from Freshly Pressed

---

### Auth

OAuth2; required for most endpoints (some are open)

https://developer.wordpress.com/docs/oauth2/

---

## Console

https://developer.wordpress.com/docs/api/console/

---

### Lib: wpcom.js

http://wpcomjs.com/


```js
// create a client WPCOM instance
var wpcom = WPCOM();
// retrieve a listing of the most recent
// 5 posts on "en.blog.wordpress.com"
wpcom
	.site('en.blog.wordpress.com')
	.postsList({ number: 5})
	.then( data => {
		console.log('the newest 5 blog post titles are:\n');
		data.posts.forEach( (post, i) => {
			console.log( '  %d: "%s"', i + 1, post.title ) );
		} );
	} )
	.catch( err => { throw err; } );
```

---

## Example Apps

---

## WordPress.com

<img src=images/calypso.png />

---

## Postbot

<img src=images/postbot.png />

---

## P2

<img src=images/p2.png />

Note:
Uses lower-level APIs, not REST

---

## Sensei

<img src=images/sensei.png />

Note:
Uses lower-level APIs, not REST

---

## Resources

- Developer Site: http://developer.wordpress.com/
- Register new app: http://developer.wordpress.com/apps/
- Console: https://developer.wordpress.com/docs/api/console/

---

## Cloudup

Instant, secure sharing

<img src=images/cloudup.png />

Note:
Cloudup lets you instantly and securely share anything -- videos, photos, music, links, and docs -- without waiting for uploads or downloads.

---

## API

Basically do anything a user can on cloudup.com:

- create/manage streams
- upload files
- get embeds

---

### Auth

- Basic Auth
- OAuth2

---

### Create a stream

```
POST /streams -d 'title=Ferrets'
```

Note:
A stream is basically a collection of objects

---

### Create Item From URL

```
POST /items/url -d 'url=cloudup.com&stream_id=cc3pGD6avN7'
```

Scrapes and formats content from any URL

---

<img src=images/cloudup-url.png />

---

### Create Item From File (Part I)

```
POST /items -d 'filename=tobi.jpg&title=Tobi&stream_id=cc3pGD6avN7'
```

Returns S3 data for uploading.

Note:
Creates a file object 

---

### Create Item From File (Part II)

Upload to S3 using returned data from previous call

```
let form = new FormData();
form.append( 'key', upload.s3_key );
form.append( 'AWSAccessKeyId', upload.s3_access_key );
<snip>
form.append( 'Content-Type', file.type );
form.append( 'Content-Length', file.size );
form.append( 'file', file );

let xhr = new XMLHttpRequest();
xhr.open( 'POST', upload.s3_url, true );
xhr.send( form );
```

---

### Create Item From File (Part III)

Set file as complete

```
PATCH /item/io5mDaTEytP -d '{ "complete": true }'  
```

Note:
Can also send periodic updates via PATCH

Once completed, you now have access to the file plus any generated thumbnails.

---

## Example Apps

---

### Mesh

<img src=images/mesh.png />

---

### Demo

---

## Resources

- Docs: https://dev.cloudup.com
- Node API Client: https://github.com/Automattic/cloudup-client

---

## Simperium

- Like a DropBox for JSON
- Dead-simple for developers
- Failure-first philosophy
- Hosted, separate, offline, traversible data store

---

### Create an App on Simperium.com

![](images/Simplenote - Create an App.png)

---

### Create an App on Simperium.com

![](images/Simplenote - App Dashboard.png)

---

### Use one of many bindings

- iOS / OSX
- Android
- JavaScript
- Python
- Ruby
- Plain HTTP

---

### Use one of many bindings

```
var simperium = new Simperium(app_id, { token: token });

var bucket = simperium.bucket('blammo');

bucket.on('ready', function() {
    console.log("bucket connected and ready!");
});

bucket.on('local', function(id) {
    if (id === 'whammo') {
        return {
            'stuff' : $('#content').val()
        };
    }
});

bucket.on('notify', function(id, data) {
    if (id == 'whammo') {
        $('#content').val(data['stuff']);
    }
});

$('document').ready(function() {
    $('#content').on('change keyup paste', function() {
        bucket.update('whammo');
    });
    bucket.start();
});
```

---

### Build cool things

<video height="480" width="640" controls>
    <source src="https://08-lvl3-pdl.vimeocdn.com/01/3364/1/41820958/97318986.mp4?expires=1445660929&token=0c336700ed6035fea23d8" type="video/mp4">
</video>

---

### HackingEDU Ideas

- LMS
- Real-time quiz platform
- Collaboration
- Group Discussion Boards
- Presence
