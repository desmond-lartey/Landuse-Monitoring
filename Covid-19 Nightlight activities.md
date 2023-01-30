//GELDERLAND NIGHT LIGHT 2015

Map.addLayer(table)

// Select Area of Interest

var geometry = table.filter(ee.Filter.eq('ADM1_NAME' , 'Gelderland'))
Map.centerObject(geometry)


//Filter image collection
var image2015 = image.filter(ee.Filter.date('2015-05-01', '2015-05-31'))//.select('avg_rad').first();

//select the fist image from the collection or select first image from composite
var firstimage = image2015.first();

//Now select the 2015 nightlight band from the first image
var image2015Nightlight = firstimage.select('avg_rad').clip(geometry)

//Create a visualisation parameter
var image2015vis = {
  min: 0.0,
  max: 60,
  palette: ['black', 'white']
};

Map.addLayer(image2015Nightlight, image2015vis, 'Gelderland Nightlight 2015')

//GELDERLAND NIGHT LIGHT 2020

//Filter image collection

var image2020 = image.filter(ee.Filter.date('2020-05-01', '2020-05-31'));

//select the first image from the collection or select first image from composite
var firstimage = image2020.first();

//Now select the 2020 Nightlight band from the first image
var image2020Nightlight = firstimage.select('avg_rad').clip(geometry);

//Create a visualisation parameters
var image2020vis = {
  min: 0.0,
  max: 60,
  palette: ['black', 'white']
};

Map.addLayer(image2020Nightlight, image2020vis, 'Gelderland Nightlight 2020')

//Export image 2015
Export.image.toDrive({
  image: image2015Nightlight, 
  description: '2015_Nightlight', 
  folder: 'GEE', 
  fileNamePrefix: 'Nightlight_2015' , 
  region: geometry, 
  scale: 463.83, 
  maxPixels: 1e10, 

});

//Export image 2020
Export.image.toDrive({
  image: image2015Nightlight, 
  description: '2020_Nightlight', 
  folder: 'GEE', 
  fileNamePrefix: 'Nightlight_2020' , 
  region: geometry, 
  scale: 463.83, 
  maxPixels: 1e10, 

});
