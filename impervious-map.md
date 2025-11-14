---
layout: default
title: Imperviousness Map
---

<div id="map" style="height: 400px;"></div>
<div id="solution" style="margin-top: 1em;"></div>
<link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
<script>
var map = L.map('map').setView([50.873, 6.7], 12); // Adjust center/zoom for Kerpen
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

fetch('Imperviousness Solutions.geojson') // Use the exact filename as in your repo
  .then(res => res.json())
  .then(data => {
    L.geoJSON(data, {
      style: f => ({
        color:
          f.properties['%'] <= 30 ? 'green' :
          f.properties['%'] <= 70 ? 'yellow' : 'purple',
        fillOpacity: 0.5
      }),
      onEachFeature: function(feature, layer) {
        layer.on('click', function() {
          // Determine the solution based on '%' value
          var perc = feature.properties['%'];
          var solution = perc <= 30 ? 'Infiltration (rain gardens, soakaways)' :
                         perc <= 70 ? 'Mixed (infiltration + tanks/permeable pavement)' :
                         'Engineered (green roofs, tanks, permeable pavement)';
          document.getElementById('solution').innerHTML =
            'Imperviousness: <b>' + perc + '%</b><br>Recommended Solution: <b>' + solution + '</b>';
        });
        layer.bindPopup('Imperviousness: ' + feature.properties['%'] + '%');
      }
    }).addTo(map);
  });
</script>
