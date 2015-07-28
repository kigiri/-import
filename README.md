# -import
draft for simple import lib for browser console
```
var $import = (function () {
  function getJSON(url, cb) {
    var xhr = new XMLHttpRequest();

    xhr.open('get', url, true);
    xhr.responseType = 'json';
    xhr.onload = function() {
      if (xhr.status === 200) {
        cb(xhr.response);
      }
    };
    xhr.send();
  }

  function injectPackage(src) {
    console.log("injecting source from", src);
    var s = document.createElement('script');
    s.src = src;
    document.getElementsByTagName('head')[0].appendChild(s);
  }

  function loadPackage(url) {
    url = url.replace("github", "cdn.rawgit") +"/master/";
    console.log(url);
    getJSON(url +"bower.json", function (packageData) {
      injectPackage(url + packageData.main);
    });
  }

  return function (packageName) {
    getJSON("https://bower-component-list.herokuapp.com", function (bowerData) {
      function importFn(packageName) {
        var i = -1;

        while (++i < bowerData.length) {
          if (bowerData[i].name === packageName) {
            console.log("loading package data...");
            return loadPackage(bowerData[i].website);
          }
        }
        console.log("package", packageName, "not found");
      }
      importFn(packageName);
      $import = importFn;
    });
    return "downloading component list...";
  };
})();
```
# Usage :
```
$import('lodash');
```
