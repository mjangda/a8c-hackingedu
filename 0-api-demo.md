# Automattic APIs

---

## What is Automattic?

A bunch of cool cats democratizing publishing.

TODO: team photo or screenshot
TODO: list of services

---

## WordPress.com

Hosted blogging / CMS platform

TODO screenshot

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

### Get a User

```
GET /me
```

TODO

---

### Get a Site 

TODO

---

### Create a Post

TODO

---

### Upload Media

TODO

---

### Lib: wpcom.js

---

### Console

TODO: screenshot 

---

## Example Apps

---

## WordPress.com

TODO: screenshot of Calypso

---

## Postbot

TODO: screenshot of Postbot

---

## P2

Note:
Not technically built on the API, but something like this is possible.

---

## Resources

- Developer Site: http://developer.wordpress.com/
- Register new app: http://developer.wordpress.com/apps/
- Console: https://developer.wordpress.com/docs/api/console/

---

## Cloudup

Instant, secure sharing

TODO: screenshot

Note:
Cloudup lets you instantly and securely share anything -- videos, photos, music, links, and docs -- without waiting for uploads or downloads.

---

## API

Basically do anything a user can on cloudup.com: create/manage streams, upload files, get embeds

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

TODO what does this do?

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

## cloudup.com

TODO screenshot

---

## Mesh

TODO screenshot

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

![](https://cldup.com/zXr_CAgLDB.png)

---

### Create an App on Simperium.com

![](https://cldup.com/xS9Lx_xXrR.png)

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

<video src="https://player.vimeo.com/video/41820958"></video>

---

### HackingEDU Ideas

- Real-time quiz platform
- Collaboration
- Group Discussion Boards
- Presence
