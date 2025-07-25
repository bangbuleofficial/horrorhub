<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Horror Feed Indonesia</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #0a0a0a;
            color: #ffffff;
            font-family: 'Inter', sans-serif;
            min-height: 100vh;
            overflow-x: hidden;
        }

        .navbar {
            position: fixed;
            top: 0;
            width: 100%;
            background: rgba(10, 10, 10, 0.95);
            backdrop-filter: blur(20px);
            z-index: 1000;
            padding: 15px 40px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: all 0.3s ease;
        }

        .logo {
            font-size: 2rem;
            font-weight: 800;
            background: linear-gradient(135deg, #ff0000, #cc0000);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .nav-links {
            display: flex;
            gap: 30px;
        }

        .nav-link {
            color: #cccccc;
            text-decoration: none;
            font-weight: 500;
            padding: 8px 16px;
            border-radius: 20px;
            transition: all 0.3s ease;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .nav-link:hover, .nav-link.active {
            color: #ffffff;
            background: rgba(255, 0, 0, 0.2);
        }

        .social-icon {
            width: 20px;
            height: 20px;
        }

        .hero {
            height: 100vh;
            background: linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)), 
                        linear-gradient(45deg, #1a0000, #330000);
            display: flex;
            align-items: center;
            padding: 0 40px;
            position: relative;
        }

        .hero::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: radial-gradient(circle at 20% 50%, rgba(255,0,0,0.1) 0%, transparent 50%),
                        radial-gradient(circle at 80% 20%, rgba(255,0,0,0.1) 0%, transparent 50%);
            pointer-events: none;
        }

        .hero-content {
            max-width: 600px;
            position: relative;
            z-index: 2;
        }

        .hero-title {
            font-size: 4rem;
            font-weight: 800;
            margin-bottom: 20px;
            background: linear-gradient(135deg, #ffffff, #ff6b6b);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .hero-subtitle {
            font-size: 1.3rem;
            color: #e0e0e0;
            margin-bottom: 30px;
            line-height: 1.5;
        }

        .hero-buttons {
            display: flex;
            gap: 20px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #ffffff, #f0f0f0);
            color: #000000;
            padding: 15px 30px;
            border: none;
            border-radius: 8px;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn-primary:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 30px rgba(255,255,255,0.2);
        }

        .btn-secondary {
            background: rgba(255,255,255,0.1);
            color: #ffffff;
            padding: 15px 30px;
            border: 2px solid rgba(255,255,255,0.3);
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn-secondary:hover {
            background: rgba(255,255,255,0.2);
        }

        .content {
            padding: 80px 40px;
            background: #111111;
        }

        .content-row {
            margin-bottom: 60px;
        }

        .section-title {
            font-size: 2rem;
            font-weight: 700;
            margin-bottom: 30px;
            color: #ffffff;
            position: relative;
            padding-left: 20px;
        }

        .section-title::before {
            content: '';
            position: absolute;
            left: 0;
            top: 50%;
            transform: translateY(-50%);
            width: 4px;
            height: 30px;
            background: linear-gradient(135deg, #ff0000, #cc0000);
            border-radius: 2px;
        }

        .carousel {
            display: flex;
            gap: 20px;
            overflow-x: auto;
            padding: 20px 0;
            scroll-behavior: smooth;
        }

        .carousel::-webkit-scrollbar {
            height: 6px;
        }

        .carousel::-webkit-scrollbar-track {
            background: rgba(255,255,255,0.1);
            border-radius: 3px;
        }

        .carousel::-webkit-scrollbar-thumb {
            background: #ff0000;
            border-radius: 3px;
        }

        .horror-card {
            min-width: 300px;
            background: linear-gradient(145deg, #1a1a1a, #0f0f0f);
            border-radius: 12px;
            overflow: hidden;
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
            border: 1px solid rgba(255,255,255,0.1);
        }

        .horror-card:hover {
            transform: scale(1.05) translateY(-5px);
            box-shadow: 0 15px 40px rgba(255,0,0,0.2);
        }

        .card-image {
            width: 100%;
            height: 180px;
            background: linear-gradient(45deg, #2a0000, #4a0000);
            display: flex;
            align-items: center;
            justify-content: center;
            color: #666;
            font-size: 3rem;
        }

        .card-content {
            padding: 20px;
        }

        .card-title {
            font-size: 1.1rem;
            font-weight: 700;
            margin-bottom: 10px;
            color: #ffffff;
        }

        .card-description {
            font-size: 0.9rem;
            color: #cccccc;
            line-height: 1.4;
            margin-bottom: 15px;
        }

        .card-meta {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .platform-tag {
            background: linear-gradient(135deg, #ff0000, #cc0000);
            color: #ffffff;
            padding: 4px 12px;
            border-radius: 15px;
            font-size: 0.7rem;
            font-weight: 600;
            text-transform: uppercase;
        }

        .timestamp {
            color: #aaaaaa;
            font-size: 0.8rem;
        }

        .generator-section {
            margin-top: 60px;
            background: linear-gradient(145deg, #1a1a1a, #111111);
            border-radius: 16px;
            padding: 40px;
            border: 2px solid rgba(255,0,0,0.2);
        }

        .generator-header {
            text-align: center;
            margin-bottom: 30px;
        }

        .generator-title {
            font-size: 1.8rem;
            font-weight: 700;
            margin-bottom: 10px;
            background: linear-gradient(135deg, #ff6b6b, #ff0000);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .generator-subtitle {
            color: #cccccc;
            font-size: 1rem;
        }

        .form-group {
            margin-bottom: 25px;
        }

        .form-label {
            display: block;
            color: #ffffff;
            font-weight: 600;
            margin-bottom: 10px;
        }

        .form-textarea {
            width: 100%;
            height: 100px;
            background: rgba(30,30,30,0.8);
            border: 2px solid rgba(255,255,255,0.2);
            border-radius: 8px;
            color: #ffffff;
            padding: 15px;
            font-family: inherit;
            resize: vertical;
            outline: none;
        }

        .form-textarea:focus {
            border-color: rgba(255,0,0,0.5);
        }

        .button-group {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            margin-bottom: 25px;
        }

        .option-btn {
            background: rgba(40,40,40,0.8);
            color: #cccccc;
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 20px;
            padding: 8px 16px;
            font-size: 0.9rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .option-btn:hover {
            background: rgba(255,255,255,0.1);
        }

        .option-btn.active {
            background: linear-gradient(135deg, #ff0000, #cc0000);
            color: #ffffff;
            border-color: #ff0000;
        }

        .generate-btn {
            width: 100%;
            background: linear-gradient(135deg, #ff0000, #cc0000);
            color: #ffffff;
            border: none;
            border-radius: 8px;
            padding: 18px;
            font-size: 1.1rem;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-bottom: 30px;
        }

        .generate-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 30px rgba(255,0,0,0.3);
        }

        .generate-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
        }

        .images-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .image-item {
            background: #222222;
            border-radius: 8px;
            overflow: hidden;
            border: 1px solid rgba(255,255,255,0.1);
        }

        .generated-image {
            width: 100%;
            height: 200px;
            background: linear-gradient(45deg, #2a0000, #4a0000);
            display: flex;
            align-items: center;
            justify-content: center;
            color: #666;
            font-size: 2rem;
        }

        .image-actions {
            padding: 15px;
            display: flex;
            gap: 10px;
        }

        .download-btn {
            flex: 1;
            background: #00aa00;
            color: #ffffff;
            border: none;
            border-radius: 6px;
            padding: 10px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .download-btn:hover {
            background: #00cc00;
        }

        .alert {
            padding: 15px;
            border-radius: 8px;
            margin: 15px 0;
            text-align: center;
            font-weight: 600;
        }

        .alert-success {
            background: rgba(0,255,0,0.1);
            border: 1px solid rgba(0,255,0,0.3);
            color: #00ff00;
        }

        .alert-error {
            background: rgba(255,0,0,0.1);
            border: 1px solid rgba(255,0,0,0.3);
            color: #ff6b6b;
        }

        .loading-spinner {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 2px solid rgba(255,255,255,0.3);
            border-top: 2px solid #ffffff;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-right: 10px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @media (max-width: 768px) {
            .navbar {
                padding: 15px 20px;
            }

            .nav-links {
                gap: 15px;
            }

            .hero {
                padding: 0 20px;
                text-align: center;
            }

            .hero-title {
                font-size: 2.5rem;
            }

            .content {
                padding: 60px 20px;
            }

            .horror-card {
                min-width: 250px;
            }

            .generator-section {
                padding: 25px;
            }
        }
    </style>
</head>
<body>
    <nav class="navbar">
        <div class="logo">HORROR FEED</div>
        <div class="nav-links">
            <div class="nav-link active" data-platform="home">
                🏠 Beranda
            </div>
            <div class="nav-link" data-platform="tiktok">
                <svg class="social-icon" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M19.59 6.69a4.83 4.83 0 0 1-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 0 1-5.2 1.74 2.89 2.89 0 0 1 2.31-4.64 2.93 2.93 0 0 1 .88.13V9.4a6.84 6.84 0 0 0-.88-.05A6.33 6.33 0 0 0 5.76 20.5a6.34 6.34 0 0 0 10.86-4.43V7.83a8.2 8.2 0 0 0 4.77 1.52v-3.4a4.85 4.85 0 0 1-1.8-.26z"/>
                </svg>
                TikTok
            </div>
            <div class="nav-link" data-platform="youtube">
                <svg class="social-icon" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/>
                </svg>
                YouTube
            </div>
            <div class="nav-link" data-platform="instagram">
                <svg class="social-icon" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/>
                </svg>
                Instagram
            </div>
        </div>
    </nav>

    <section class="hero">
        <div class="hero-content">
            <h1 class="hero-title">Cerita Horor<br>Indonesia</h1>
            <p class="hero-subtitle">Temukan kisah-kisah menyeramkan dari seluruh nusantara. Dari penampakan mistis hingga ritual kuno yang mengerikan.</p>
            <div class="hero-buttons">
                <button class="btn-primary" onclick="scrollToContent()">
                    ▶ Mulai Menonton
                </button>
                <button class="btn-secondary">
                    ℹ Info Selengkapnya
                </button>
            </div>
        </div>
    </section>

    <section class="content" id="content">
        <div class="content-row">
            <h2 class="section-title">🔥 Trending Hari Ini</h2>
            <div class="carousel" id="trending-carousel">
                <!-- Content will be loaded here -->
            </div>
        </div>

        <div class="content-row">
            <h2 class="section-title">📱 Cerita TikTok Viral</h2>
            <div class="carousel" id="second-carousel">
                <!-- Content will be loaded here -->
            </div>
        </div>

        <div class="generator-section">
            <div class="generator-header">
                <h3 class="generator-title">🎨 Generator Gambar Horor AI</h3>
                <p class="generator-subtitle">Buat gambar horor mengerikan dengan teknologi AI</p>
            </div>

            <div class="form-group">
                <label class="form-label">Deskripsi Gambar Horror:</label>
                <textarea class="form-textarea" id="image-prompt" placeholder="Contoh: Hantu pocong putih di kuburan tua dengan kabut tebal, suasana malam yang mengerikan..."></textarea>
            </div>

            <div class="form-group">
                <label class="form-label">Gaya Horror:</label>
                <div class="button-group">
                    <button class="option-btn active" data-style="realistic">📸 Realistis</button>
                    <button class="option-btn" data-style="gothic">🏰 Gothic</button>
                    <button class="option-btn" data-style="dark">🌙 Dark</button>
                    <button class="option-btn" data-style="vintage">📰 Vintage</button>
                </div>
            </div>

            <div class="form-group">
                <label class="form-label">Tema Horror Indonesia:</label>
                <div class="button-group">
                    <button class="option-btn" data-theme="pocong">👻 Pocong</button>
                    <button class="option-btn" data-theme="kuntilanak">👤 Kuntilanak</button>
                    <button class="option-btn" data-theme="tuyul">👹 Tuyul</button>
                    <button class="option-btn" data-theme="santet">🔮 Santet</button>
                </div>
            </div>

            <button class="generate-btn" id="generate-btn">
                🎨 Generate Gambar Horror
            </button>

            <div id="alert-container"></div>
            <div class="images-grid" id="images-grid"></div>
        </div>
    </section>

    <script>
        // Current active platform
        let currentPlatform = 'home';
        let feedUpdateInterval;
        let timestampUpdateInterval;

        // Horror content data for different platforms
        const horrorFeeds = {
            home: [
                {
                    platform: 'tiktok',
                    title: 'Penampakan Misterius di Kos Jakarta',
                    description: 'Mahasiswa mengalami kejadian aneh di kos. Suara langkah kaki malam hari dan pintu terbuka sendiri.',
                    icon: '👻',
                    timestamp: 5
                },
                {
                    platform: 'youtube',
                    title: 'Ritual Santet di Desa Terpencil',
                    description: 'Dokumenter tentang praktik santet di Jawa Tengah dengan kesaksian korban.',
                    icon: '🔮',
                    timestamp: 12
                },
                {
                    platform: 'instagram',
                    title: 'Foto Pocong di Kuburan Tua',
                    description: 'Foto ziarah kubur menunjukkan sosok putih mencurigakan di belakang makam.',
                    icon: '⚰️',
                    timestamp: 18
                },
                {
                    platform: 'tiktok',
                    title: 'Suara Tangisan dari Rumah Kosong',
                    description: 'Video viral merekam tangisan misterius dari rumah kosong 10 tahun.',
                    icon: '🏚️',
                    timestamp: 25
                },
                {
                    platform: 'youtube',
                    title: 'Misteri Boneka Haunted',
                    description: 'Koleksi boneka antik yang bergerak sendiri di malam hari.',
                    icon: '🪆',
                    timestamp: 30
                },
                {
                    platform: 'instagram',
                    title: 'Kuntilanak di Hutan Jati',
                    description: 'Pendaki gunung mengabadikan sosok perempuan berambut panjang.',
                    icon: '🌲',
                    timestamp: 45
                }
            ],
            tiktok: [
                {
                    platform: 'tiktok',
                    title: 'Hantu Jeruk Purut Live',
                    description: 'Live TikTok dari kuburan jeruk purut Jakarta, viewer melihat bayangan aneh.',
                    icon: '📱',
                    timestamp: 0,
                    isLive: true
                },
                {
                    platform: 'tiktok',
                    title: 'Challenge Midnight Game',
                    description: 'Tiktoker Indonesia main midnight game, hasilnya bikin merinding.',
                    icon: '🕐',
                    timestamp: 3
                },
                {
                    platform: 'tiktok',
                    title: 'Suara Azan di Hutan',
                    description: 'Pendaki dengar suara azan tengah malam di hutan yang tidak ada masjid.',
                    icon: '🌲',
                    timestamp: 8
                },
                {
                    platform: 'tiktok',
                    title: 'Pocong Dance Challenge',
                    description: 'Viral dance challenge dengan kostum pocong, tapi ada yang aneh di videonya.',
                    icon: '💃',
                    timestamp: 15
                },
                {
                    platform: 'tiktok',
                    title: 'Boneka Bergerak Sendiri',
                    description: 'Video viral boneka di kamar kos bergerak sendiri saat pemilik tidur.',
                    icon: '🪆',
                    timestamp: 22
                },
                {
                    platform: 'tiktok',
                    title: 'Panggil Jin dengan Dupa',
                    description: 'Remaja coba ritual panggil jin pakai dupa, ending tidak terduga.',
                    icon: '🕯️',
                    timestamp: 28
                }
            ],
            youtube: [
                {
                    platform: 'youtube',
                    title: 'INVESTIGASI: Rumah Berhantu Cipanas',
                    description: 'Tim paranormal investigasi rumah berhantu di Cipanas yang sudah kosong 20 tahun.',
                    icon: '🎥',
                    timestamp: 60
                },
                {
                    platform: 'youtube',
                    title: 'Eksperimen Ouija Board Indonesia',
                    description: 'Youtuber Indonesia coba ouija board asli, kontak dengan arwah penasaran.',
                    icon: '🔮',
                    timestamp: 120
                },
                {
                    platform: 'youtube',
                    title: 'Cerita Mistis Sopir Angkot Jakarta',
                    description: 'Kumpulan cerita horor dari sopir angkot Jakarta yang mengalami hal mistis.',
                    icon: '🚐',
                    timestamp: 180
                },
                {
                    platform: 'youtube',
                    title: 'Ritual Pelet Jawa Kuno',
                    description: 'Dokumenter tentang ritual pelet kuno di Jawa dengan narasumber ahli.',
                    icon: '🪬',
                    timestamp: 240
                },
                {
                    platform: 'youtube',
                    title: 'Hantu Jembatan Ancol',
                    description: 'Investigasi penampakan hantu di jembatan Ancol yang viral di media sosial.',
                    icon: '🌉',
                    timestamp: 300
                },
                {
                    platform: 'youtube',
                    title: 'Dukun Santet Terkuat Indonesia',
                    description: 'Wawancara eksklusif dengan dukun santet yang diklaim paling sakti.',
                    icon: '🧙',
                    timestamp: 360
                }
            ],
            instagram: [
                {
                    platform: 'instagram',
                    title: 'Story Hantu Lift Mal',
                    description: 'Instagram story viral tentang hantu di lift mal yang muncul di CCTV.',
                    icon: '📸',
                    timestamp: 30
                },
                {
                    platform: 'instagram',
                    title: 'Foto Mistis di Pantai Selatan',
                    description: 'Foto liburan di pantai selatan menampilkan sosok perempuan di laut.',
                    icon: '🌊',
                    timestamp: 60
                },
                {
                    platform: 'instagram',
                    title: 'Selfie dengan Sosok Gaib',
                    description: 'Remaja selfie di kuburan, setelah dilihat ada sosok aneh di belakang.',
                    icon: '🤳',
                    timestamp: 90
                },
                {
                    platform: 'instagram',
                    title: 'Filter IG Panggil Hantu',
                    description: 'Filter Instagram baru yang diklaim bisa memunculkan hantu di foto.',
                    icon: '👻',
                    timestamp: 120
                },
                {
                    platform: 'instagram',
                    title: 'Foto Prewedding Berhantu',
