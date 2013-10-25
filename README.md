XHR requests not going through for WP8 reproduction steps:

Compile and run this project in VS, connect to: http://weinre.mediapop.co/client/#wp8

Type in:

    var xhr = new XMLHttpRequest();
    xhr.open('GET', 'js/index.js', true);
    xhr.send();
    console.log(xhr.responseText);

It'll error out on `xhr.open` with `TypeError: Object doesn't support property or method 'addEventListener'`.

Run the same code on iOS and it'll show the content of `js/index.js`.

Running this will access the file:

    var xhr = new XMLHttpRequest();
    xhr._url = 'js/index.js';
    xhr.send();
    console.log(xhr.responseText);

The value of `XMLHttpRequest.prototype.open` also does not match the patched version in `cordovalib/XHRHelper.cs`.

The version from the browser:

    > XMLHttpRequest.prototype.open
    function() {
    var result;
    callBeforeHooks(hookSite, this, arguments);
    try {
    result = func.apply(this, arguments);
    } catch (e) {
    callExceptHooks(hookSite, this, arguments, e);
    throw e;
    } finally {
    callAfterHooks(hookSite, this, arguments, result);
    }
    return result;
    }
