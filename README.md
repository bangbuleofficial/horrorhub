<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>HORROR FEED INDONESIA</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet"/>
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      font-family: 'Inter', sans-serif;
      margin: 0;
      background-color: #111;
      color: white;
    }

    header {
      background-color: #000;
      padding: 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 3px solid red;
    }

    header h1 {
      color: red;
      font-weight: 800;
      margin: 0;
    }

    nav a {
      margin-left: 20px;
      text-decoration: none;
      color: white;
      background-color: #4a0c0c;
      padding: 10px 20px;
      border-radius: 20px;
      font-weight: 600;
      transition: background 0.2s;
    }

    nav a:hover {
      background-color: #a00;
    }

    .container {
      max-width: 1200px;
      margin: auto;
      padding: 30px 20px;
    }

    h2 {
      font-size: 24px;
      border-left: 5px solid red;
      padding-left: 10px;
      margin-bottom: 20px;
    }

    .cards {
      display: flex;
      gap: 20px;
      flex-wrap: wrap;
    }

    .card {
      background: linear-gradient(145deg, #1c1c1c, #0f0f0f);
      padding: 20px;
      border-radius: 12px;
      flex: 1 1 280px;
      min-width: 280px;
      max-width: 360px;
      position: relative;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }

    .card img {
      width: 100%;
      height: 180px;
      object-fit: cover;
      border-radius: 8px;
      margin-bottom: 15px;
    }

    .card h3 {
      margin: 0 0 10px 0;
      font-size: 18px;
    }

    .badge {
      background-color: red;
      color: white;
      padding: 4px 10px;
      border-radius: 10px;
      font-size: 12px;
      margin-right: 10px;
      font-weight: 600;
    }

    .timestamp {
      font-size: 12px;
      color: #ccc;
    }

    .filters {
      margin-top: 40px;
    }

    .filters h3 {
      margin-bottom: 10px;
      font-size: 18px;
    }

    .filters .buttons {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-bottom: 10px;
    }

    .filters button {
      padding: 8px 16px;
      border: none;
      border-radius: 20px;
      font-weight: 600;
      cursor: pointer;
      background-color: #2c2c2c;
      color: white;
      transition: background 0.2s;
    }

    .filters button.active, .filters button:hover {
      background-color: red;
      color: #fff;
    }

    .generate-button {
      background-color: red;
      padding: 15px;
      color: white;
      font-size: 16px;
      font-weight: 600;
      border: none;
      border-radius: 10px;
      margin: 30px 0;
      width: 100%;
      cursor: pointer;
      transition: background 0.2s;
    }

    .generate-button:hover {
      background-color: #a00;
    }

    .downloads {
      display: flex;
      gap: 20px;
      flex-wrap: wrap;
      margin-top: 20px;
    }

    .download-card {
      background-color: #1c1c1c;
      padding: 10px;
      border-radius: 10px;
      text-align: center;
      flex: 1 1 200px;
      min-width: 200px;
      max-width: 300px;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .download-card img {
      width: 100%;
      height: 120px;
      object-fit: cover;
      border-radius: 8px;
      margin-bottom: 10px;
    }

    .download-card a {
      background-color: limegreen;
      color: white;
      padding: 10px 20px;
      border-radius: 8px;
      font-weight: bold;
      text-decoration: none;
      transition: background 0.2s;
      display: inline-block;
    }

    .download-card a:hover {
      background-color: #228B22;
    }

    @media (max-width: 1024px) {
      .cards,
      .downloads {
        flex-direction: column;
        gap: 25px;
      }
      .card, .download-card {
        max-width: 100%;
      }
    }

    @media (max-width: 600px) {
      .container {
        padding: 15px 5px;
      }
      header {
        flex-direction: column;
        align-items: flex-start;
        gap: 10px;
      }
      nav a {
        margin-left: 0;
        margin-right: 10px;
      }
    }
  </style>
</head>

<body>
  <header>
    <h1>HORROR FEED</h1>
    <nav>
      <a href="#">Beranda</a>
      <a href="#">TikTok</a>
    </nav>
  </header>

  <div class="container">
    <h2>🔥 Trending Hari Ini</h2>
    <div class="cards">
      <div class="card">
        <img src="https://via.placeholder.com/300x180" alt="Horror Image">
        <h3>Penampakan Misterius di Kos Jakarta</h3>
        <p>Mahasiswa mengalami kejadian aneh di kos. Suara langkah kaki malam hari dan pintu terbuka sendiri.</p>
        <div>
          <span class="badge">TIKTOK</span><span class="timestamp">5 menit lalu</span>
        </div>
      </div>
      <div class="card">
        <img src="https://via.placeholder.com/300x180" alt="Horror Image">
        <h3>Ritual Santet di Desa Terpencil</h3>
        <p>Dokumenter tentang praktik santet di Jawa Tengah dengan kesaksian korban.</p>
        <div>
          <span class="badge">YOUTUBE</span><span class="timestamp">12 menit lalu</span>
        </div>
      </div>
      <div class="card">
        <img src="https://via.placeholder.com/300x180" alt="Horror Image">
        <h3>Foto Pocong di Kuburan Tengah Kota</h3>
        <p>Foto ziarah kubur menunjukkan sosok putih mencurigakan di belakang makam.</p>
        <div>
          <span class="badge">INSTAGRAM</span><span class="timestamp">18 menit lalu</span>
        </div>
      </div>
    </div>

    <div class="filters">
      <h3>Gaya Horror:</h3>
      <div class="buttons">
        <button class="active" type="button">Realistis</button>
        <button type="button">Gothic</button>
        <button type="button">Dark</button>
        <button type="button">Vintage</button>
      </div>

      <h3 style="margin-top: 20px;">Tema Horror Indonesia:</h3>
      <div class="buttons">
        <button type="button">Pocong</button>
        <button type="button">Kuntilanak</button>
        <button type="button">Tuyul</button>
        <button type="button">Santet</button>
      </div>
    </div>

    <button class="generate-button" type="button">🎨 Generate Gambar Horror</button>

    <div class="downloads">
      <div class="download-card">
        <img src="https://via.placeholder.com/200x120" alt="Generated">
        <a href="#" download>📥 Download</a>
      </div>
      <div class="download-card">
        <img src="https://via.placeholder.com/200x120" alt="Generated">
        <a href="#" download>📥 Download</a>
      </div>
      <div class="download-card">
        <img src="https://via.placeholder.com/200x120" alt="Generated">
        <a href="#" download>📥 Download</a>
      </div>
    </div>
  </div>
</body>
</html>
