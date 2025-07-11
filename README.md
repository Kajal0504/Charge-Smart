<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChargeSmart - EV Optimization Platform</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
   
   
   <style>
        :root {
            --primary: #2d6cdf;
            --secondary: #26c669;
            --bg: #f4fafd;
            --card: #fff;
            --text: #222;
        }
        body {
            margin: 0;
            font-family: 'Segoe UI', Arial, sans-serif;
            background: var(--bg);
            color: var(--text);
        }
        .container {
            max-width: 440px;
            margin: 0 auto;
            padding: 0 8px;
        }
        header {
            text-align: center;
            padding: 36px 0 16px 0;
        }
        .brand {
            font-size: 2.3rem;
            font-weight: 800;
            color: var(--primary);
            letter-spacing: 1.5px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        .brand i {
            color: var(--secondary);
            font-size: 2.1rem;
        }
        .nav {
            display: flex;
            justify-content: center;
            gap: 16px;
            margin-bottom: 22px;
            background: #eaf6ff;
            border-radius: 8px;
            padding: 6px 0;
            box-shadow: 0 1px 6px rgba(44,108,223,0.04);
        }
        .nav button {
            background: none;
            border: none;
            color: var(--primary);
            font-size: 1.13rem;
            cursor: pointer;
            padding: 7px 16px;
            border-radius: 6px;
            transition: background 0.2s, color 0.2s;
            font-weight: 500;
        }
        .nav button.active, .nav button:hover {
            background: var(--primary);
            color: #fff;
        }
        .card {
            background: var(--card);
            border-radius: 18px;
            box-shadow: 0 4px 24px rgba(44,108,223,0.10);
            padding: 30px 20px 22px 20px;
            margin-bottom: 28px;
            transition: box-shadow 0.2s;
        }
        .btn-main {
            background: linear-gradient(90deg, var(--secondary), #27ae60);
            color: #fff;
            border: none;
            border-radius: 8px;
            padding: 15px 0;
            width: 100%;
            font-size: 1.13rem;
            font-weight: 700;
            margin-top: 20px;
            cursor: pointer;
            box-shadow: 0 2px 8px rgba(44,108,223,0.07);
            letter-spacing: 0.5px;
            transition: background 0.2s, box-shadow 0.2s;
        }
        .btn-main:hover {
            background: linear-gradient(90deg, #27ae60, var(--secondary));
            box-shadow: 0 4px 16px rgba(44,108,223,0.13);
        }
        .station-list {
            display: flex;
            flex-direction: column;
            gap: 16px;
        }
        .station-card {
            background: linear-gradient(90deg, #eaf6ff 80%, #f4fafd 100%);
            border-radius: 12px;
            padding: 16px 14px;
            display: flex;
            flex-direction: column;
            gap: 7px;
            box-shadow: 0 2px 10px rgba(44,108,223,0.06);
            cursor: pointer;
            border: 2px solid transparent;
            transition: border 0.2s, box-shadow 0.2s;
        }
        .station-card.selected, .station-card:hover {
            border: 2px solid var(--primary);
            box-shadow: 0 4px 18px rgba(44,108,223,0.13);
        }
        .station-title {
            font-weight: 700;
            color: var(--primary);
            font-size: 1.13rem;
            letter-spacing: 0.2px;
        }
        .station-meta {
            font-size: 1.01rem;
            color: #333;
        }
        .map-view {
            width: 100%;
            height: 320px;
            border-radius: 16px;
            margin-bottom: 20px;
            box-shadow: 0 2px 12px rgba(44,108,223,0.08);
        }
        .route-list {
            display: flex;
            flex-direction: column;
            gap: 14px;
        }
        .route-card {
            background: linear-gradient(90deg, #f6fff6 80%, #eaffea 100%);
            border-radius: 12px;
            padding: 14px 12px;
            border: 2px solid transparent;
            transition: border 0.2s, box-shadow 0.2s;
            box-shadow: 0 1px 8px rgba(44,108,223,0.06);
        }
        .route-card.recommended {
            border: 2px solid var(--secondary);
            background: linear-gradient(90deg, #eaffea 80%, #f6fff6 100%);
            box-shadow: 0 4px 18px rgba(46,204,113,0.13);
        }
        .chart-bar {
            display: flex;
            align-items: flex-end;
            gap: 2px;
            height: 60px;
            margin-top: 10px;
        }
        .bar {
            width: 8px;
            background: var(--primary);
            border-radius: 2px 2px 0 0;
            transition: background 0.2s;
        }
        .bar.busy {
            background: #e67e22;
        }
        .bar.full {
            background: #e74c3c;
        }
        @media (max-width: 600px) {
            .container { max-width: 100vw; padding: 0 2vw; }
            .map-view { height: 220px; }
            .card { padding: 18px 4vw 14px 4vw; }
            .brand { font-size: 1.5rem; }
        }
    </style>
</head>
<body>
    <!-- Splash/Login/Signup Page -->
    <div class="container" id="auth-container" style="display:block;">
        <div class="card" style="text-align:center;margin-top:60px;">
            <div class="brand" style="font-size:2.5rem;margin-bottom:18px;"><i class="fa-solid fa-bolt"></i> ChargeSmart</div>
            <h2>Welcome to ChargeSmart</h2>
            <div id="auth-tabs" style="margin-bottom:18px;">
                <button id="show-login" class="btn-main" style="width:45%;margin-right:5px;">Login</button>
                <button id="show-signup" class="btn-main" style="width:45%;">Sign Up</button>
            </div>
            <form id="login-form" style="margin-top:10px;">
                <input type="text" id="login-username" placeholder="Username" required style="margin-bottom:12px;width:90%;padding:10px;font-size:1.1rem;"><br>
                <input type="password" id="login-password" placeholder="Password" required style="margin-bottom:18px;width:90%;padding:10px;font-size:1.1rem;"><br>
                <button type="submit" class="btn-main" style="width:100%;">Login</button>
            </form>
            <form id="signup-form" style="margin-top:10px; display:none;">
                <input type="text" id="signup-username" placeholder="Choose Username" required style="margin-bottom:12px;width:90%;padding:10px;font-size:1.1rem;"><br>
                <input type="password" id="signup-password" placeholder="Choose Password" required style="margin-bottom:18px;width:90%;padding:10px;font-size:1.1rem;"><br>
                <button type="submit" class="btn-main" style="width:100%;">Sign Up</button>
            </form>
            <div id="auth-error" style="color:#e74c3c;margin-top:10px;font-weight:600;"></div>
        </div>
    </div>
    <!-- Main App -->
    <div class="container" id="main-app" style="display:none;">
        <header>
            <div class="brand"><i class="fa-solid fa-bolt"></i> ChargeSmart</div>
        </header>
        <nav class="nav">
            <button id="nav-home" class="active">Home</button>
            <button id="nav-map">Map</button>
            <button id="nav-route">Smart Route</button>
            <button id="nav-profile">Profile</button>
        </nav>
        <main id="main-content"></main>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
    // Dummy data
    const stations = [
        { id: 1, name: 'EV FastCharge - Connaught Place', lat: 28.6315, lng: 77.2167, available: 3, total: 5, wait: 10, predict: 2, predictWait: 8, congestion: [2,3,4,5,4,3,2] },
        { id: 2, name: 'ChargeGrid - Khan Market', lat: 28.6006, lng: 77.2270, available: 1, total: 4, wait: 18, predict: 2, predictWait: 12, congestion: [3,4,5,5,4,3,2] },
        { id: 3, name: 'PlugNgo - Lajpat Nagar', lat: 28.5708, lng: 77.2436, available: 4, total: 5, wait: 3, predict: 3, predictWait: 2, congestion: [1,2,2,3,2,1,1] },
        { id: 4, name: 'Tata Power EZ - Karol Bagh', lat: 28.6512, lng: 77.1906, available: 0, total: 3, wait: 25, predict: 1, predictWait: 20, congestion: [3,3,3,2,2,1,0] }
    ];
    let selectedStation = null;
    let currentPage = 'home';

    // Simple localStorage-based user system
    function getUser() {
        return JSON.parse(localStorage.getItem('chargesmart_user') || 'null');
    }
    function setUser(user) {
        localStorage.setItem('chargesmart_user', JSON.stringify(user));
    }
    function clearUser() {
        localStorage.removeItem('chargesmart_user');
    }

    // Auth page logic
    const loginForm = document.getElementById('login-form');
    const signupForm = document.getElementById('signup-form');
    const authError = document.getElementById('auth-error');
    document.getElementById('show-login').onclick = function() {
        loginForm.style.display = '';
        signupForm.style.display = 'none';
        authError.innerText = '';
    };
    document.getElementById('show-signup').onclick = function() {
        loginForm.style.display = 'none';
        signupForm.style.display = '';
        authError.innerText = '';
    };

    // Login
    loginForm.onsubmit = function(e) {
        e.preventDefault();
        const username = document.getElementById('login-username').value.trim();
        const password = document.getElementById('login-password').value;
        const user = getUser();
        if (user && user.username === username && user.password === password) {
            document.getElementById('auth-container').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            showPage('home');
        } else {
            authError.innerText = "Invalid username or password!";
        }
    };

    // Signup
    signupForm.onsubmit = function(e) {
        e.preventDefault();
        const username = document.getElementById('signup-username').value.trim();
        const password = document.getElementById('signup-password').value;
        if (!username || !password) {
            authError.innerText = "Please enter username and password.";
            return;
        }
        setUser({ username, password });
        authError.style.color = "#2ecc71";
        authError.innerText = "Signup successful! Please login.";
        setTimeout(() => {
            authError.style.color = "#e74c3c";
            document.getElementById('show-login').click();
        }, 1200);
    };

    // Auto-login if already registered
    window.onload = function() {
        if (getUser()) {
            document.getElementById('auth-container').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            showPage('home');
        }
    };

    // Routing
    function showPage(page) {
        currentPage = page;
        document.querySelectorAll('.nav button').forEach(btn => btn.classList.remove('active'));
        document.getElementById('nav-' + page).classList.add('active');
        if (page === 'home') renderHome();
        if (page === 'map') renderMap();
        if (page === 'route') renderRoute();
        if (page === 'profile') renderProfile();
    }
    document.getElementById('nav-home').onclick = () => showPage('home');
    document.getElementById('nav-map').onclick = () => showPage('map');
    document.getElementById('nav-route').onclick = () => showPage('route');
    document.getElementById('nav-profile').onclick = () => showPage('profile');

    // Home Page (Find Charging Station)
    function renderHome() {
        document.getElementById('main-content').innerHTML = `
            <div class="card" style="text-align:center;">
                <h2>Welcome to ChargeSmart</h2>
                <p>Optimize your EV charging experience.<br>Find stations, get smart routes, and more!</p>
                <button class="btn-main" onclick="showPage('map')">Find Charging Station</button>
            </div>
        `;
    }

   
    function renderMap() {
    // Ask for city and state before showing stations
    document.getElementById('main-content').innerHTML = `
        <div class="card" style="text-align:center;">
            <h3>Enter Your Location</h3>
            <form id="location-form" style="margin-top:10px;">
                <input type="text" id="user-city" placeholder="Enter City" required style="margin-bottom:8px;width:90%;padding:6px;"><br>
                <input type="text" id="user-state" placeholder="Enter State" required style="margin-bottom:8px;width:90%;padding:6px;"><br>
                <button type="submit" class="btn-main" style="width:100%;">Show Stations</button>
            </form>
            <div id="station-list-area"></div>
        </div>
    `;

    document.getElementById('location-form').onsubmit = function(e) {
        e.preventDefault();
        const city = document.getElementById('user-city').value;
        const state = document.getElementById('user-state').value;
        // Sort stations by wait time (ascending)
        const sortedStations = [...stations].sort((a, b) => a.wait - b.wait);
        document.getElementById('station-list-area').innerHTML = `
            <div id="map" class="map-view"></div>
            <div class="station-list">
                ${sortedStations.map(st => `
                    <div class="station-card" onclick="selectStationWithLocation(${st.id}, '${city}', '${state}')">
                        <div class="station-title">${st.name}</div>
                        <div class="station-meta">Live: <b>${st.available}/${st.total}</b> slots &bull; Wait: <b>${st.wait} min</b></div>
                        <div class="station-meta">Predicted (15 min): <b>${st.predict}/${st.total}</b> slots &bull; Wait: <b>${st.predictWait} min</b></div>
                    </div>
                `).join('')}
            </div>
        `;
        setTimeout(() => initMap(sortedStations), 100);
    };
}

    // Smart Route Page
    function renderRoute() {
        // Dummy: 3 routes, pick best by ETA+wait
        const userLoc = { lat: 28.6129, lng: 77.2295 };
        const routes = stations.map(st => {
            const dist = getDistance(userLoc.lat, userLoc.lng, st.lat, st.lng);
            const eta = Math.round(dist / 0.5 * 60); // 30km/h
            const total = eta + st.wait;
            return { ...st, dist: dist.toFixed(2), eta, total };
        });
        routes.sort((a,b) => a.total - b.total);
        document.getElementById('main-content').innerHTML = `
            <div class="card">
                <h3>Smart Route Comparison</h3>
                <div class="route-list">
                    ${routes.map((r,i) => `
                        <div class="route-card${i===0?' recommended':''}">
                            <div><b>${r.name}</b> ${i===0?'<span style=\'color:var(--secondary);font-weight:600\'>(Recommended)</span>':''}</div>
                            <div>Distance: <b>${r.dist} km</b> &bull; ETA: <b>${r.eta} min</b></div>
                            <div>Predicted Wait: <b>${r.wait} min</b></div>
                            <div>Total Time: <b>${r.total} min</b></div>
                        </div>
                    `).join('')}
                </div>
            </div>
        `;
    }

    // Replace your selectStation function with this version:

function selectStationWithLocation(id, city, state) {
    const st = stations.find(s => s.id === id);
    selectedStation = st;
    document.getElementById('main-content').innerHTML = `
        <div class="card">
            <button onclick="showPage('map')" style="float:right;background:none;border:none;font-size:1.2rem;color:var(--primary);cursor:pointer"><i class='fa fa-arrow-left'></i></button>
            <h3>${st.name}</h3>
            <div>Live: <b>${st.available}/${st.total}</b> slots</div>
            <div>Predicted (15 min): <b>${st.predict}/${st.total}</b> slots</div>
            <div>Wait: <b>${st.wait} min</b> (Predicted: <b>${st.predictWait} min</b>)</div>
            <div style="margin-top:18px;">
                <canvas id="congestionChart" width="320" height="80"></canvas>
            </div>
            <div style="margin-top:22px;">
                <button class="btn-main" id="book-slot-btn" ${st.available === 0 ? 'disabled style="background:#ccc;cursor:not-allowed;"' : ''}>
                    ${st.available === 0 ? 'No Slots Available' : 'Book Slot for ' + city + ', ' + state}
                </button>
                <div id="book-slot-msg" style="margin-top:10px;color:var(--secondary);font-weight:600;"></div>
            </div>
        </div>
    `;
    setTimeout(() => renderChart(st.congestion), 100);

    setTimeout(() => {
        const btn = document.getElementById('book-slot-btn');
        if (btn && st.available > 0) {
            btn.onclick = function() {
                btn.disabled = true;
                btn.innerText = "Slot Booked!";
                document.getElementById('book-slot-msg').innerHTML = `
                    <div>
                        <span style="color:var(--primary);font-weight:600;">Booking Confirmed!</span><br>
                        City: ${city}<br>
                        State: ${state}<br>
                        <button id="confirm-booking-btn" class="btn-main" style="margin-top:10px;width:100%;">Confirm</button>
                    </div>
                `;
                document.getElementById('confirm-booking-btn').onclick = function() {
                    document.getElementById('book-slot-msg').innerHTML = `<span style="color:var(--primary);font-weight:600;">Thank you. Your slot is confirmed for ${city}, ${state}!</span>`;
                };
            };
        }
    }, 200);
}
    
    // Profile/Feedback Page
    function renderProfile() {
    document.getElementById('main-content').innerHTML = `
        <div class="card">
            <h3>Profile / Feedback</h3>
            <p>Feature coming soon!<br>
            Send feedback to <a href='mailto:feedback@chargesmart.com'>feedback@chargesmart.com</a></p>
            <p>
                Or fill out our <a href="https://forms.gle/ydAsWsr4xQK3X6tt6" target="_blank" style="color:var(--primary);font-weight:600;">Google Feedback Form</a>
            </p>
        </div>
    `;
}

    // Map logic (Leaflet for open/free, or Google Maps if key provided)
    function initMap() {
        if (window.L) {
            if (window._map) window._map.remove();
            window._map = L.map('map').setView([28.6129, 77.2295], 12);
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '© OpenStreetMap contributors'
            }).addTo(window._map);
            stations.forEach(st => {
                const marker = L.marker([st.lat, st.lng]).addTo(window._map);
                marker.on('click', () => selectStation(st.id));
                marker.bindTooltip(st.name);
            });
        } else if (window.google && window.google.maps) {
            if (window._gmap) window._gmap = null;
            const map = new google.maps.Map(document.getElementById('map'), {
                center: { lat: 28.6129, lng: 77.2295 },
                zoom: 12
            });
            stations.forEach(st => {
                const marker = new google.maps.Marker({
                    position: { lat: st.lat, lng: st.lng },
                    map,
                    title: st.name
                });
                marker.addListener('click', () => selectStation(st.id));
            });
        } else {
            document.getElementById('map').innerHTML = '<div style="color:#e74c3c;text-align:center;padding:30px 0">Map failed to load.<br>Check your internet or API key.</div>';
        }
    }

    // Chart logic
    function renderChart(data) {
        const ctx = document.getElementById('congestionChart').getContext('2d');
        new Chart(ctx, {
            type: 'bar',
            data: {
                labels: ['Now','+10m','+20m','+30m','+40m','+50m','+60m'],
                datasets: [{
                    label: 'Busy Slots',
                    data: data,
                    backgroundColor: data.map(v => v>=5?'#e74c3c':v>=3?'#e67e22':'#2d6cdf')
                }]
            },
            options: {
                plugins: { legend: { display: false } },
                scales: { y: { beginAtZero: true, max: 6 } }
            }
        });
    }

    // Utility: Haversine distance in km
    function getDistance(lat1, lon1, lat2, lon2) {
        function toRad(x) { return x * Math.PI / 180; }
        const R = 6371;
        const dLat = toRad(lat2 - lat1);
        const dLon = toRad(lon2 - lon1);
        const a = Math.sin(dLat/2) * Math.sin(dLat/2) +
            Math.cos(toRad(lat1)) * Math.cos(toRad(lat2)) *
            Math.sin(dLon/2) * Math.sin(dLon/2);
        const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
        return R * c;
    }

    // Try to load Leaflet (open source map)
    (function(){
        var script = document.createElement('script');
        script.src = 'https://unpkg.com/leaflet@1.9.4/dist/leaflet.js';
        script.onload = () => showPage('home');
        document.body.appendChild(script);
        var link = document.createElement('link');
        link.rel = 'stylesheet';
        link.href = 'https://unpkg.com/leaflet@1.9.4/dist/leaflet.css';
        document.head.appendChild(link);
    })();
    </script>
</body>
</html>
