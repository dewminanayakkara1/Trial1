---
layout: default
title: Find Solutions
---

# Find Rainwater Solutions for Your Home

Fill in the details below to get relevant recommendations.

<form id="solution-form">
  <label>Postal Code: <input type="text" name="postal" required></label><br>
  <label>Housing Area (m²): <input type="number" name="area"></label><br>
  <label>Housing Type:</label>
    <select name="type">
      <option value="detached">Detached</option>
      <option value="semi-detached">Semi-detached</option>
      <option value="apartment">Apartment</option>
    </select><br>
  <label>Garden?</label>
    <select name="garden">
      <option value="yes">Yes</option>
      <option value="no">No</option>
    </select><br>
  <button type="button" onclick="showResults()">Find Solutions</button>
</form>

<div id="results"></div>

<script>
function showResults() {
  var type = document.querySelector('[name="type"]').value;
  var garden = document.querySelector('[name="garden"]').value;
  var results = '';
  if(type == 'detached' && garden == 'yes') {
    results = "<ul><li>Rain gardens</li><li>Infiltration trenches</li><li>Rainwater tanks</li></ul>";
  }
  if(type == 'apartment') {
    results = "<ul><li>Green roofs</li><li>Permeable pavements</li></ul>";
  }
  // Add more filters as needed!
  if(results == '') results = "Please check solution types on our site or contact us.";
  document.getElementById('results').innerHTML = results;
}
</script>
