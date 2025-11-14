---
layout: default
title: Imperviousness Map
---

<div id="map" style="height: 400px;"></div>
<div id="solution" style="margin-top: 1em;"></div>
<link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
<script>
var map = L.map('map').setView([50.873, 6.7], 12); // Center for Kerpen
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

fetch('https://github.com/dewminanayakkara1/Trial1/blob/a854db36fb3fe11fad169b383ca9e65957256b90/Imperviousness%20Solutions.geojson') // <-- paste your raw URL here
  .then(res => res.json())
  .then(data => {
    L.geoJSON(data, {
      style: function(feature) {
        var perc = feature.properties['%']; // Use the name of your field exactly (here: %)
        return {
          color:
            perc <= 30 ? 'green' :
            perc <= 70 ? 'orange' : 'red',
          fillOpacity: 0.5
        };
      },
      onEachFeature: function(feature, layer) {
        layer.on('click', function() {
          // Suggest solution based on imperviousness
          var perc = feature.properties['%'];
          var solution = perc <= 30 ? 'Infiltration (e.g. rain gardens, soakaways)' :
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
