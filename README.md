#mintao mtHeader
*mtHeader* is a bunch of static functions to set http headers. You may want to tell the browser to cache a specific page, open an download dialog for a certain file or disable the browser cache eg. for the captcha file.

*mtHeader* is based on common mime types. The parsed *mime.types* file is in standard unix format. So if missing a mime type, simply overwrite the delivered *mime.types* file - no additional modifications needed.

**Mime types are cached. So don't forget to clean the cache after providing a new mime.types file!**

##Here you can find a fresh version
[http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types](http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types "Apache mime.types")

#Scopes
Scopes are pre-defined header sets.

**Inculded scopes**
- nocache
- length    [size in bytes]
- download  [file name and optional file size in bytes]
- public
- private

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


**Setting this png to private**

    mtHeader::html('private');
    readfile('confidential.html');

This tells the browser that the upcoming html page is private.


**Telling the browser, to cache a feed for 1 hour**

    ...
    $atomFeed = createFeed($items);
    mtHeader::atom(array('expires' => 3600));
    echo $atomFeed;

This time the outputted atom feed needs to be cached 3600 seconds by the browser. So within the next 3600 seconds this feed should be loaded from the "browsers" (client's) cache.


**Telling the browser not to cache**

    ...
    $captcha = createCaptchaPng('kaS9hI');
    mtHeader::png('nocache');
    imagepng($captcha);

This tells the browser not to cache the following png. This is quite useful for captcha images - so they aren't cached.


**Setting individual headers**

    mtHeader::png(array('GreetingsTo' => 'my mom'));
    readfile('myMom.png');

This header is absolutely nonsense - but possible ;) So the browser receives some greetings to my mom, every time this image is displayed.


**Force download with given file name "setup.exe" 1kb size**

    $file = '/path/to/my/file-1';
    $fileSize = filesize($file);
    mtHeader::exe(array('download' => array('setup.exe', $fileSize)));
    readfile($file);

This initializes a download. Even if the original file name in this example is "file-1" the browser saves the upcoming file as "setup.exe". Also tell the browser the file size, so the calculations in the user's download dialog will work.