[En français](readme_fr.md)

![ECCC logo](../img_eccc-logo.png)

[TOC](../readme_en.md) > MSC GeoMet

## GeoMet and Javascript web-mapping libraries
GeoMet-Weather and GeoMet-Climate serve OGC Open Web Services (OWS)
that are easily integrated into web mapping libraries such as OpenLayers
and Leaflet. This section will show you how to work with a Web Map Service 
(WMS) using both libraries. By the end of it, you will be able to display any
GeoMet layer on an interactive map, query the WMS for data, and even animate
a time-enabled GeoMet layer.

- [Displaying a GeoMet-Weather WMS layer](#displaying-a-geomet-weather-wms-layer)
    * [OpenLayer example](#openlayers-example)
    * [Leaflet example](#leaflet-example)
- [Building interactive popups with OpenLayers and GeoMet](#building-interactive-popups-with-openlayers-and-geomet)
- [Animating time-enabled GeoMet data with OpenLayers](#animating-time-enabled-geomet-data-with-openlayers)

## Displaying a GeoMet-Weather WMS layer

The following steps will show how to create a simple web map with
OpenLayers and Leaflet. The map will display the Global Deterministic
Prediction System's air surface temperature data (`GPDS.ETA_TT`) over an
OpenStreetMap basemap. An example is given for the both OpenLayers and Leaflet.

### OpenLayers example

#### HTML

```html
<html lang="en">
<head>
  <meta charset="utf-8">
  <style>
    #map {
      width: 100%;
      height: 400px;
    }
  </style>

  <title>GeoMet OpenLayers Example</title>
  <meta name="description" content="GeoMet OpenLayers Example">
  <meta name="author" content="CCMEP">

  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.3.0/css/ol.css" type="text/css">
  <script src="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.3.0/build/ol.js"></script>
</head>

<body>
  <div id="map">
  </div>
</body>
</html>
```

There are a two important things to note. 

First, in order to use OpenLayers, we must import the required JS and CSS
libraries. A  `<div>` element is added in the body of our HTML document
and assigned an `id` attribute with a value of `map`.
The `id` value will be referred to in the Javascript code in order to
specify where to render the interactive map in the HTML document.

A small amount of CSS is also added in the `<head>` tag in order to define the
width and height of the map container.

#### Javascript
With the HTML code now complete, let's turn our attention to writing the
JavaScript code required to create our web map.

```javascript
let layers_to_add = [
    new ol.layer.Tile({
      source: new ol.source.OSM()
    }),
    new ol.layer.Tile({
      opacity: 0.4,
      source: new ol.source.TileWMS({
        url: 'https://geo.weather.gc.ca/geomet/',
        params: {'LAYERS': 'GDPS.ETA_TT', 'TILED': true},
        transition: 0
      })
    })
  ];

let map = new ol.Map({
  target: 'map',
  layers: layers,
  view: new ol.View({
    center: ol.proj.fromLonLat([-97, 57]),
    zoom: 0
  })
});

```

To keep things a little tidy, we create an `layers_to_add` array
that contains a list of layers we want to add to our map. Each item is a layer 
that is linked to a source. In this case two layers are added to the map:
(1) the OpenStreetMap layer for our basemap and (2) the GeoMet-Weather WMS
for the Global Deterministic Prediction System's air surface temperature layer.
The GDPS air surface temperature source also has some additional properties:

 * `url`: WMS service URL.
 * `params`: WMS request parameters. The `LAYERS` parameter is required.
 * `transition`: Duration of the opacity transition for rendering. We disable
 this here since we are setting an opacity and want it to be applied
 prior to displaying each tile.

Second, we create a new variable called `map` and use the `ol.Map` constructor
to define the map that will be rendered in our HTML document. In the
object passed to the constructor, define a `target` for our map (e.g
the `id` value of the HTML container, in this case `map`). The
`layers_to_add` array is then passed to the `layers` property and the `ol.View`
constructor is used to define the initial view of our map. In our case, we set
the  initial zoom level and center the view on a specific lon/lat coordinate.

That's it!

Below is a a live example of the above code. Try modifying
the Javascript code to display another layer, changing the opacity, and the 
initial zoom and centre coordinates of the map.

<iframe height="275" style="width: 100%;" scrolling="no" title="GeoMet Simple WMS  OpenLayers" src="https://codepen.io/dukestep/embed/ZEGgBLN?height=275&theme-id=light&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/dukestep/pen/ZEGgBLN'>GeoMet Simple WMS  OpenLayers</a> by Dukestep
  (<a href='https://codepen.io/dukestep'>@dukestep</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

### Leaflet example

#### HTML
```html
<html lang="en">

<head>
  <meta charset="utf-8">
  <style>
    #map {
      width: 100%;
      height: 400px;
    }
  </style>

  <title>GeoMet Simple WMS Leaflet Example</title>
  <meta name="description" content="GeoMet OpenLayers Example">
  <meta name="author" content="CCMEP">

  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.6.0/dist/leaflet.css" integrity="sha512-xwE/Az9zrjBIphAcBb3F6JVqxf46+CDLwfLMHloNu6KEQCAWi6HcDUbeOfBIptF7tcCzusKFjFw2yuvEpDL9wQ==" crossorigin="" />
  <script src="https://unpkg.com/leaflet@1.6.0/dist/leaflet.js" integrity="sha512-gZwIG9x3wUXg2hdXF6+rVkLF/0Vi9U8D2Ntg4Ga5I5BZpVkVxlJWbSQtXPSiUTtC0TjtGOmxa1AJPuV0CPthew==" crossorigin=""></script>
</head>

<body>
  <div id="map">
  </div>
</body>

</html>
```

First, in order to use Leaflet, we must import the required JS and CSS
libraries. A  `<div>` element is added in the body of our HTML document
and assigned an `id` attribute with a value of `map`.
The `id` value will be referred to in the Javascript code in order to
specify where to render the interactive map in the HTML document.

A small amount of CSS is also added in the `<head>` tag in order to define the
width and height of the map container.

#### Javascript
With the HTML code now complete, let's turn our attention to writing the
JavaScript code required to create our web map.

```javascript
let map = L.map("map").setView([0,0], 3);

let OpenStreetMap_Mapnik = L.tileLayer(
  "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
  {
    maxZoom: 19,
    attribution:
      '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
  }
).addTo(map);

let wmsLayer = L.tileLayer.wms('https://geo.weather.gc.ca/geomet?', {
    layers: 'GDPS.ETA_TT'
    version: '1.3.0',
    opacity: 0.5,
}).addTo(map);
```

The above code instantiates a [map object](https://leafletjs.com/reference-1.6.0.html#map)
using the Leaflet API and sets the initial map view with the
[<code>setView</code>](https://leafletjs.com/reference-1.6.0.html#map-setview)
method.

Following the instantiation of the map, two layers are defined and added
to the map instance.  For each layer, a base URL is passed as well as
parameters/options object used to define in further detail. For example,
when instantiating the `wmsLayer` variable we define the opacity of the
`GDPS.ETA_TT` layer and the WMS version to used when a request is made within
 the parameters object.

See the live example below:

<iframe height="265" style="width: 100%;" scrolling="no" title="GeoMet Simple WMS Leaflet" src="https://codepen.io/dukestep/embed/wvKwYLX?height=265&theme-id=light&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/dukestep/pen/wvKwYLX'>GeoMet Simple WMS Leaflet</a> by Dukestep
  (<a href='https://codepen.io/dukestep'>@dukestep</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

### Building interactive popups with OpenLayers and GeoMet
Now that the GDPS air surface temperature layer is properly displayed on an
interactive map, let's try adding some additional functionality to the map.
Web Map Services (WMS) allow a user to make a [GetFeatureInfo request](./web-services_en.md/#wms-getfeatureinfo)
to extract data associated to a coordinate on the map. Using the OpenLayers
API, we will create a popup when a user clicks on the map that will display
the coordinates of the clicked point and the corresponding air surface
temperature. This implementation is heavily inspired by the
[popup](https://openlayers.org/en/latest/examples/popup.html?q=popup) and
[WMS GetFeatureInfo](https://openlayers.org/en/latest/examples/getfeatureinfo-tile.html?q=popup)
examples provided by OpenLayers.

#### HTML
Some additional HTML and CSS will need to be added to our initial HTML
document.

```html
<html lang="en">
<head>
  <meta charset="utf-8">
  <style>
    #map {
      width: 100%;
      height: 400px;
    }
    /* Additional CSS for popup */
    .ol-popup {
      position: absolute;
      background-color: white;
      box-shadow: 0 1px 4px rgba(0,0,0,0.2);
      padding: 15px;
      border-radius: 10px;
      border: 1px solid #cccccc;
      bottom: 12px;
      left: -50px;
      min-width: 300px;
    }
    .ol-popup:after, .ol-popup:before {
      top: 100%;
      border: solid transparent;
      content: " ";
      height: 0;
      width: 0;
      position: absolute;
      pointer-events: none;
    }
    .ol-popup:after {
      border-top-color: white;
      border-width: 10px;
      left: 48px;
      margin-left: -10px;
    }
    .ol-popup:before {
      border-top-color: #cccccc;
      border-width: 11px;
      left: 48px;
      margin-left: -11px;
    }
    .ol-popup-closer {
      text-decoration: none;
      position: absolute;
      top: 2px;
      right: 8px;
    }
    .ol-popup-closer:after {
      content: "✖";
    }
  </style>

  <title>GeoMet OpenLayers Example</title>
  <meta name="description" content="GeoMet OpenLayers Example">
  <meta name="author" content="CCMEP">

  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.3.0/css/ol.css" type="text/css">
  <script src="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.3.0/build/ol.js"></script>
</head>

<body>
  <div id="map">
  </div>
  <!-- New div containing the HTML code for the popup -->
  <div id="popup" class="ol-popup">
    <a href="#" id="popup-closer" class="ol-popup-closer"></a>
    <div id="popup-content"></div>
  </div>
</body>
</html>
```

To create the popup, a new `div` element is added. Within this new
element, an anchor tag is added to allow for closing the popup when displayed
on the map. An empty `div` will contain the results of the
GetFeatureInfo and the coordinates of the clicked map point. The additional
CSS is used define the look and feel of the popup as well as displaying/hiding
the popup when the user clicks on the map.

#### Javascript
```javascript
/**
 * Elements that make up the popup.
 */
var container = document.getElementById('popup');
var content = document.getElementById('popup-content');
var closer = document.getElementById('popup-closer');


/**
 * Create an overlay to anchor the popup to the map.
 */
var overlay = new ol.Overlay({
  element: container,
  autoPan: true,
  autoPanAnimation: {
    duration: 250
  }
});


/**
 * Add a click handler to hide the popup.
 * @return {boolean} Don't follow the href.
 */
closer.onclick = function() {
  overlay.setPosition(undefined);
  closer.blur();
  return false;
};

let layers = [
    new ol.layer.Tile({
      source: new ol.source.OSM()
    }),
    new ol.layer.Tile({
      opacity: 0.4,
      source: new ol.source.TileWMS({
        // TODO: Change this to dev environment for JSON response
        url: 'https://geo.wxod-dev.cmc.ec.gc.ca/geomet',
        params: {'LAYERS': 'GDPS.ETA_TT', 'TILED': true},
        transition: 0
      })
    })
  ]

let map = new ol.Map({
  target: 'map',
  layers: layers,
  overlays: [overlay],
  view: new ol.View({
    center: ol.proj.fromLonLat([-97, 57]),
    zoom: 0
  })
});


map.on('singleclick', function(evt) {
  var coordinate = evt.coordinate;
  var xy_coordinates = ol.coordinate.toStringXY(ol.proj.toLonLat(coordinate), 4);
  var viewResolution = map.getView().getResolution();
  var wms_source = map.getLayers().item(1).getSource();
  var url = wms_source.getFeatureInfoUrl(
    evt.coordinate, viewResolution, 'EPSG:3857',
    {'INFO_FORMAT': 'application/json'});
  if (url) {
    fetch(url)
      .then(function (response) { return response.json(); })
      .then(function (json) {
      content.innerHTML = `<b>Coordinates (Lon/Lat): </b> <code>${xy_coordinates}</code><br><b> Temperature: </b><code>${Math.round(json.features[0].properties.value)} °C</code>`;
      overlay.setPosition(coordinate);
      });
    }
});
```

There are four main additions to the Javascript code.

First, we create references to the three HTML elements that are used by the 
popup( `container`, `content`, and `closer`). 

The [<code>ol.Overlay()</code>](https://openlayers.org/en/latest/apidoc/module-ol_Overlay-Overlay.html)
constructor is then used to create a new overlay which will be used to anchor
the popup to the map. Note that `container` (i.e. our popup) is associated to
the `element` property of the object passed to the Overlay
constructor. The overlay is then passed as an array item to the
`overlays` property when constructing the map.

An inline `onclick` event is then added to the `closer` (i.e the `<a>` tag in
the popup), that sets the overlay position to `undefined`, effectively
hiding the overlay when the anchor tag contained in the popup is clicked.

Finally, our map will take advantage of the
[<code>ol.Map.on()</code>](https://openlayers.org/en/latest/apidoc/module-ol_Map-Map.html#on)
method to listen for `singleclick` events on the map. The callback function
that is triggered by the event does the following:

* Fetches the coordinates of the clicked map point, then reprojects the
coordinates to EPSG:4326 (WSG 84). The `ol.coordinate.toStringXY` method 
transforms the coordinates into a comma delimited string.
* Retrieves the resolution of the map view.
* Retrieves the source of the `GDPS.ETA_TT` layer.
* Uses the `ol.source.TileWMS.getFeatureInfoUrl()` method to create
a WMS GetFeatureInfo request. Passed as arguments are the click event
coordinates, view resolution, map projection, and an object
containing any GetFeatureInfo parameters (`INFO_FORMAT` should at least be 
provided).
* If the GetFeatureInfo URL is properly constructed, submits the 
GetFeatureInfo request using the Javascript API. Upon receiving
a response, the JSON is retrieved and the popup content is updated with
additional HTML that includes the coordinates and the value property
 of the first GeoJSON feature retrieved by the GetFeatureInfo request.
* Sets the overlay position to the coordinates of the initial
click event.

See the live example below:

<iframe height="275" style="width: 100%;" scrolling="no" title="GeoMet GetFeatureInfo OpenLayers" src="https://codepen.io/dukestep/embed/PoqvpPW?height=275&theme-id=light&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/dukestep/pen/PoqvpPW'>GeoMet GetFeatureInfo OpenLayers</a> by Dukestep
  (<a href='https://codepen.io/dukestep'>@dukestep</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

### Animating time-enabled GeoMet data with OpenLayers
A significant amount of data served by GeoMet has one or more temporal
dimensions. The following section will dive into how OpenLayers can help
us visualize and animate the various time slices of such layers,
in this case weather radar data, served as a WMS via GeoMet-Weather. This
example is inspired by the [WMS Time example](https://openlayers.org/en/latest/examples/wms-time.html)
provided by OpenLayers.

Two GeoMet-Weather layers are used to create this animation: `RADAR_1KM_RRAI`
and `RADAR_COVERAGE_RRAI.INV`. These layers are available over a moving
3-hour window, every 10 minutes.

#### HTML
```html
<html lang="en">
<head>
  <meta charset="utf-8">
  <style>
    #map {
      width: 100%;
      height: 400px;
    }
    #controller {
      background: #ececec;
      padding: 0.5rem;
    }
    #play,#pause {
      padding: 0rem 1rem;
    }
    #info {
      padding-left: 0.5 rem;
    }
  </style>
  <title>GeoMet Animated WMS Time OpenLayers Example</title>
  <meta name="description" content="GeoMet OpenLayers Example">
  <meta name="author" content="CCMEP">

  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.3.0/css/ol.css" type="text/css">
  <link href="https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet" integrity="sha384-wvfXpqpZZVQGK6TAh5PVlGOfQNHSoD2xbE+QkPxCAFlNEevoEH3Sl0sibVcOQVnN" crossorigin="anonymous">
  <script src="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.3.0/build/ol.js"></script>
</head>

<body>
  <div id="map">
  </div>
  <div id="controller" role="group" aria-label="Animation controls" style="background: #ececec; padding: 0.5rem;">
    <button id="play" class="btn btn-primary btn-sm" type="button">
            <i class="fa fa-play" style="padding: 0rem 1rem"></i>
    </button>
    <button id="pause" class="btn btn-primary btn-sm" type="button">
      <i class="fa fa-pause" style="padding: 0rem 1rem"></i>
    </button>
    <span id="info" style="padding-left: 0.5rem;"></span>
  </div>

</body>
</html>
```

Much like the other examples, the above HTML loads the OpenLayers Javacscript
and CSS libraries, as well as the Bootstrap and Font Awesome CSS used to
display the play/pause buttons in the animation controller. An additional
`#controller` container is created below the map and contains the play/pause
bottons and a `span` element used to display the currently displayed time step.

#### Javascript

```javascript
const parser = new DOMParser();

/* Async function used to retrieve start and end time from RADAR_1KM_RRAI layer GetCapabilities document */
async function getRadarStartEndTime() {
  let response = await fetch(
    "https://geo.weather.gc.ca/geomet/?lang=en&service=WMS&request=GetCapabilities&version=1.3.0&LAYERS=RADAR_1KM_RRAI"
  );
  let data = await response
    .text()
    .then((data) =>
      parser
        .parseFromString(data, "text/xml")
        .getElementsByTagName("Dimension")[0]
        .innerHTML.split("/")
    );
  return [new Date(data[0]), new Date(data[1])];
}

let frameRate = 1.0; // frames per second
let animationId = null;
let startTime = null;
let endTime = null;
let current_time = null;

let layers = [
  new ol.layer.Tile({
    source: new ol.source.OSM()
  }),
  new ol.layer.Image({
    source: new ol.source.ImageWMS({
      format: "image/png",
      url: "https://geo.weather.gc.ca/geomet/",
      params: { LAYERS: "RADAR_1KM_RRAI"},
      transition: 0
    })
  }),
  new ol.layer.Image({
    source: new ol.source.ImageWMS({
      format: "image/png",
      url: "https://geo.weather.gc.ca/geomet/",
      params: { LAYERS: "RADAR_COVERAGE_RRAI.INV"},
      transition: 0
    })
  })
];

let map = new ol.Map({
  target: "map",
  layers: layers,
  view: new ol.View({
    center: ol.proj.fromLonLat([-97, 57]),
    zoom: 3
  })
});

function updateInfo(current_time) {
  let el = document.getElementById("info");
  el.innerHTML = `Time / Heure (UTC): ${current_time.toISOString()}`;
}

function setTime() {
  current_time = current_time;
  if (current_time === null) {
    current_time = startTime;
  } else if (current_time >= endTime) {
    current_time = startTime;
  } else {
    current_time = new Date(
      current_time.setMinutes(current_time.getMinutes() + 10)
    );
  }
  layers[1]
    .getSource()
    .updateParams({ TIME: current_time.toISOString().split(".")[0] + "Z" });
  layers[2]
    .getSource()
    .updateParams({ TIME: current_time.toISOString().split(".")[0] + "Z" });
  updateInfo(current_time);
}

getRadarStartEndTime().then((data) => {
  startTime = data[0];
  endTime = data[1];
  setTime();
});

let stop = function () {
  if (animationId !== null) {
    window.clearInterval(animationId);
    animationId = null;
  }
};

let play = function () {
  stop();
  animationId = window.setInterval(setTime, 1000 / frameRate);
};

let startButton = document.getElementById("play");
startButton.addEventListener("click", play, false);

let stopButton = document.getElementById("pause");
stopButton.addEventListener("click", stop, false);
```

In the Javscript code, an async function is created to retrieve the start
and end time currently available for the weather radar data (`RADAR_1KM_RRAI`).
When a response is received, the `DOMParser()` is used to retrieve the content
of the GetCapabilties `<Dimension>` tag and returns an array containing
two datetime objects representing the start and end time of the available
weather radar data.

Much like the other OpenLayers above, an array of layers is defined and passed
to the `ol.Map` constructor.

The `SetTime()` function is used to retrieved the current displayed datetime
and sets the time step to be displayed. If no time is set, the map is set to
display the retrieved start time. If the current displayed is greater or equal
to the retrieved end time, the time is reset to the retrieved start time,
enabling looping of the animation. Otherwise, the current time is retrieved
and 10 minutes are added to it, in order to retrieve the next available
timestep. The `RADAR_1KM_RRAI` and `RADAR_COVERAGE_RRAI` then get their
`TIME` parameter updated and OpenLayers automatically creates new requests
for these updated images.

The `getRadarStartEndTime()` function is called once to set the `startTime`
and `EndTime` variables and calls the `setTime(), setting the initial
map state.

Finally, `stop()` and `play()` functions are defined. The `play()` function
will call the `setTime()` function at a set interval, increasing the weather
radar timestep at each interval. The `stop()` function clears the interval.
These functions are then added as the callback function for click event
listeners assigned to their respective buttons in the HTML.

See the live example below:
<iframe height="275" style="width: 100%;" scrolling="no" title="GeoMet WMS Time Openlayers Example" src="https://codepen.io/dukestep/embed/abvoKye?height=275&theme-id=light&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/dukestep/pen/abvoKye'>GeoMet WMS Time Openlayers Example</a> by Dukestep
  (<a href='https://codepen.io/dukestep'>@dukestep</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>