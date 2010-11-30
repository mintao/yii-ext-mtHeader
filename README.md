#Introduction

*mtHeader* is a bunch of (static) functions to set http headers. PHP 5.3 is needed to use this helper.

There comes the time, when a man wants to force a browser to do what he wants. So you may want to tell the browser to **cache a specific page**, **open a download dialog** (even for a html file) or **disable the browser cache** eg. for the captcha image.

*mtHeader* is simple. Just type mtHeader::XXX() where XXX is a placeholder for a file extension like *jpg*, *html*, *exe* or *js*.
Then just send/render the file.

#Scopes

As in the yii models - where there are named scopes - in mtHeader are also scopes (pre-defined header sets).

**Inculded scopes**

- nocache
- length    [size in bytes]
- download  [file name and optional file size in bytes]
- public
- private

Feel free to attach some more (and then make a pull request, so I can merge them)

#Usage

Put *mtHeader* into your extensions folder. To be able to use it, add the include path in your yii config:

    'import' => array(
        ...
        'ext.mtHeader.*',
        ...
    ),

That's all.

#Examples
**Setting a plain text header**

    mtHeader::txt();
    readfile('myPoetry.txt');

This tells the browser to show the upcoming content as text.

---

**Setting this png to private**

    mtHeader::html('private');
    readfile('confidential.html');

This tells the browser that the upcoming html page is private.

---

**Telling the browser, to cache a feed for 1 hour**

    ...
    $atomFeed = createFeed($items);
    mtHeader::atom(array('expires' => 3600));
    echo $atomFeed;

This time the outputted atom feed needs to be cached 3600 seconds by the browser. So within the next 3600 seconds this feed should be loaded from the "browsers" (client's) cache.

---

**Telling the browser not to cache**

    ...
    $captcha = createCaptchaPng('kaS9hI');
    mtHeader::png('nocache');
    imagepng($captcha);

This tells the browser not to cache the following png. This is quite useful for captcha images - so they aren't cached.

---

**Setting individual headers**

    mtHeader::png(array('GreetingsTo' => 'my mom'));
    readfile('myMom.png');

This header is absolutely nonsense - but possible ;) So the browser receives some greetings to my mom, every time this image is displayed.

---

**Force download with given file name "setup.exe" 1kb size**

    $file = '/path/to/my/file-1';
    $fileSize = filesize($file);
    mtHeader::exe(array('download' => array('setup.exe', $fileSize)));
    readfile($file);

This initializes a download. Even if the original file name in this example is "file-1" the browser saves the upcoming file as "setup.exe". Also tell the browser the file size, so the calculations in the user's download dialog will work.

#Where to get a fresh copy of your mime.types file?

**Here you can find a fresh mime.types file**
[http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types](http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types "Apache mime.types")

#Attention

Mime types are cached to speed up whole process. So if you made any changes or replaced the file with a new copy, don't forget to clean the yii cache!
