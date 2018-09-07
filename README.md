```javascript
const appEl = document.querySelector('#app');
const progressEl = document.querySelector('#progress');

// https://stackoverflow.com/questions/14218607/javascript-loading-progress-of-an-image
Image.prototype.load = function( url, callback ) {
  var thisImg = this,
      xmlHTTP = new XMLHttpRequest();

  thisImg.completedPercentage = 0;

  xmlHTTP.open( 'GET', url , true );
  xmlHTTP.responseType = 'arraybuffer';

  xmlHTTP.onload = function( e ) {
    const h = xmlHTTP.getAllResponseHeaders();
    const m = h.match( /^Content-Type\:\s*(.*?)$/mi );
    const mimeType = m[ 1 ] || 'image/png';

    console.log(h);


    // Construct the img from arraybu{}
    const blob = new Blob( [ this.response ], { type: mimeType } );
    thisImg.src = window.URL.createObjectURL( blob );

    if ( callback ) {
      callback(this);
    }
  };

  xmlHTTP.onprogress = function( e ) {
      if ( e.lengthComputable ) {
       thisImg.completedPercentage = parseInt( ( e.loaded / e.total ) * 100 );
       console.log(thisImg.completedPercentage);
      }
  };

  xmlHTTP.onloadstart = function() {
      // Display your progress bar here, starting at 0
      thisImg.completedPercentage = 0;
  };

  xmlHTTP.onloadend = function() {
      // You can also remove your progress bar here, if you like.
      thisImg.completedPercentage = 100;
  }

  xmlHTTP.send();
};

const img = new Image();
img.load('https://storage.googleapis.com/tylermadison-info/assets/img/sf.jpg', () => {
  appEl.appendChild(img);
});

console.log(img);
```
