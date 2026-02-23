<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SK's 2026 Master – Mission Control</title>

<!-- Leaflet CSS -->

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<style>
* {
    box-sizing: border-box;
}

body {
margin:0;
font-family: ‘Segoe UI’, Tahoma, Geneva, Verdana, sans-serif;
background: radial-gradient(circle at top, #001933 0%, #000814 80%);
color:white;
overflow-x:hidden;
}

body::before {
content:””;
position:fixed;
width:100%;
height:100%;
background:url(‘https://www.transparenttextures.com/patterns/stardust.png’);
opacity:0.2;
z-index:-1;
pointer-events:none;
}

/* LOGIN SCREEN */
#loginScreen {
display:flex;
justify-content:center;
align-items:center;
height:100vh;
flex-direction:column;
animation: fadeIn 0.5s;
}

.login-container {
background: rgba(0, 191, 255, 0.05);
padding: 40px;
border-radius: 15px;
box-shadow: 0 0 30px rgba(0, 191, 255, 0.3);
backdrop-filter: blur(10px);
max-width: 400px;
width: 90%;
}

.login-container h1 {
text-align: center;
margin-bottom: 30px;
font-size: 1.8em;
background: linear-gradient(135deg, #00bfff, #00ffff);
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
background-clip: text;
}

input, select {
width: 100%;
padding:12px;
margin:10px 0;
border:2px solid rgba(0, 191, 255, 0.3);
border-radius:8px;
background: rgba(0, 0, 0, 0.3);
color: white;
font-size: 14px;
transition: all 0.3s;
}

input:focus, select:focus {
outline: none;
border-color: #00bfff;
box-shadow: 0 0 15px rgba(0, 191, 255, 0.5);
}

button {
width: 100%;
padding:12px 20px;
background: linear-gradient(135deg, #00bfff, #0099cc);
border:none;
border-radius:8px;
color:white;
cursor:pointer;
font-size: 16px;
font-weight: bold;
margin-top: 15px;
transition: all 0.3s;
}

button:hover {
transform: translateY(-2px);
box-shadow: 0 5px 20px rgba(0, 191, 255, 0.5);
}

button:active {
transform: translateY(0);
}

.error-message {
color: #ff4444;
text-align: center;
margin-top: 10px;
display: none;
animation: shake 0.3s;
}

/* DASHBOARD */
#dashboard {
display:none;
padding:20px;
max-width: 1600px;
margin: 0 auto;
animation: fadeIn 0.5s;
}

.header {
display: flex;
justify-content: space-between;
align-items: center;
margin-bottom: 30px;
flex-wrap: wrap;
gap: 15px;
}

.header h1 {
font-size: 2em;
margin: 0;
background: linear-gradient(135deg, #00bfff, #00ffff);
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
background-clip: text;
}

.user-info {
background: rgba(0, 191, 255, 0.1);
padding: 10px 20px;
border-radius: 8px;
display: flex;
align-items: center;
gap: 15px;
}

.logout-btn {
width: auto;
padding: 8px 16px;
font-size: 14px;
margin: 0;
}

.kpi-container {
display:grid;
grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
gap:20px;
margin-bottom: 30px;
}

.kpi {
background:rgba(0,191,255,0.1);
padding:25px;
border-radius:12px;
box-shadow:0 0 15px rgba(0, 191, 255, 0.3);
animation:pulse 3s infinite;
transition: transform 0.3s;
position: relative;
overflow: hidden;
}

.kpi:hover {
transform: translateY(-5px);
box-shadow:0 5px 25px rgba(0, 255, 255, 0.5);
}

.kpi::before {
content: ‘’;
position: absolute;
top: 0;
left: -100%;
width: 100%;
height: 100%;
background: linear-gradient(90deg, transparent, rgba(255,255,255,0.1), transparent);
transition: left 0.5s;
}

.kpi:hover::before {
left: 100%;
}

.kpi h2 {
margin: 0 0 10px 0;
font-size: 0.9em;
color: #00bfff;
text-transform: uppercase;
letter-spacing: 1px;
}

.kpi h1 {
margin: 0;
font-size: 2.5em;
color: #00ffff;
text-shadow: 0 0 10px rgba(0, 255, 255, 0.5);
}

.kpi .trend {
font-size: 0.8em;
color: #4ade80;
margin-top: 5px;
}

.kpi .trend.down {
color: #ff6b6b;
}

@keyframes pulse {
0%, 100% { box-shadow:0 0 15px rgba(0, 191, 255, 0.3); }
50% { box-shadow:0 0 25px rgba(0, 255, 255, 0.6); }
}

.controls {
display: flex;
gap: 15px;
margin-bottom: 20px;
flex-wrap: wrap;
}

.controls select, .controls button {
width: auto;
min-width: 150px;
}

.map-container {
background: rgba(0, 191, 255, 0.05);
padding: 20px;
border-radius: 15px;
margin-bottom: 20px;
box-shadow: 0 0 20px rgba(0, 191, 255, 0.2);
}

.map-container h2 {
margin-top: 0;
color: #00bfff;
}

#map, #globe {
height:500px;
border-radius:12px;
box-shadow: 0 5px 20px rgba(0, 0, 0, 0.3);
}

.stats-grid {
display: grid;
grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
gap: 20px;
margin-top: 20px;
}

.stat-card {
background: rgba(0, 191, 255, 0.05);
padding: 20px;
border-radius: 12px;
border: 1px solid rgba(0, 191, 255, 0.2);
}

.stat-card h3 {
margin-top: 0;
color: #00bfff;
font-size: 1.1em;
}

.stat-list {
list-style: none;
padding: 0;
margin: 0;
}

.stat-list li {
padding: 10px 0;
border-bottom: 1px solid rgba(0, 191, 255, 0.1);
display: flex;
justify-content: space-between;
}

.stat-list li:last-child {
border-bottom: none;
}

@keyframes fadeIn {
from { opacity: 0; transform: translateY(20px); }
to { opacity: 1; transform: translateY(0); }
}

@keyframes shake {
0%, 100% { transform: translateX(0); }
25% { transform: translateX(-10px); }
75% { transform: translateX(10px); }
}

/* MOBILE RESPONSIVE */
@media (max-width: 768px) {
.header h1 {
font-size: 1.5em;
}

```
.kpi h1 {
    font-size: 2em;
}

#map, #globe {
    height: 350px;
}

.controls {
    flex-direction: column;
}

.controls select, .controls button {
    width: 100%;
}
```

}

/* Loading animation */
.loading {
display: inline-block;
width: 20px;
height: 20px;
border: 3px solid rgba(0, 191, 255, 0.3);
border-radius: 50%;
border-top-color: #00bfff;
animation: spin 1s linear infinite;
}

@keyframes spin {
to { transform: rotate(360deg); }
}

</style>
</head>
<body>

<!-- LOGIN SCREEN -->

<div id="loginScreen">
    <div class="login-container">
        <h1>🌍 Executive Access</h1>
        <h3 style="text-align:center; color:#00bfff; margin-top:0;">SK's 2026 Master</h3>
        <input type="password" id="password" placeholder="Enter Access Code" autocomplete="current-password">
        <select id="roleSelect">
            <option value="viewer">Viewer Access</option>
            <option value="admin">Administrator</option>
        </select>
        <button onclick="login()">Enter Mission Control</button>
        <div class="error-message" id="errorMessage">Access Denied - Invalid Credentials</div>
        <p style="text-align:center; font-size:0.8em; color:#666; margin-top:20px;">
            Demo: Use password "elite2026"
        </p>
    </div>
</div>

<!-- DASHBOARD -->

<div id="dashboard">
    <div class="header">
        <h1>🌍 SK's 2026 Master – Global Mission Control</h1>
        <div class="user-info">
            <span id="userRole">👤 Viewer</span>
            <button class="logout-btn" onclick="logout()">Logout</button>
        </div>
    </div>

```
<div class="kpi-container">
    <div class="kpi">
        <h2>Total Workforce</h2>
        <h1 id="totalWorkforce">176</h1>
        <div class="trend">↑ +8% this month</div>
    </div>
    <div class="kpi">
        <h2>Active Contracts</h2>
        <h1 id="activeContracts">7</h1>
        <div class="trend">→ Stable</div>
    </div>
    <div class="kpi">
        <h2>Active Territories</h2>
        <h1 id="territories">5</h1>
        <div class="trend">↑ +2 this quarter</div>
    </div>
    <div class="kpi">
        <h2>Monthly Revenue</h2>
        <h1 id="revenue">$2.4M</h1>
        <div class="trend">↑ +12% vs last month</div>
    </div>
</div>

<div class="controls">
    <select id="contractFilter" onchange="filterContract()">
        <option value="all">All Contracts</option>
        <option value="Gymnation">Gymnation</option>
        <option value="Wellfit">Wellfit</option>
        <option value="Mercedes">Mercedes</option>
        <option value="Emirates">Emirates Facilities</option>
    </select>
    <button onclick="refreshData()">🔄 Refresh Data</button>
    <button onclick="toggleView()">🗺️ Toggle View</button>
</div>

<div class="map-container">
    <h2>📍 Global Operations Map</h2>
    <div id="map"></div>
</div>

<div class="map-container">
    <h2>🌐 3D Territory Visualization</h2>
    <div id="globe"></div>
</div>

<div class="stats-grid">
    <div class="stat-card">
        <h3>Top Locations</h3>
        <ul class="stat-list">
            <li><span>Dubai</span><span>82 staff</span></li>
            <li><span>Abu Dhabi</span><span>58 staff</span></li>
            <li><span>Sharjah</span><span>24 staff</span></li>
            <li><span>Ajman</span><span>12 staff</span></li>
        </ul>
    </div>
    
    <div class="stat-card">
        <h3>Contract Performance</h3>
        <ul class="stat-list">
            <li><span>Gymnation</span><span>95% ⭐</span></li>
            <li><span>Wellfit</span><span>92% ⭐</span></li>
            <li><span>Mercedes</span><span>98% ⭐</span></li>
            <li><span>Emirates</span><span>89% ⭐</span></li>
        </ul>
    </div>
</div>
```

</div>

<!-- Scripts -->

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

<script>
// ========== GLOBAL STATE ==========
let currentRole = 'viewer';
let map, markers = [];
let locationData = [
    {name: "Dubai - HQ", lat: 25.2048, lng: 55.2708, staff: 82, contract: "Gymnation"},
    {name: "Abu Dhabi", lat: 24.4539, lng: 54.3773, staff: 58, contract: "Wellfit"},
    {name: "Sharjah", lat: 25.3463, lng: 55.4209, staff: 24, contract: "Mercedes"},
    {name: "Ajman", lat: 25.4052, lng: 55.5136, staff: 12, contract: "Emirates"},
    {name: "Al Ain", lat: 24.2075, lng: 55.7447, staff: 8, contract: "Gymnation"}
];

// ========== LOGIN FUNCTION ==========
function login(){
    const pw = document.getElementById("password").value;
    const role = document.getElementById("roleSelect").value;
    
    // In production, this should be server-side authentication
    if(pw === "elite2026"){
        currentRole = role;
        document.getElementById("loginScreen").style.display="none";
        document.getElementById("dashboard").style.display="block";
        applyRole(role);
        initializeDashboard();
        document.getElementById("errorMessage").style.display = "none";
    } else {
        const errorMsg = document.getElementById("errorMessage");
        errorMsg.style.display = "block";
        setTimeout(() => {
            errorMsg.style.display = "none";
        }, 3000);
    }
}

function logout(){
    document.getElementById("dashboard").style.display="none";
    document.getElementById("loginScreen").style.display="flex";
    document.getElementById("password").value = "";
}

function applyRole(role){
    const userRoleElement = document.getElementById("userRole");
    if(role === "viewer"){
        userRoleElement.textContent = "👤 Viewer Access";
        console.log("Viewer role: restricted access");
    } else {
        userRoleElement.textContent = "👑 Administrator";
        console.log("Admin role: full access");
    }
}

// ========== DASHBOARD INITIALIZATION ==========
function initializeDashboard(){
    initMap();
    initGlobe();
    updateKPIs();
}

// ========== LEAFLET MAP ==========
function initMap(){
    map = L.map('map').setView([25.2048, 55.2708], 8);
    
    // Dark theme tile layer
    L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
        attribution: '© OpenStreetMap © CartoDB',
        maxZoom: 19
    }).addTo(map);
    
    // Add location markers
    updateMarkers();
    
    // Add flight paths
    drawFlightPaths();
}

function updateMarkers(filter = 'all'){
    // Clear existing markers
    markers.forEach(marker => map.removeLayer(marker));
    markers = [];
    
    // Filter locations
    const filteredData = filter === 'all' 
        ? locationData 
        : locationData.filter(loc => loc.contract === filter);
    
    // Add new markers
    filteredData.forEach(location => {
        const marker = L.circleMarker([location.lat, location.lng], {
            radius: Math.max(8, location.staff / 5),
            color: "#00ffff",
            fillColor: "#00bfff",
            fillOpacity: 0.7,
            weight: 2
        }).addTo(map);
        
        marker.bindPopup(`
            <div style="color:#000; font-weight:bold;">
                <h3 style="margin:0 0 5px 0;">${location.name}</h3>
                <p style="margin:2px 0;">👥 Staff: ${location.staff}</p>
                <p style="margin:2px 0;">📋 Contract: ${location.contract}</p>
            </div>
        `);
        
        markers.push(marker);
    });
}

function drawFlightPaths(){
    // Draw curved paths between locations
    for(let i = 0; i < locationData.length - 1; i++){
        const start = [locationData[i].lat, locationData[i].lng];
        const end = [locationData[i+1].lat, locationData[i+1].lng];
        
        L.polyline([start, end], {
            color: '#00ffff',
            weight: 2,
            opacity: 0.4,
            dashArray: '10, 10'
        }).addTo(map);
    }
}

// ========== THREE.JS 3D GLOBE ==========
let scene, camera, renderer, earth;

function initGlobe(){
    scene = new THREE.Scene();
    camera = new THREE.PerspectiveCamera(60, 1, 0.1, 1000);
    renderer = new THREE.WebGLRenderer({antialias: true, alpha: true});
    
    const container = document.getElementById("globe");
    const width = container.clientWidth;
    const height = 500;
    
    renderer.setSize(width, height);
    container.appendChild(renderer.domElement);
    
    // Earth sphere with texture-like appearance
    const geometry = new THREE.SphereGeometry(2, 64, 64);
    
    // Create a material that looks like Earth
    const material = new THREE.MeshPhongMaterial({
        color: 0x2233aa,
        emissive: 0x112244,
        shininess: 5,
        wireframe: false
    });
    
    earth = new THREE.Mesh(geometry, material);
    scene.add(earth);
    
    // Add wireframe overlay
    const wireframeGeo = new THREE.SphereGeometry(2.01, 32, 32);
    const wireframeMat = new THREE.MeshBasicMaterial({
        color: 0x00ffff,
        wireframe: true,
        transparent: true,
        opacity: 0.1
    });
    const wireframe = new THREE.Mesh(wireframeGeo, wireframeMat);
    scene.add(wireframe);
    
    // Add location markers on globe
    locationData.forEach(loc => {
        addGlobeMarker(loc.lat, loc.lng);
    });
    
    // Atmospheric glow
    const glowGeometry = new THREE.SphereGeometry(2.3, 32, 32);
    const glowMaterial = new THREE.MeshBasicMaterial({
        color: 0x00bfff,
        transparent: true,
        opacity: 0.1,
        side: THREE.BackSide
    });
    const glow = new THREE.Mesh(glowGeometry, glowMaterial);
    scene.add(glow);
    
    // Lighting
    const ambientLight = new THREE.AmbientLight(0x404040, 2);
    scene.add(ambientLight);
    
    const pointLight = new THREE.PointLight(0xffffff, 1);
    pointLight.position.set(5, 3, 5);
    scene.add(pointLight);
    
    camera.position.z = 5;
    
    animate();
    
    // Handle window resize
    window.addEventListener('resize', () => {
        const newWidth = container.clientWidth;
        renderer.setSize(newWidth, height);
        camera.aspect = newWidth / height;
        camera.updateProjectionMatrix();
    });
}

function addGlobeMarker(lat, lng){
    // Convert lat/lng to 3D coordinates
    const phi = (90 - lat) * (Math.PI / 180);
    const theta = (lng + 180) * (Math.PI / 180);
    
    const x = -2.05 * Math.sin(phi) * Math.cos(theta);
    const y = 2.05 * Math.cos(phi);
    const z = 2.05 * Math.sin(phi) * Math.sin(theta);
    
    const markerGeometry = new THREE.SphereGeometry(0.05, 16, 16);
    const markerMaterial = new THREE.MeshBasicMaterial({
        color: 0x00ffff,
        emissive: 0x00ffff,
        emissiveIntensity: 0.5
    });
    const marker = new THREE.Mesh(markerGeometry, markerMaterial);
    marker.position.set(x, y, z);
    scene.add(marker);
}

function animate(){
    requestAnimationFrame(animate);
    earth.rotation.y += 0.001;
    renderer.render(scene, camera);
}

// ========== KPI UPDATES ==========
function updateKPIs(){
    const workforce = 176 + Math.floor(Math.random() * 10 - 5);
    document.getElementById("totalWorkforce").textContent = workforce;
    
    const revenue = (2.4 + Math.random() * 0.3 - 0.15).toFixed(1);
    document.getElementById("revenue").textContent = `$${revenue}M`;
}

function refreshData(){
    updateKPIs();
    updateMarkers();
    
    // Show loading feedback
    const button = event.target;
    const originalText = button.textContent;
    button.innerHTML = '<span class="loading"></span> Refreshing...';
    button.disabled = true;
    
    setTimeout(() => {
        button.textContent = originalText;
        button.disabled = false;
    }, 1000);
}

// ========== FILTERS ==========
function filterContract(){
    const filter = document.getElementById("contractFilter").value;
    updateMarkers(filter);
}

let currentView = 'map';
function toggleView(){
    const mapContainer = document.getElementById("map").parentElement;
    const globeContainer = document.getElementById("globe").parentElement;
    
    if(currentView === 'map'){
        mapContainer.style.display = 'none';
        globeContainer.style.display = 'block';
        currentView = 'globe';
    } else {
        mapContainer.style.display = 'block';
        globeContainer.style.display = 'none';
        currentView = 'map';
    }
}

// ========== AUTO-REFRESH KPIs ==========
setInterval(updateKPIs, 60000); // Every minute

// Allow Enter key to login
document.getElementById("password").addEventListener("keypress", function(event){
    if(event.key === "Enter"){
        login();
    }
});

</script>

</body>
</html>
