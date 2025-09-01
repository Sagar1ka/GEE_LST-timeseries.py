// Plot a time series of a band's value in regions of the American West.
var trieste = ee.Feature(ee.Geometry.Polygon(
        [[[13.753852844238281, 45.642487994553925],
           [13.763465881347656, 45.63936753872803],
           [13.7713623046875, 45.64008765934397],
           [13.7713623046875, 45.64896838595769],
           [13.766899108886719, 45.64896838595769]]]),
           {label: 'City'});

var karst = ee.Feature(ee.Geometry.Polygon(
       [[[13.853759765625, 45.719603972998634],
          [13.85650634765625, 45.69275137290874],
          [13.8812255859375, 45.69275137290874],
          [13.875732421875, 45.719603972998634]]]),
          {label: 'karst'});
          
var sellanevea = ee.Feature(ee.Geometry.Polygon(
        [[[13.4527587890625, 46.4378568950242],
          [13.4527587890625, 46.41892578708076],
          [13.4967041015625, 46.42081919374915],
          [13.4967041015625, 46.44164232762498]]]),
           {label: 'sella'});
           
var adria = ee.Feature(ee.Geometry.Polygon(
        [[[12.98583984375, 45.39844997630408],
          [12.98583984375, 45.089035564831036],
          [13.38134765625, 45.12005284153054],
          [13.38134765625, 45.49094569262733]]]),
           {label: 'adria'});
var COLOR = {
  trieste: 'ff0000',
  karst: '0000ff',
  sellanevea: '00ff00',
  adria: '0099ff'
};
//var westernRegions = new ee.FeatureCollection([trieste, karst, sellanevea]);
var westernRegions = new ee.FeatureCollection([trieste, adria, sellanevea]);

// Get brightness temperature data for 1 year.
var landsat8Toa = ee.ImageCollection('LANDSAT/LC8_L1T_32DAY_TOA');
var temps2013 = landsat8Toa.filterBounds(westernRegions)
    .filterDate('2012-12-25', '2016-12-25')
    .select('B10');

// Convert temperature to Celsius.
temps2013 = temps2013.map(function(image) {
  return image.addBands(image.subtract(273.15).select([0], ['Temp']));
});

var tempTimeSeries = ui.Chart.image.seriesByRegion({
  imageCollection: temps2013,
  regions: westernRegions,
  reducer: ee.Reducer.mean(),
  band: 'Temp',
  scale: 200,
  xProperty: 'system:time_start',
  seriesProperty: 'label'
});
tempTimeSeries.setChartType('ScatterChart');
tempTimeSeries.setOptions({
  title: 'Temperature over time in regions of the FVG',
  vAxis: {
    title: 'Temperature (parameter)'
  },
  lineWidth: 1,
  pointSize: 4,
  series: {
    0: {color: COLOR.trieste},
    1: {color: COLOR.adria},
    2: {color: COLOR.sella}
  }
});

print(tempTimeSeries);

Map.addLayer(trieste, {color: COLOR.trieste});
Map.addLayer(karst, {color: COLOR.karst});
Map.addLayer(sellanevea, {color: COLOR.sella});
Map.addLayer(adria, {color: COLOR.adria});

Map.setCenter(13.5, 45.7, 6);
