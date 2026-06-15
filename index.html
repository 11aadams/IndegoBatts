<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Indego E-Bike Battery Dashboard</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.css" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.js"></script>
<style>
  body { font-family: -apple-system, Arial, sans-serif; margin: 0; background: #f4f5f7; color: #1a1a1a; }
  header { background: #00263a; color: #fff; padding: 16px 24px; }
  header h1 { margin: 0; font-size: 20px; }
  header p { margin: 4px 0 0; font-size: 13px; color: #b8c4cc; }
  .controls { padding: 12px 24px; background: #fff; border-bottom: 1px solid #ddd; display: flex; gap: 12px; align-items: center; flex-wrap: wrap; }
  input, button { padding: 8px 12px; border: 1px solid #ccc; border-radius: 6px; font-size: 14px; }
  button { background: #00b6e3; color: #fff; border: none; cursor: pointer; font-weight: 600; }
  button:hover { background: #0095bd; }
  .summary { display: flex; gap: 16px; padding: 16px 24px; flex-wrap: wrap; }
  .card { background: #fff; border-radius: 8px; padding: 12px 18px; box-shadow: 0 1px 3px rgba(0,0,0,0.08); min-width: 140px; }
  .card .num { font-size: 26px; font-weight: 700; }
  .card .label { font-size: 12px; color: #666; }
  table { width: 100%; border-collapse: collapse; background: #fff; }
  th, td { padding: 8px 12px; border-bottom: 1px solid #eee; text-align: left; font-size: 13px; vertical-align: top; }
  th { background: #f0f2f4; cursor: pointer; position: sticky; top: 0; }
  tr:hover { background: #f9fbfc; }
  .ebike-count { font-weight: 700; color: #00263a; }
  .note { padding: 12px 24px; font-size: 12px; color: #666; background: #fff8e1; border-top: 1px solid #f0e0a0; border-bottom: 1px solid #f0e0a0; }
  .table-wrap { max-height: 65vh; overflow-y: auto; margin: 0 24px 24px; border-radius: 8px; box-shadow: 0 1px 3px rgba(0,0,0,0.08); }
  .status { font-size: 12px; padding: 0 24px; color: #777; }
  .pill { display: inline-block; padding: 2px 6px; margin: 2px; border-radius: 10px; font-size: 11px; color: #fff; font-weight: 600; min-width: 30px; text-align: center; }
  .b-high { background: #2e9e3f; }
  .b-mid { background: #e0a800; }
  .b-low { background: #d9534f; }
  .b-unavail { opacity: 0.45; text-decoration: line-through; }
  .toggle-btn { background: none; border: none; color: #00b6e3; cursor: pointer; font-size: 12px; text-decoration: underline; padding: 0; }
</style>
</head>
<body>
<header>
  <h1>Ride Indego – Live Station, E-Bike &amp; Battery Dashboard</h1>
</header>




<div class="summary" id="summary"></div>
<div class="summary" id="threshold"></div>

<div style="display:flex; gap:16px; margin:0 24px 24px; flex-wrap: wrap;">
  <div class="note" id="routeSection" style="flex:1; min-width:300px; margin:0;">
    <strong>Top 10 stations with most e-bikes below 15% battery — driving route</strong>
    <div id="routePanel"></div>
  </div>
  <div id="map" style="flex:1; min-width:300px; height: 400px; border-radius: 8px;"></div>
</div>



<div class="controls">
  <input id="search" type="text" placeholder="Search station name or address...">
  <button onclick="loadData()">Refresh Data</button>
  <span class="status" id="status">Loading...</span>
</div>


<div class="table-wrap">
<table id="stationTable">
  <thead>
    <tr>
      <th onclick="sortBy('id')">Station ID</th>
      <th onclick="sortBy('name')">Station Name</th>
      <th onclick="sortBy('bikesAvailable')">Total Bikes</th>
      <th onclick="sortBy('electricBikesAvailable')">E-Bikes</th>
      <th onclick="sortBy('avgBattery')">Avg E-Bike Battery</th>
      <th onclick="sortBy('lowCount')">E-Bikes &lt;15%</th>
      <th>Per-Bike Battery (%)</th>
    </tr>
  </thead>
  <tbody id="tbody"></tbody>
</table>
</div>

<script>
let stations = [];
let sortKey = 'electricBikesAvailable', sortDir = -1;

function batteryClass(b) {
  if (b >= 60) return 'b-high';
  if (b >= 30) return 'b-mid';
  return 'b-low';
}

async function loadData() {
  document.getElementById('status').textContent = 'Loading...';
  try {
    const res = await fetch('https://bts-status.bicycletransit.workers.dev/phl');
    const data = await res.json();

    stations = data.features.map(f => {
      const p = f.properties;
      const ebikes = (p.bikes || []).filter(b => b.isElectric && b.battery !== null);
      const avgBattery = ebikes.length
        ? Math.round(ebikes.reduce((a,b)=>a+b.battery,0) / ebikes.length)
        : null;
      const lowCount = (p.bikes || []).filter(b => b.isElectric && b.battery !== null && b.battery < 15).length;
      return {
        id: p.id,
        name: p.name,
        lat: p.latitude,
        lon: p.longitude,
        bikesAvailable: p.bikesAvailable,
        electricBikesAvailable: p.electricBikesAvailable,
        bikes: p.bikes || [],
        avgBattery: avgBattery === null ? -1 : avgBattery,
        lowCount: lowCount
      };
    });

    document.getElementById('status').textContent =
      'Last updated: ' + new Date(data.last_updated).toLocaleTimeString();
    render();
  } catch (e) {
    document.getElementById('status').textContent = 'Error loading data: ' + e.message;
  }
}

function render() {
  const q = document.getElementById('search').value.toLowerCase();
  let rows = stations.filter(s => s.name.toLowerCase().includes(q));
  rows.sort((a,b) => (a[sortKey] > b[sortKey] ? 1 : -1) * sortDir);

  const totalBikes = stations.reduce((a,s)=>a+s.bikesAvailable,0);
  const totalEbikes = stations.reduce((a,s)=>a+s.electricBikesAvailable,0);
  const allBatteries = stations.flatMap(s => s.bikes.filter(b=>b.isElectric && b.battery!==null).map(b=>b.battery));
  const overallAvg = allBatteries.length ? Math.round(allBatteries.reduce((a,b)=>a+b,0)/allBatteries.length) : 0;
  const lowBattery = allBatteries.filter(b=>b<30).length;

  document.getElementById('summary').innerHTML = `
    <div class="card"><div class="num">${stations.length}</div><div class="label">Stations</div></div>
    <div class="card"><div class="num">${totalBikes}</div><div class="label">Total Bikes Available</div></div>
    <div class="card"><div class="num">${totalEbikes}</div><div class="label">Total E-Bikes</div></div>
    <div class="card"><div class="num">${overallAvg}%</div><div class="label">System-wide Avg E-Bike Battery</div></div>
    <div class="card"><div class="num">${lowBattery}</div><div class="label">E-Bikes Below 30% Battery</div></div>
  `;

  const below15 = allBatteries.filter(b=>b<15).length;
  const above15 = allBatteries.length - below15;
  const pctBelow = allBatteries.length ? (below15/allBatteries.length*100).toFixed(1) : '0.0';
  const pctAbove = allBatteries.length ? (above15/allBatteries.length*100).toFixed(1) : '0.0';

  document.getElementById('threshold').innerHTML = `
    <div class="card"><div class="num">${pctBelow}%</div><div class="label">E-Bikes Below 15% Battery (${below15})</div></div>
    <div class="card"><div class="num">${pctAbove}%</div><div class="label">E-Bikes Above 15% Battery (${above15})</div></div>
  `;

  // Top 10 stations by count of low-battery (<15%) e-bikes
  const stationLow = stations.map(s => ({
    id: s.id, name: s.name, lat: s.lat, lon: s.lon, lowCount: s.lowCount
  })).filter(s => s.lowCount > 0)
    .sort((a,b) => b.lowCount - a.lowCount)
    .slice(0, 10);

  let routeHtml = '';
  if (stationLow.length) {
    routeHtml += '<ol>' + stationLow.map(s => `<li>${s.name} (Station ${s.id}) — ${s.lowCount} low-battery e-bike(s)</li>`).join('') + '</ol>';
    const START = '39.916190,-75.183320';
    const waypoints = stationLow.slice(0, -1).map(s => `${s.lat},${s.lon}`).join('|');
    const destination = `${stationLow[stationLow.length-1].lat},${stationLow[stationLow.length-1].lon}`;
    let url = `https://www.google.com/maps/dir/?api=1&origin=${START}&destination=${destination}&travelmode=driving`;
    if (waypoints) url += `&waypoints=${encodeURIComponent(waypoints)}`;
    routeHtml += `<p>Starting point fixed at <code>${START}</code>.</p>`;
    routeHtml += `<p><a href="${url}" target="_blank">Open driving route in Google Maps</a></p>`;
    updateMap(stationLow, START);
  } else {
    routeHtml = '<p>No stations currently have e-bikes below 15% battery.</p>';
  }
  document.getElementById('routePanel').innerHTML = routeHtml;

  document.getElementById('tbody').innerHTML = rows.map(s => {
    const ebikes = s.bikes.filter(b => b.isElectric);
    const pills = ebikes.map(b => {
      const cls = batteryClass(b.battery ?? 0);
      const unavail = b.isAvailable ? '' : ' b-unavail';
      const title = `Dock ${b.dockNumber}${b.isAvailable ? '' : ' (not rentable)'}`;
      return `<span class="pill ${cls}${unavail}" title="${title}">${b.battery}%</span>`;
    }).join('');
    return `
    <tr>
      <td>${s.id}</td>
      <td>${s.name}</td>
      <td>${s.bikesAvailable}</td>
      <td class="ebike-count">${s.electricBikesAvailable}</td>
      <td>${s.avgBattery >= 0 ? s.avgBattery + '%' : '—'}</td>
      <td>${s.lowCount}</td>
      <td>${pills || '—'}</td>
    </tr>`;
  }).join('');
}

function sortBy(key) {
  if (sortKey === key) sortDir *= -1; else { sortKey = key; sortDir = -1; }
  render();
}

let map, markerLayer, routeLayer;
const START_COORD = [39.916190, -75.183320];

function initMap() {
  map = L.map('map').setView(START_COORD, 12);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap contributors'
  }).addTo(map);
  markerLayer = L.layerGroup().addTo(map);
  routeLayer = L.layerGroup().addTo(map);

  L.marker(START_COORD, {
    icon: L.divIcon({className: '', html: '<div style="background:#00263a;color:#fff;border-radius:50%;width:14px;height:14px;border:2px solid #fff;"></div>'})
  }).addTo(map).bindPopup('Start point');
}

function updateMap(stationLow, startStr) {
  markerLayer.clearLayers();
  routeLayer.clearLayers();

  const start = startStr.split(',').map(Number);
  const points = [start, ...stationLow.map(s => [s.lat, s.lon])];

  stationLow.forEach((s, i) => {
    L.marker([s.lat, s.lon])
      .addTo(markerLayer)
      .bindPopup(`#${i+1}: ${s.name} (Station ${s.id})<br>${s.lowCount} e-bike(s) &lt;15%`);
  });

  map.fitBounds(L.latLngBounds(points), {padding: [30,30]});
}

document.getElementById('search').addEventListener('input', render);
initMap();
loadData();
setInterval(loadData, 60000);
</script>
</body>
</html>
