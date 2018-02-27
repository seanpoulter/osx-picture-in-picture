# Picture in Picture Support for Any `<video>` Tag

* Safari supports Picture-in-Picture video
* Here's [Apple's release notes](https://developer.apple.com/library/content/releasenotes/General/WhatsNewInSafari/Articles/Safari_9_0.html#//apple_ref/doc/uid/TP40014305-CH9-SW18) for Picture in Picture Support
* If the UI doesn't support it, you can enable Picture-in-Picture mode using a bookmarklet
* VanillaJS to support all the things

```js
javascript:(function bookmarklet(target) {
  pictureInPicture(target);

  function pictureInPicture(baseElement) {
    try {
      trySetPictureInPicture(baseElement);
    } catch (err) {
      alert('ERROR: ' + err.message);
    }
  }

  function trySetPictureInPicture(baseElement) {
    const video = tryFindVideo(baseElement);
    assertFunctionAvailable(video);
    video.webkitSetPresentationMode('picture-in-picture');
  }

  function tryFindVideo(baseElement) {
    const videos = findVideoTags(baseElement);
    assertOneVideoFound(videos);
    return videos[0];
  }

  function findVideoTags(baseElement) {
    return baseElement.getElementsByTagName('video');
  }

  function assertOneVideoFound(nodeList) {
    if (nodeList && nodeList.length === 1) {
      return;
    }
    throw new Error('Expected one <video> tag but found ' + nodeList.length + ' in document.');
  }

  function assertFunctionAvailable(element) {
    if (typeof element.webkitSetPresentationMode !== 'function') {
      throw new Error('Expected <video> to support the "webkitSetPresentationMode" method');
    }
  }
})(document);
```
