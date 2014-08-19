cims_original
=============

cims_original



// the infos object is used to track layer visibility and position
var map, eNightTallyLayer, eNightPollLayer, infos = {};

require(["esri/map", "esri/dijit/Legend",
		 "esri/dijit/LayerSwipe", "esri/dijit/HomeButton", "esri/dijit/Geocoder",
		 "esri/graphic", "esri/arcgis/utils",
		 "esri/symbols/SimpleMarkerSymbol",
		 "esri/geometry/screenUtils",
		 
		 "dojo/dom", "dojo/dom-construct", "dojo/dom-style",
		 "dojo/query", "dojo/on", "dojo/parser", "dojo/_base/array", 
		 "dojo/dnd/Source", "dijit/registry", "dojo/_base/Color",
		 
		 "dojo/domReady!",
		 
		 "esri/layers/ArcGISDynamicMapServiceLayer",
		 "esri/layers/DynamicLayerInfo", "esri/layers/LayerDataSource",
		 "esri/layers/LayerDrawingOptions", "esri/layers/TableDataSource",
		 "esri/Color", 
		 "esri/renderers/SimpleRenderer",
		 "esri/symbols/SimpleFillSymbol", "esri/symbols/SimpleLineSymbol"


//start xfer
/*
		  "esri/urlUtils",
      "esri/map",
      "esri/lang",
      "esri/graphic",
      "esri/InfoTemplate",
      "esri/layers/GraphicsLayer",
      "esri/renderers/SimpleRenderer",

      "esri/geometry/Point",
      "esri/tasks/FeatureSet",

      "esri/tasks/ClosestFacilityTask",
      "esri/tasks/ClosestFacilityParameters",

      "esri/symbols/SimpleMarkerSymbol",
      "esri/symbols/SimpleLineSymbol",
      
      "dijit/form/ComboBox",
      "dijit/layout/BorderContainer",
      "dijit/layout/ContentPane"
      */
      //end sfer

		

		],

		 function
		 (
					 Map, Legend, LayerSwipe, HomeButton, Geocoder, Popup, PopupTemplate,			  
					 ArcGISDynamicMapServiceLayer, arcgisUtils, array, Graphic, Color,
					 DynamicLayerInfo, LayerDataSource, LayerDrawingOptions, TableDataSource,
					 SimplemarkerSymbol, SimpleRenderer, SimpleFillSymbol, SimpleLineSymbol,
					 screenUtils, dom, domConstruct, domStyle, query, on, 
					 parser, arrayUtils, Source, registry,

					 parser,
      urlUtils, esriLang, InfoTemplate, GraphicsLayer, SimpleRenderer, 
      Point, FeatureSet, 
      ClosestFacilityTask, ClosestFacilityParameters 

	
      ) 



//here

		{
			
			/*var incidentsGraphicsLayer, routeGraphicLayer, closestFacilityTask;
   
      parser.parse();
      
      urlUtils.addProxyRule({
        urlPrefix: "route.arcgis.com",  
        proxyUrl: "/sproxy"
      });
*/



			/*	   
			// instantiate a new simpleline and fill symbol instance to use when we select a feature in a layer	   
			var sls = new SimpleLineSymbol("solid", new Color("#444444"), 3);
			var sfs = new SimpleFillSymbol("solid", sls, new Color([68, 68, 68, 0.25]));
			// instantiate a new popup instance to show information about the selected feature
			var popup = new Popup({
				fillSymbol: sfs,
				lineSymbol: null,
				markerSymbol: null
			}, domConstruct.create("div"));*/
				   
			

			// assign the map instance object to the mapDiv and specify parameters
			map = new Map("mapDiv", {
				center: [-118.345, 33.990], // [lon, lat]- centers on Los Angeles
				zoom: 12,
				logo: false,
				basemap: "national-geographic" //"topo"				
			});
			

//here again
/*
map.on("click", mapClickHandler);
      
      params = new ClosestFacilityParameters();
      params.impedenceAttribute= "Miles";      
      params.defaultCutoff= 7.0;      
      params.returnIncidents=false;
      params.returnRoutes=true;
      params.returnDirections=true;
      
      map.on("load", function(evtObj) {
        var map = evtObj.target;
        var facilityPointSymbol = new SimpleMarkerSymbol(
          SimpleMarkerSymbol.STYLE_SQUARE, 
          20,
          new SimpleLineSymbol(
            SimpleLineSymbol.STYLE_SOLID,
            new Color([89,95,35]), 2
          ),
          new Color([130,159,83,0.40])
        ); 

        var incidentPointSymbol = new SimpleMarkerSymbol(
          SimpleMarkerSymbol.STYLE_CIRCLE, 
          16,
          new SimpleLineSymbol(
            SimpleLineSymbol.STYLE_SOLID,
            new Color([89,95,35]), 2
          ),
          new Color([130,159,83,0.40])
        );  

        incidentsGraphicsLayer = new GraphicsLayer();
        
        var incidentsRenderer = new SimpleRenderer(incidentPointSymbol);
        incidentsGraphicsLayer.setRenderer(incidentsRenderer);
        map.addLayer(incidentsGraphicsLayer);

        routeGraphicLayer = new GraphicsLayer();
        
        var routePolylineSymbol = new SimpleLineSymbol(
          SimpleLineSymbol.STYLE_SOLID, 
          new Color([89,95,35]), 
          4.0
        );
        var routeRenderer = new SimpleRenderer(routePolylineSymbol);
        routeGraphicLayer.setRenderer(routeRenderer);

        map.addLayer(routeGraphicLayer);

        var facilitiesGraphicsLayer = new GraphicsLayer();
        var facilityRenderer = new SimpleRenderer(facilityPointSymbol);
        facilitiesGraphicsLayer.setRenderer(facilityRenderer);
       
        map.addLayer(facilitiesGraphicsLayer);
        
        facilitiesGraphicsLayer.add(new Graphic(new Point(-13625960,4549921,map.spatialReference)));
        facilitiesGraphicsLayer.add(new Graphic(new Point(-13626184,4549247,map.spatialReference)));
        facilitiesGraphicsLayer.add(new Graphic(new Point(-13626477,4549415,map.spatialReference)));
        facilitiesGraphicsLayer.add(new Graphic(new Point(-13625385,4549659,map.spatialReference))); 
        facilitiesGraphicsLayer.add(new Graphic(new Point(-13624374,4548254,map.spatialReference))); 
        facilitiesGraphicsLayer.add(new Graphic(new Point(-13624891,4548565,map.spatialReference))); 
   
        var facilities = new FeatureSet();
        facilities.features = facilitiesGraphicsLayer.graphics;
        
        params.facilities = facilities;
        params.outSpatialReference = map.spatialReference;
       
      });
      
      closestFacilityTask = new ClosestFacilityTask("http://route.arcgis.com/arcgis/rest/services/World/ClosestFacility/NAServer/ClosestFacility_World");

      registry.byId("numLocations").on("change", function() {
        params.defaultTargetFacilityCount = this.value;
        clearGraphics();
      });

      function clearGraphics() {
        //clear graphics
        dom.byId("directionsDiv").innerHTML= "";
        map.graphics.clear();
        routeGraphicLayer.clear();
        incidentsGraphicsLayer.clear();    
      } 

      function mapClickHandler(evt) {
        clearGraphics();
        dom.byId("directionsDiv").innerHTML= "";
        var inPoint = new Point(evt.mapPoint.x, evt.mapPoint.y, map.spatialReference);
        var location = new Graphic(inPoint);
        incidentsGraphicsLayer.add(location);
        
        var features = [];
        features.push(location);
        var incidents = new FeatureSet();
        incidents.features = features;
        params.incidents = incidents;
        
        map.graphics.enableMouseEvents();
       
        routeGraphicLayer.on("mouse-over", function(evt){
          //clear existing directions and highlight symbol
          map.graphics.clear();
          dom.byId("directionsDiv").innerHTML= "Hover over the route to view directions";
          
          var highlightSymbol = new SimpleLineSymbol(SimpleLineSymbol.STYLE_SOLID, new Color([0,255,255],0.25), 4.5);
          var highlightGraphic = new Graphic(evt.graphic.geometry,highlightSymbol);
          
          map.graphics.add(highlightGraphic);
          dom.byId("directionsDiv").innerHTML = esriLang.substitute(evt.graphic.attributes,"${*}");
        });

        //solve 
        closestFacilityTask.solve(params, function(solveResult){
          var directions = solveResult.directions;
          array.forEach(solveResult.routes, function(route, index){
            //build an array of route info
            var attr = array.map(solveResult.directions[index].features, function(feature){
              return feature.attributes.text;
            });
            var infoTemplate = new InfoTemplate("Attributes", "${*}");
            
            route.setInfoTemplate(infoTemplate);
            route.setAttributes(attr);
            
            routeGraphicLayer.add(route);
            dom.byId("directionsDiv").innerHTML = "Hover over the route to view directions";
          });
          
          //display any messages
          if(solveResult.messages.length > 0){
            dom.byId("directionsDiv").innerHTML = "<b>Error:</b> " + solveResult.messages[0];
          }      
        });
      }
    });    
*/
//end of here again








			// add eNight_TallyStatus ArcGIS Server service as a dynamicservicelayer
			var tallyURL = "http://14gisp:6080/arcgis/rest/services/eNight_TallyStatus/MapServer";
			eNightTallyLayer = new esri.layers.ArcGISDynamicMapServiceLayer(tallyURL,{"opacity":0.85});
			eNightTallyLayer.id = "lyrTallyStatus";
			eNightTallyLayer.refreshInterval = 1;
			
			// add eNight_PollStatus ArcGIS Server service as a dynamicservicelayer
			var pollURL = "http://14gisp:6080/arcgis/rest/services/eNight_PollStatus/MapServer";
			eNightPollLayer = new esri.layers.ArcGISDynamicMapServiceLayer(pollURL); //,{"opacity":0.75});
			eNightPollLayer.id = "lyrPollStatus";
			eNightPollLayer.refreshInterval = 1;
			
			// add Layers to the map
			map.addLayers([eNightTallyLayer, eNightPollLayer]);
			
			// create the map legend
			var legend = new esri.dijit.Legend({
				map:map
			},"legendDiv");
			legend.startup();
			
			// create the swipe widget to show the two status layers
			var swipeWidget = new LayerSwipe({
				type: "vertical",  //Try switching to "scope" or "horizontal"
				map: map,
				left: 500,
				layers: [eNightPollLayer]
			}, "swipeDiv");
			swipeWidget.startup();
						
			// set the Home Button which when clicked, brings the view back to the original extent
			var home = new HomeButton({
				map: map
			}, "HomeButton");
			home.startup();
			
			// Geocoder Widget
			var geocoder = new esri.dijit.Geocoder({
				map: map,
				autoComplete: false,
				arcgisGeocoder: {
					name: "Esri World Geocoder",
					placeholder: "Find Address"
				}
			},"searchAddress");
			geocoder.startup();
			
			// Geocoder Widget Select Event Listener
			geocoder.on("select", showLocation);
			
			// Zooms in on the location of the found point by the Geocoder widget
			function showLocation(evt) {
				map.graphics.clear();
				var point = evt.result.feature.geometry;
				
				if (point == null) {
					alert("Could not find that address.\nPlease try again.");
				} else {
					/*
					var symbol = new esri.symbol.PictureMarkerSymbol("images/pins/red-pin.png", 17, 32); 
					var graphic = new Graphic(point, symbol);					
					map.graphics.add(graphic);
					*/
												
					map.infoWindow.setTitle("Please add closest polling places for future enhancement.  Here's Your Search Result:");
					map.infoWindow.setContent(evt.result.name);
					map.infoWindow.show(evt.result.feature.geometry);
				}
			}
			
			
			$(document).ready(function () {
				$("#basemapList li").click(function (e) {
					switch (e.target.text) {
						case "Streets":
							map.setBasemap("streets");
							break;
						case "Imagery":
							map.setBasemap("hybrid");
							break;
						case "National Geographic":
							map.setBasemap("national-geographic");
							break;
						case "Topographic":
							map.setBasemap("topo");
							break;
						case "Gray":
							map.setBasemap("gray");
							break;
						case "Open Street Map":
							map.setBasemap("osm");
							break;
					}
				});
			  
				$('.navbar-brand').sidr({
					name: 'respNav',
					source: '.nav-collapse',
				});
			});
			
			//this code is close sidr menu if clicked outside  {optional}
			//$(document).bind("click", function () {
			$('#mapDiv').bind("click", function () {
				$.sidr('close', 'respNav');
			});
						$('.navbar').bind("click", function () {
				$.sidr('close', 'respNav');
			});
			//end manual collapse
});	
