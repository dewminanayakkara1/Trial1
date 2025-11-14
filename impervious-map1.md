---
layout: default
title: Imperviousness Map1
---

<div id="map" style="height: 500px; margin-bottom:1em;"></div>
<div id="solution" style="margin-top: 1em; font-size: 1.1em;"></div>
<link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
<script>
// Center your map to the local area (update coordinates/zoom if needed)
var map = L.map('map').setView([50.900, 6.700], 13);
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
  attribution: '© OpenStreetMap contributors'
}).addTo(map);

fetch('https://raw.githubusercontent.com/dewminanayakkara1/Trial1/6f8505ded645064e85d06230b0ba997ccb72af10/Imperviousness%20Solutions1.json')
  .then(res => res.json())
  .then(data => {
    L.geoJSON(data, {
      style: function(feature) {
        var perc = feature.properties['%'];
        // Color-coding based on % value
        return {
          color: perc <= 30 ? 'green' : perc <= 70 ? 'orange' : 'red',
          fillOpacity: 0.5,
          weight: 1
        };
      },
      onEachFeature: function(feature, layer) {
        // Popup with info and solution
        var perc = feature.properties['%'];
        var solution = perc <= 30 ? 'Infiltration (rain gardens, soakaways)' :
                       perc <= 70 ? 'Mixed (infiltration + tanks/permeable pavement)' :
                       'Engineered (green roofs, tanks, permeable pavement)';
        var solText = feature.properties['Solution'] ? feature.properties['Solution'] : solution;
        layer.on('click', function() {
          document.getElementById('solution').innerHTML =
            'Imperviousness: <b>' + perc + '%</b><br>Recommended Solution: <b>' + solText + '</b>';
        });
        layer.bindPopup('Imperviousness: ' + perc + '%<br>Solution: ' + solText);
      }
    }).addTo(map);
  });
</script>
