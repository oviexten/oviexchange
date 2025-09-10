<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Ovie Exchange</title>
  <style>
    * {margin: 0; padding: 0; box-sizing: border-box;}
    body { font-family: Arial, sans-serif; background: #f5f5f5; color: #333; }
    header {
      background: #111; color: #fff; padding: 1rem 2rem;
      display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap;
    }
    header .logo { font-size: 1.5rem; font-weight: bold; color: #00d4ff; }
    nav { display: flex; gap: 1rem; flex-wrap: wrap; }
    nav a { color: #fff; text-decoration: none; padding: 0.5rem 1rem; border-radius: 5px; }
    nav a:hover, nav a.active { background: #00d4ff; color: #000; }
    main { padding: 2rem; max-width: 1200px; margin: auto; }
    section { display: none; }
    section.active { display: block; }

    /* Forms */
    .form-container {
      background: #fff; padding: 2rem; border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1); max-width: 400px; margin: auto;
    }
    .form-container h2 { margin-bottom: 1rem; color: #00d4ff; text-align: center; }
    .form-container input {
      width: 100%; padding: 0.8rem; margin-bottom: 1rem;
      border: 1px solid #ccc; border-radius: 5px;
    }
    .form-container button {
      width: 100%; padding: 0.8rem; border: none;
      border-radius: 5px; background: #00d4ff; font-weight: bold; cursor: pointer;
    }
    .form-container button:hover { background: #00aacc; }

    /* Dashboard cards */
    .dashboard { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 1.5rem; }
    .card {
      background: #fff; padding: 1.5rem; border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1); text-align: center;
    }
    .card h3 { margin-bottom: 1rem; color: #00d4ff; }
    .balance { font-size: 1.5rem; font-weight: bold; }

    /* Trading */
    .trade-container { display: grid; grid-template-columns: 2fr 1fr; gap: 2rem; }
    .trade-panel, .chart-box {
      background: #fff; padding: 1.5rem; border-radius: 10px; box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
    .trading-form { display: flex; flex-direction: column; gap: 1rem; }
    .trading-form input, .trading-form select {
      padding: 0.7rem; border: 1px solid #ccc; border-radius: 5px;
    }
    .btn { padding: 0.8rem; border: none; border-radius: 5px; font-weight: bold; cursor: pointer; }
    .btn-buy { background: #00ff88; }
    .btn-sell { background: #ff4d4d; color: #fff; }

    /* Tables */
    table { width: 100%; border-collapse: collapse; background: #fff; box-shadow: 0 4px 10px rgba(0,0,0,0.1); border-radius: 10px; overflow: hidden; }
    th, td { padding: 1rem; border-bottom: 1px solid #ddd; text-align: left; }
    th { background: #00d4ff; color: #000; }

    /* About & Contact */
    .about, .contact { background: #fff; padding: 2rem; border-radius: 10px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
    .contact input, .contact textarea {
      width: 100%; padding: 0.8rem; margin-bottom: 1rem; border: 1px solid #ccc; border-radius: 5px;
    }
    .contact button {
      background: #00d4ff; padding: 0.8rem 1rem; border: none; border-radius: 5px; cursor: pointer; font-weight: bold;
    }

    @media (max-width: 900px) { .trade-container { grid-template-columns: 1fr; } nav { flex-direction: column; } }
  </style>
</head>
<body>
  <header>
    <div class="logo">Ovie Exchange</div>
    <nav>
      <a href="#" class="active" onclick="showSection('login')">Login</a>
      <a href="#" onclick="showSection('signup')">Sign Up</a>
      <a href="#" onclick="showSection('dashboard')">Dashboard</a>
      <a href="#" onclick="showSection('markets')">Markets</a>
      <a href="#" onclick="showSection('trading')">Trading</a>
      <a href="#" onclick="showSection('portfolio')">Portfolio</a>
      <a href="#" onclick="showSection('history')">History</a>
      <a href="#" onclick="showSection('about')">About</a>
      <a href="#" onclick="showSection('contact')">Contact</a>
    </nav>
  </header>

  <main>
    <!-- Login -->
    <section id="login" class="active">
      <div class="form-container">
        <h2>Login</h2>
        <input type="email" id="loginEmail" placeholder="Email" required>
        <input type="password" id="loginPassword" placeholder="Password" required>
        <button onclick="loginUser()">Login</button>
      </div>
    </section>

    <!-- Sign Up -->
    <section id="signup">
      <div class="form-container">
        <h2>Sign Up</h2>
        <input type="text" id="signupName" placeholder="Full Name" required>
        <input type="email" id="signupEmail" placeholder="Email" required>
        <input type="password" id="signupPassword" placeholder="Password" required>
        <button onclick="registerUser()">Create Account</button>
      </div>
    </section>

    <!-- Dashboard -->
    <section id="dashboard">
      <h2>Dashboard</h2>
      <div class="dashboard">
        <div class="card"><h3>Total Balance</h3><p class="balance" id="totalBalance">$0</p></div>
        <div class="card"><h3>BTC Price</h3><p class="balance" id="btcPrice">$0</p></div>
        <div class="card"><h3>ETH Price</h3><p class="balance" id="ethPrice">$0</p></div>
      </div>
    </section>

    <!-- Markets -->
    <section id="markets">
      <h2>Markets</h2>
      <table>
        <thead><tr><th>Coin</th><th>Price (USD)</th></tr></thead>
        <tbody id="marketTable"></tbody>
      </table>
    </section>

    <!-- Trading -->
    <section id="trading">
      <h2>Trading</h2>
      <div class="trade-container">
        <div class="chart-box">
          <h3>BTC/USD Live Chart</h3>
          <canvas id="priceChart"></canvas>
        </div>
        <div class="trade-panel">
          <h3>Quick Trade</h3>
          <form class="trading-form" onsubmit="placeTrade(event)">
            <select id="crypto">
              <option value="bitcoin">Bitcoin (BTC)</option>
              <option value="ethereum">Ethereum (ETH)</option>
              <option value="cardano">Cardano (ADA)</option>
              <option value="solana">Solana (SOL)</option>
            </select>
            <input type="number" id="amount" placeholder="Amount in USD" required>
            <select id="type">
              <option value="buy">Buy</option>
              <option value="sell">Sell</option>
            </select>
            <button type="submit" class="btn btn-buy">Execute Trade</button>
          </form>
        </div>
      </div>
    </section>

    <!-- Portfolio -->
    <section id="portfolio">
      <h2>Portfolio</h2>
      <table>
        <thead><tr><th>Asset</th><th>Quantity</th><th>Value (USD)</th></tr></thead>
        <tbody>
          <tr><td>Bitcoin</td><td>1.25 BTC</td><td id="btcHoldings">$0</td></tr>
          <tr><td>Ethereum</td><td>10 ETH</td><td id="ethHoldings">$0</td></tr>
        </tbody>
      </table>
    </section>

    <!-- History -->
    <section id="history">
      <h2>Trade History</h2>
      <table>
        <thead><tr><th>Date</th><th>Asset</th><th>Type</th><th>Amount</th><th>Status</th></tr></thead>
        <tbody id="historyTable">
          <tr><td>2025-09-09</td><td>BTC</td><td>Buy</td><td>$2000</td><td>Completed</td></tr>
        </tbody>
      </table>
    </section>

    <!-- About -->
    <section id="about">
      <div class="about">
        <h2>About Ovie Exchange</h2>
        <p>Ovie Exchange is a demo crypto trading platform designed to showcase live markets, portfolio tracking, and trading simulation. Built for learning and project purposes.</p>
      </div>
    </section>

    <!-- Contact -->
    <section id="contact">
      <div class="contact">
        <h2>Contact Us</h2>
        <form onsubmit="sendMessage(event)">
          <input type="text" id="name" placeholder="Your Name" required>
          <input type="email" id="email" placeholder="Your Email" required>
          <textarea id="message" placeholder="Message" rows="5" required></textarea>
          <button type="submit">Send</button>
        </form>
      </div>
    </section>
  </main>

  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script>
    // Navigation
    function showSection(id) {
      document.querySelectorAll("section").forEach(s => s.classList.remove("active"));
      document.querySelectorAll("nav a").forEach(a => a.classList.remove("active"));
      document.getElementById(id).classList.add("active");
      event.target.classList.add("active");
    }

    // Chart.js setup
    const ctx = document.getElementById("priceChart").getContext("2d");
    const chartData = { labels: [], datasets: [{ label: "BTC/USD", data: [], borderColor: "#00d4ff", backgroundColor: "rgba(0,212,255,0.1)", fill: true, tension: 0.3 }] };
    const btcChart = new Chart(ctx, { type: "line", data: chartData });

    // Fetch live prices
    async function fetchPrices() {
      const res = await fetch("https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,cardano,solana&vs_currencies=usd");
      const data = await res.json();

      // Update Dashboard
      document.getElementById("btcPrice").textContent = `$${data.bitcoin.usd.toLocaleString()}`;
      document.getElementById("ethPrice").textContent = `$${data.ethereum.usd.toLocaleString()}`;

      // Update Portfolio
      document.getElementById("btcHoldings").textContent = `$${(1.25 * data.bitcoin.usd).toLocaleString()}`;
      document.getElementById("ethHoldings").textContent = `$${(10 * data.ethereum.usd).toLocaleString()}`;

      // Update Total Balance
      const total = (1.25 * data.bitcoin.usd) + (10 * data.ethereum.usd);
      document.getElementById("totalBalance").textContent = `$${total.toLocaleString()}`;

      // Update Chart
      const now = new Date().toLocaleTimeString();
      chartData.labels.push(now);
      chartData.datasets[0].data.push(data.bitcoin.usd);
      if (chartData.labels.length > 10) {
        chartData.labels.shift();
        chartData.datasets[0].data.shift();
      }
      btcChart.update();

      // Update Markets Table
      const marketTable = document.getElementById("marketTable");
      marketTable.innerHTML = `
        <tr><td>Bitcoin (BTC)</td><td>$${data.bitcoin.usd.toLocaleString()}</td></tr>
        <tr><td>Ethereum (ETH)</td><td>$${data.ethereum.usd.toLocaleString()}</td></tr>
        <tr><td>Cardano (ADA)</td><td>$${data.cardano.usd.toLocaleString()}</td></tr>
        <tr><td>Solana (SOL)</td><td>$${data.solana.usd.toLocaleString()}</td></tr>
      `;
    }

    setInterval(fetchPrices, 10000);
    fetchPrices();

    // Dummy Trade
    function placeTrade(e) {
      e.preventDefault();
      const crypto = document.getElementById("crypto").value;
      const amount = document.getElementById("amount").value;
      const type = document.getElementById("type").value;
      const date = new Date().toISOString().split("T")[0];
      const row = `<tr><td>${date}</td><td>${crypto}</td><td>${type}</td><td>$${amount}</td><td>Completed</td></tr>`;
      document.getElementById("historyTable").innerHTML += row;
      alert(`${type.toUpperCase()} order placed: $${amount} of ${crypto}`);
    }

    // Contact form
    function sendMessage(e) {
      e.preventDefault();
      alert("Message sent! We will get back to you.");
    }

    // Simple Auth (localStorage)
    function registerUser() {
      const email = document.getElementById("signupEmail").value;
      const pass = document.getElementById("signupPassword").value;
      localStorage.setItem("user", JSON.stringify({ email, pass }));
      alert("Account created! Please log in.");
      showSection("login");
    }

    function loginUser() {
      const email = document.getElementById("loginEmail").value;
      const pass = document.getElementById("loginPassword").value;
      const user = JSON.parse(localStorage.getItem("user"));
      if (user && user.email === email && user.pass === pass) {
        alert("Login successful!");
        showSection("dashboard");
      } else {
        alert("Invalid credentials!");
      }
    }
  </script>
</body>
</html>
