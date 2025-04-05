<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Stray Care - Helping Stray Animals</title>
  <!-- Leaflet CSS for maps -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <!-- Font Awesome for icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    /* Reset and base styles */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    body {
      line-height: 1.6;
      color: #333;
      background-color: #f0f7ff;
    }

    .container {
      width: 100%;
      max-width: 1200px;
      margin: 0 auto;
      padding: 0 20px;
    }

    /* Header styles */
    header {
      background: linear-gradient(135deg, #3b82f6 0%, #06b6d4 100%);
      color: white;
      padding: 1rem;
      position: sticky;
      top: 0;
      z-index: 50;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    }

    .header-container {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .logo {
      display: flex;
      align-items: center;
      gap: 10px;
      text-decoration: none;
      color: white;
    }

    .logo img {
      width: 40px;
      height: 40px;
      border-radius: 50%;
    }

    .logo-text {
      font-size: 1.5rem;
      font-weight: bold;
    }

    .nav-desktop {
      display: none;
    }

    @media (min-width: 768px) {
      .nav-desktop {
        display: block;
      }
      .mobile-menu-button {
        display: none;
      }
    }

    .nav-list {
      display: flex;
      list-style: none;
      gap: 1.5rem;
    }

    .nav-link {
      display: flex;
      align-items: center;
      gap: 5px;
      color: rgba(255, 255, 255, 0.9);
      text-decoration: none;
      transition: color 0.3s;
      font-size: 0.9rem;
    }

    .nav-link:hover {
      color: rgba(255, 255, 255, 1);
    }

    .nav-link.active {
      color: white;
      font-weight: 600;
    }

    .user-controls {
      display: none;
    }

    @media (min-width: 768px) {
      .user-controls {
        display: flex;
        align-items: center;
        gap: 1rem;
      }
    }

    .sign-in-link {
      color: rgba(255, 255, 255, 0.9);
      text-decoration: none;
      font-size: 0.9rem;
    }

    .sign-in-link:hover {
      color: white;
    }

    .sign-up-link {
      background-color: rgba(255, 255, 255, 0.1);
      padding: 0.5rem 1rem;
      border-radius: 0.375rem;
      color: white;
      text-decoration: none;
      backdrop-filter: blur(4px);
      font-size: 0.9rem;
    }

    .sign-up-link:hover {
      background-color: rgba(255, 255, 255, 0.2);
    }

    .mobile-menu-button {
      background: none;
      border: none;
      color: white;
      font-size: 1.5rem;
      cursor: pointer;
    }

    .mobile-menu {
      display: none;
      background-color: white;
      margin-top: 1rem;
      border-radius: 0.5rem;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      padding: 1rem;
    }

    .mobile-menu.active {
      display: block;
    }

    .mobile-nav-list {
      list-style: none;
      margin-bottom: 1rem;
    }

    .mobile-nav-item {
      margin-bottom: 0.5rem;
    }

    .mobile-nav-link {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      padding: 0.5rem 0;
      color: #333;
      text-decoration: none;
    }

    .mobile-nav-link:hover {
      color: #3b82f6;
    }

    .mobile-user-controls {
      padding-top: 1rem;
      border-top: 1px solid #eee;
    }

    /* Page styles */
    .page {
      display: none;
      padding: 2rem 0;
      min-height: calc(100vh - 70px);
    }

    .page.active {
      display: block;
    }

    /* Button styles */
    .btn {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 0.5rem;
      padding: 0.5rem 1rem;
      border-radius: 0.375rem;
      font-weight: 500;
      cursor: pointer;
      transition: background-color 0.3s, color 0.3s;
      text-decoration: none;
      border: none;
      font-size: 0.875rem;
    }

    .btn-primary {
      background-color: #3b82f6;
      color: white;
    }

    .btn-primary:hover {
      background-color: #2563eb;
    }

    .btn-outline {
      background-color: transparent;
      border: 1px solid #d1d5db;
      color: #374151;
    }

    .btn-outline:hover {
      background-color: #f3f4f6;
    }

    .btn-block {
      display: block;
      width: 100%;
    }

    .btn:disabled {
      opacity: 0.5;
      cursor: not-allowed;
    }

    /* Image slider styles */
    .image-slider {
      position: relative;
      width: 100%;
      height: 500px;
      overflow: hidden;
      border-radius: 0.75rem;
      margin-bottom: 2rem;
    }

    .slide {
      position: absolute;
      width: 100%;
      height: 100%;
      opacity: 0;
      transition: opacity 1s ease-in-out;
    }

    .slide.active {
      opacity: 1;
    }

    .slide img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .slide-overlay {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      background: linear-gradient(to bottom, transparent, rgba(0, 0, 0, 0.7));
      padding: 1.5rem;
      color: white;
    }

    .slide-overlay h2 {
      font-size: 1.5rem;
      margin-bottom: 0.5rem;
    }

    .slider-nav {
      position: absolute;
      bottom: 1rem;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      gap: 0.5rem;
    }

    .slider-dot {
      width: 0.5rem;
      height: 0.5rem;
      border-radius: 50%;
      background-color: rgba(255, 255, 255, 0.5);
      cursor: pointer;
      transition: all 0.3s;
    }

    .slider-dot.active {
      width: 1rem;
      border-radius: 0.25rem;
      background-color: white;
    }

    .slider-btn {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background-color: rgba(0, 0, 0, 0.3);
      color: white;
      border: none;
      width: 2.5rem;
      height: 2.5rem;
      border-radius: 50%;
      font-size: 1.25rem;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: background-color 0.3s;
    }

    .slider-btn:hover {
      background-color: rgba(0, 0, 0, 0.5);
    }

    .slider-btn-prev {
      left: 1rem;
    }

    .slider-btn-next {
      right: 1rem;
    }

    /* Section styles */
    section {
      margin-bottom: 3rem;
    }

    .section-title {
      font-size: 2rem;
      font-weight: 700;
      margin-bottom: 1.5rem;
      color: #0369a1;
    }

    .gradient-text {
      background: linear-gradient(135deg, #3b82f6 0%, #06b6d4 100%);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }

    .text-center {
      text-align: center;
    }

    /* Card styles */
    .card {
      background-color: white;
      border-radius: 0.5rem;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      overflow: hidden;
      margin-bottom: 1.5rem;
    }

    .card-header {
      padding: 1rem;
      border-bottom: 1px solid #f3f4f6;
    }

    .card-title {
      font-size: 1.25rem;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: 0.5rem;
    }

    .card-content {
      padding: 1rem;
    }

    .card-footer {
      padding: 1rem;
      border-top: 1px solid #f3f4f6;
    }

    /* Grid and flex layouts */
    .grid {
      display: grid;
      gap: 1.5rem;
    }

    @media (min-width: 768px) {
      .grid-cols-2 {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    @media (min-width: 1024px) {
      .grid-cols-3 {
        grid-template-columns: repeat(3, 1fr);
      }
    }

    .flex {
      display: flex;
    }

    .flex-col {
      flex-direction: column;
    }

    .items-center {
      align-items: center;
    }

    .justify-between {
      justify-content: space-between;
    }

    .gap-2 {
      gap: 0.5rem;
    }

    .gap-4 {
      gap: 1rem;
    }

    /* Form styles */
    .form-group {
      margin-bottom: 1rem;
    }

    .form-label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 500;
      font-size: 0.875rem;
    }

    .input {
      width: 100%;
      padding: 0.75rem;
      border: 1px solid #d1d5db;
      border-radius: 0.375rem;
      font-size: 0.875rem;
    }

    .input:focus {
      outline: none;
      border-color: #3b82f6;
      box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.3);
    }

    textarea.input {
      resize: vertical;
    }

    /* Tab styles */
    .tabs {
      margin-bottom: 2rem;
    }

    .tab-list {
      display: flex;
      border-bottom: 1px solid #e5e7eb;
      margin-bottom: 1rem;
    }

    .tab {
      padding: 0.5rem 1rem;
      cursor: pointer;
      border-bottom: 2px solid transparent;
      transition: all 0.3s;
    }

    .tab.active {
      border-bottom-color: #3b82f6;
      color: #3b82f6;
    }

    .tab-content {
      display: none;
    }

    .tab-content.active {
      display: block;
    }

    /* Map styles */
    #map, #adoptionMap {
      height: 400px;
      width: 100%;
      border-radius: 0.5rem;
    }

    /* Message styles */
    .message-list {
      height: 300px;
      overflow-y: auto;
    }

    .message {
      padding: 1rem;
      border-radius: 0.5rem;
      margin-bottom: 1rem;
    }

    .message-user {
      background-color: #e0f2fe;
      margin-left: 2rem;
    }

    .message-other {
      background-color: #f9fafb;
      margin-right: 2rem;
    }

    .message-sender {
      font-weight: 600;
      margin-bottom: 0.25rem;
    }

    .message-text {
      color: #4b5563;
    }

    /* Toast notification styles */
    .toast-container {
      position: fixed;
      top: 1rem;
      right: 1rem;
      z-index: 1000;
    }

    .toast {
      background-color: white;
      color: #333;
      padding: 0.75rem 1rem;
      border-radius: 0.375rem;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      margin-bottom: 0.5rem;
      display: flex;
      align-items: center;
      gap: 0.5rem;
      animation: slideIn 0.3s ease-out, fadeOut 0.3s ease-in 2.7s forwards;
    }

    .toast-success {
      border-left: 4px solid #10b981;
    }

    .toast-error {
      border-left: 4px solid #ef4444;
    }

    .toast-info {
      border-left: 4px solid #3b82f6;
    }

    @keyframes slideIn {
      from {
        transform: translateX(100%);
        opacity: 0;
      }
      to {
        transform: translateX(0);
        opacity: 1;
      }
    }

    @keyframes fadeOut {
      from {
        opacity: 1;
      }
      to {
        opacity: 0;
      }
    }

    /* Chatbot styles */
    .chatbot {
      position: fixed;
      bottom: 1rem;
      right: 1rem;
      z-index: 50;
    }

    .chatbot-button {
      width: 3rem;
      height: 3rem;
      border-radius: 50%;
      background-color: #3b82f6;
      color: white;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }

    .chatbot-window {
      position: absolute;
      bottom: 4rem;
      right: 0;
      width: 300px;
      background-color: white;
      border-radius: 0.5rem;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      overflow: hidden;
      display: none;
    }

    .chatbot-window.active {
      display: block;
    }

    .chatbot-header {
      background-color: #3b82f6;
      color: white;
      padding: 0.75rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .chatbot-close {
      background: none;
      border: none;
      color: white;
      cursor: pointer;
      font-size: 1rem;
    }

    .chatbot-messages {
      height: 250px;
      overflow-y: auto;
      padding: 0.75rem;
    }

    .chatbot-message {
      padding: 0.5rem;
      border-radius: 0.375rem;
      margin-bottom: 0.5rem;
      max-width: 80%;
    }

    .chatbot-message-bot {
      background-color: #f3f4f6;
      margin-right: auto;
    }

    .chatbot-message-user {
      background-color: #e0f2fe;
      margin-left: auto;
    }

    .chatbot-input-container {
      padding: 0.75rem;
      border-top: 1px solid #e5e7eb;
      display: flex;
      gap: 0.5rem;
    }

    .chatbot-input {
      flex-grow: 1;
      padding: 0.5rem;
      border: 1px solid #d1d5db;
      border-radius: 0.375rem;
      font-size: 0.875rem;
    }

    .chatbot-send {
      background-color: #3b82f6;
      color: white;
      border: none;
      border-radius: 0.375rem;
      padding: 0.5rem;
      cursor: pointer;
    }

    /* Sign in and Sign up styles */
    .auth-container {
      max-width: 400px;
      margin: 0 auto;
      padding: 2rem;
    }

    .auth-logo {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-bottom: 2rem;
    }

    .auth-logo img {
      width: 80px;
      height: 80px;
      border-radius: 50%;
      margin-bottom: 1rem;
    }

    .auth-title {
      font-size: 1.5rem;
      font-weight: 700;
      color: #0369a1;
      text-align: center;
    }

    .auth-form {
      background-color: white;
      border-radius: 0.5rem;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      padding: 2rem;
    }

    .auth-footer {
      margin-top: 1rem;
      text-align: center;
      font-size: 0.875rem;
      color: #6b7280;
    }

    .auth-link {
      color: #3b82f6;
      text-decoration: none;
    }

    .auth-link:hover {
      text-decoration: underline;
    }

    /* Footer styles */
    footer {
      background-color: #e0f2fe;
      padding: 2rem 0;
      margin-top: 2rem;
    }

    .footer-grid {
      display: grid;
      gap: 1.5rem;
    }

    @media (min-width: 768px) {
      .footer-grid {
        grid-template-columns: repeat(3, 1fr);
      }
    }

    .footer-title {
      font-weight: 700;
      color: #0369a1;
      margin-bottom: 0.5rem;
    }

    .footer-links {
      list-style: none;
    }

    .footer-link {
      color: #333;
      text-decoration: none;
      margin-bottom: 0.25rem;
      display: block;
    }

    .footer-link:hover {
      text-decoration: underline;
    }

    .footer-contact {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      margin-bottom: 0.5rem;
    }

    .footer-social {
      display: flex;
      gap: 1rem;
      margin-top: 0.5rem;
    }

    .footer-social a {
      color: #0369a1;
      font-size: 1.25rem;
    }

    .footer-bottom {
      margin-top: 1.5rem;
      padding-top: 1rem;
      border-top: 1px solid #bfdbfe;
      text-align: center;
      font-size: 0.875rem;
    }

    /* Utility classes */
    .mb-1 {
      margin-bottom: 0.25rem;
    }

    .mb-2 {
      margin-bottom: 0.5rem;
    }

    .mb-4 {
      margin-bottom: 1rem;
    }

    .mb-8 {
      margin-bottom: 2rem;
    }

    .mt-2 {
      margin-top: 0.5rem;
    }

    .mt-4 {
      margin-top: 1rem;
    }

    .w-full {
      width: 100%;
    }

    .text-green {
      color: #10b981;
    }

    .text-yellow {
      color: #f59e0b;
    }

    .text-gray {
      color: #6b7280;
    }

    .mr-2 {
      margin-right: 0.5rem;
    }
  </style>
</head>
<body>
  <!-- Header -->
  <header>
    <div class="container header-container">
      <a href="#" class="logo" onclick="showPage('home')">
        <img src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/Blue_Retro_Animals___Pets_Logo-removebg-preview-Hp48SBWnWC7zZIwbRz5gmxpjGoEebQ.png" alt="Stray Care Logo">
        <span class="logo-text">Stray Care</span>
      </a>

      <!-- Desktop Navigation -->
      <nav class="nav-desktop">
        <ul class="nav-list">
          <li><a href="#" class="nav-link" id="nav-home" onclick="showPage('home')"><i class="fas fa-home"></i> Home</a></li>
          <li><a href="#" class="nav-link" id="nav-help-center" onclick="showPage('help-center')"><i class="fas fa-hand-holding-heart"></i> Help Center</a></li>
          <li><a href="#" class="nav-link" id="nav-adoption" onclick="showPage('adoption')"><i class="fas fa-heart"></i> Adoption</a></li>
          <li><a href="#" class="nav-link" id="nav-donation" onclick="showPage('donation')"><i class="fas fa-dollar-sign"></i> Donation</a></li>
          <li><a href="#" class="nav-link" id="nav-join-community" onclick="showPage('join-community')"><i class="fas fa-users"></i> Join Community</a></li>
          <li><a href="#" class="nav-link" id="nav-profile" onclick="showPage('profile')"><i class="fas fa-user"></i> Profile</a></li>
        </ul>
      </nav>

      <!-- Desktop User Controls -->
      <div class="user-controls">
        <a href="#" class="sign-in-link" onclick="showPage('signin')">Sign In</a>
        <a href="#" class="sign-up-link" onclick="showPage('signup')">Sign Up</a>
      </div>

      <!-- Mobile Menu Button -->
      <button class="mobile-menu-button" id="mobileMenuButton">
        <i class="fas fa-bars"></i>
      </button>
    </div>

    <!-- Mobile Menu -->
    <div class="container">
      <div class="mobile-menu" id="mobileMenu">
        <ul class="mobile-nav-list">
          <li class="mobile-nav-item">
            <a href="#" class="mobile-nav-link" onclick="showPage('home')"><i class="fas fa-home"></i> Home</a>
          </li>
          <li class="mobile-nav-item">
            <a href="#" class="mobile-nav-link" onclick="showPage('help-center')"><i class="fas fa-hand-holding-heart"></i> Help Center</a>
          </li>
          <li class="mobile-nav-item">
            <a href="#" class="mobile-nav-link" onclick="showPage('adoption')"><i class="fas fa-heart"></i> Adoption</a>
          </li>
          <li class="mobile-nav-item">
            <a href="#" class="mobile-nav-link" onclick="showPage('donation')"><i class="fas fa-dollar-sign"></i> Donation</a>
          </li>
          <li class="mobile-nav-item">
            <a href="#" class="mobile-nav-link" onclick="showPage('join-community')"><i class="fas fa-users"></i> Join Community</a>
          </li>
          <li class="mobile-nav-item">
            <a href="#" class="mobile-nav-link" onclick="showPage('profile')"><i class="fas fa-user"></i> Profile</a>
          </li>
        </ul>
        <div class="mobile-user-controls">
          <a href="#" class="btn btn-outline btn-block mb-2" onclick="showPage('signin')">Sign In</a>
          <a href="#" class="btn btn-primary btn-block" onclick="showPage('signup')">Sign Up</a>
        </div>
      </div>
    </div>
  </header>

  <!-- Main Content -->
  <main>
    <!-- Home Page -->
    <div class="page active" id="home-page">
      <div class="container">
        <!-- Image Slider -->
        <section>
          <div class="image-slider" id="imageSlider">
            <div class="slide active">
              <img src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/image-R8xq2fQ5xkg4TmUHy4hovZ3NnXm6Jb.png" alt="Stray dogs drinking milk">
              <div class="slide-overlay">
                <h2>Help Us Feed Stray Animals</h2>
                <p>Join our mission to provide food and care for stray animals in need.</p>
              </div>
            </div>
            <div class="slide">
              <img src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/image-RYaxpqnt6Il4AN1h8UMNZEI5Br4X0X.png" alt="Feeding stray dogs at night">
              <div class="slide-overlay">
                <h2>Every Animal Deserves Care</h2>
                <p>Your support can make a difference in the lives of stray animals.</p>
              </div>
            </div>
            <div class="slide">
              <img src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/image-hBM6soknX7mFANvmTb1dbS6cl50juA.png" alt="Feeding multiple stray dogs">
              <div class="slide-overlay">
                <h2>Together We Can Help Them</h2>
                <p>Volunteer, donate, or adopt to support our cause.</p>
              </div>
            </div>
            <button class="slider-btn slider-btn-prev" id="sliderPrev">
              <i class="fas fa-chevron-left"></i>
            </button>
            <button class="slider-btn slider-btn-next" id="sliderNext">
              <i class="fas fa-chevron-right"></i>
            </button>
            <div class="slider-nav" id="sliderNav">
              <span class="slider-dot active" data-index="0"></span>
              <span class="slider-dot" data-index="1"></span>
              <span class="slider-dot" data-index="2"></span>
            </div>
          </div>
        </section>

        <!-- Welcome Section -->
        <section class="text-center mb-8">
          <h1 class="gradient-text" style="font-size: 2.5rem; font-weight: 700; margin-bottom: 1rem;">Welcome to Stray Care</h1>
          <p style="font-size: 1.25rem; color: #4b5563;">Helping stray animals find food, shelter, and care.</p>
        </section>

        <!-- Find Nearby Hotels Section -->
        <section>
          <h2 class="section-title">Find Nearby Hotels with Food</h2>
          <div class="mb-4 flex flex-col gap-4" style="margin-bottom: 2rem;">
            <div style="display: flex; gap: 1rem; flex-wrap: wrap;">
              <input type="text" id="locationInput" placeholder="Enter your location" class="input" style="flex: 1; min-width: 200px;">
              <button id="searchButton" class="btn btn-primary" style="display: flex; align-items: center;">
                <i class="fas fa-search" style="margin-right: 0.5rem;"></i> Find Nearby Hotels
              </button>
            </div>
          </div>

          <div class="tabs">
            <div class="tab-list">
              <div class="tab active" data-tab="list">List View</div>
              <div class="tab" data-tab="map">Map View</div>
            </div>

            <div class="tab-content active" id="listTab">
              <div class="grid grid-cols-3" id="hotelsList">
                <!-- Hotel cards will be dynamically inserted here -->
              </div>
            </div>

            <div class="tab-content" id="mapTab">
              <div id="map"></div>
              <div id="selectedH  id="mapTab">
              <div id="map"></div>
              <div id="selectedHotelCard" style="margin-top: 1rem; display: none;"></div>
            </div>
          </div>
        </section>

        <!-- Messages Section -->
        <section>
          <h2 class="section-title">Messages</h2>
          <div class="card">
            <div class="card-header">
              <div class="card-title">Recent Messages</div>
            </div>
            <div class="card-content">
              <div class="message-list" id="messageList">
                <!-- Messages will be dynamically inserted here -->
              </div>
            </div>
            <div class="card-footer">
              <div style="display: flex; gap: 0.5rem;">
                <input type="text" id="messageInput" placeholder="Type a message..." class="input" style="flex-grow: 1;">
                <button id="sendMessageButton" class="btn btn-primary">
                  <i class="fas fa-paper-plane"></i>
                </button>
              </div>
            </div>
          </div>
        </section>

        <!-- How You Can Help Section -->
        <section>
          <h2 class="section-title">How You Can Help</h2>
          <div class="grid grid-cols-3">
            <div class="card">
              <div class="card-header">
                <div class="card-title">
                  <i class="fas fa-dollar-sign"></i> Donate
                </div>
              </div>
              <div class="card-content">
                <p>Your donations help us provide food and medical care to stray animals.</p>
              </div>
              <div class="card-footer">
                <a href="#" class="btn btn-primary btn-block" onclick="showPage('donation')">Donate Now</a>
              </div>
            </div>

            <div class="card">
              <div class="card-header">
                <div class="card-title">
                  <i class="fas fa-users"></i> Volunteer
                </div>
              </div>
              <div class="card-content">
                <p>Join our team of volunteers and make a difference in the lives of stray animals.</p>
              </div>
              <div class="card-footer">
                <a href="#" class="btn btn-primary btn-block" onclick="showPage('join-community')">Join Us</a>
              </div>
            </div>

            <div class="card">
              <div class="card-header">
                <div class="card-title">
                  <i class="fas fa-heart"></i> Adopt
                </div>
              </div>
              <div class="card-content">
                <p>Give a loving home to a stray animal and change their life forever.</p>
              </div>
              <div class="card-footer">
                <a href="#" class="btn btn-primary btn-block" onclick="showPage('adoption')">Adopt a Pet</a>
              </div>
            </div>
          </div>
        </section>

        <!-- Animal Icons -->
        <div style="display: flex; justify-content: center; gap: 2rem; margin-top: 3rem;">
          <i class="fas fa-cat" style="font-size: 3rem; color: #3b82f6;"></i>
          <i class="fas fa-dog" style="font-size: 3rem; color: #3b82f6;"></i>
        </div>
      </div>
    </div>

    <!-- Help Center Page -->
    <div class="page" id="help-center-page">
      <div class="container">
        <h1 class="section-title">Help Center</h1>
        
        <div class="tabs">
          <div class="tab-list">
            <div class="tab active" data-tab="medical">Medical Help</div>
            <div class="tab" data-tab="legal">Legal Help</div>
          </div>

          <div class="tab-content active" id="medicalTab">
            <div class="grid grid-cols-2 mb-4">
              <div class="card">
                <div class="card-header">
                  <div class="card-title">Emergency Contacts</div>
                </div>
                <div class="card-content">
                  <img src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/Attendence.pdf%20(2)-7Pf93DNhG7lALxuiDSZW8ensvszWU1.png" alt="Emergency Contact Details" style="width: 100%; height: auto; border-radius: 0.5rem;">
                </div>
                <div class="card-footer">
                  <button class="btn btn-primary btn-block">
                    <i class="fas fa-external-link-alt"></i> View Full Size
                  </button>
                </div>
              </div>

              <div class="card">
                <div class="card-header">
                  <div class="card-title">Additional Contacts</div>
                </div>
                <div class="card-content">
                  <img src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/Attendence.pdf-YNb5qRlSweiDXDcZBprqb3eD6UH8fS.png" alt="Additional Emergency Contacts" style="width: 100%; height: auto; border-radius: 0.5rem;">
                </div>
                <div class="card-footer">
                  <button class="btn btn-primary btn-block">
                    <i class="fas fa-external-link-alt"></i> View Full Size
                  </button>
                </div>
              </div>
            </div>

            <div class="card mb-4">
              <div class="card-header">
                <div class="card-title">24/7 Ambulance Services</div>
              </div>
              <div class="card-content">
                <div class="grid grid-cols-2">
                  <div class="flex items-center gap-2 mb-2">
                    <i class="fas fa-phone text-sky-600"></i>
                    <span>Asha Animal Helpline: 9833684434</span>
                    <button class="btn btn-primary" style="padding: 0.25rem 0.5rem; font-size: 0.75rem;">Call</button>
                  </div>
                  <div class="flex items-center gap-2 mb-2">
                    <i class="fas fa-phone text-sky-600"></i>
                    <span>Samast Mahajan: 9152991599</span>
                    <button class="btn btn-primary" style="padding: 0.25rem 0.5rem; font-size: 0.75rem;">Call</button>
                  </div>
                  <div class="flex items-center gap-2 mb-2">
                    <i class="fas fa-phone text-sky-600"></i>
                    <span>World For All: 8879830802</span>
                    <button class="btn btn-primary" style="padding: 0.25rem 0.5rem; font-size: 0.75rem;">Call</button>
                  </div>
                </div>
              </div>
              <div class="card-footer">
                <button class="btn btn-primary">
                  <i class="fas fa-phone"></i> Call Now
                </button>
              </div>
            </div>

            <div class="card">
              <div class="card-header">
                <div class="card-title">Medicine Guide</div>
              </div>
              <div class="card-content">
                <img src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/Attendence.pdf%20(1)-HSTN5Exzj2QynZrNKM3PTfTyNJO4Le.png" alt="Medicine Guide" style="width: 100%; height: auto; border-radius: 0.5rem;">
              </div>
              <div class="card-footer">
                <button class="btn btn-primary">
                  <i class="fas fa-download"></i> Download Guide
                </button>
              </div>
            </div>
          </div>

          <div class="tab-content" id="legalTab">
            <div class="grid grid-cols-2 mb-4">
              <div class="card">
                <div class="card-header">
                  <div class="card-title">Know Your Rights</div>
                </div>
                <div class="card-content">
                  <ul style="list-style: none;">
                    <li class="flex items-center gap-2 mb-2">
                      <i class="fas fa-balance-scale text-sky-600"></i>
                      <span>Feeding strays is legal and protected under the Prevention of Cruelty to Animals Act</span>
                    </li>
                    <li class="flex items-center gap-2 mb-2">
                      <i class="fas fa-balance-scale text-sky-600"></i>
                      <span>Animal cruelty is punishable by law with fines and imprisonment</span>
                    </li>
                    <li class="flex items-center gap-2 mb-2">
                      <i class="fas fa-balance-scale text-sky-600"></i>
                      <span>No one can prohibit you from feeding or caring for stray animals</span>
                    </li>
                  </ul>
                </div>
                <div class="card-footer">
                  <button class="btn btn-primary">Learn More</button>
                </div>
              </div>

              <div class="card">
                <div class="card-header">
                  <div class="card-title">Report Animal Abuse</div>
                </div>
                <div class="card-content">
                  <p class="mb-4">If you witness animal abuse or neglect, report it to the authorities immediately.</p>
                  <div class="flex items-center gap-2">
                    <i class="fas fa-file-alt text-sky-600"></i>
                    <span>Animal Welfare Board Helpline: 1800-987-6543</span>
                  </div>
                </div>
                <div class="card-footer">
                  <button class="btn btn-primary">
                    <i class="fas fa-phone"></i> Call Helpline
                  </button>
                </div>
              </div>
            </div>

            <div class="card">
              <div class="card-header">
                <div class="card-title">File a Cruelty Complaint</div>
              </div>
              <div class="card-content">
                <form id="crueltyForm">
                  <div class="grid grid-cols-2 gap-4 mb-4">
                    <div class="form-group">
                      <label class="form-label">Location of Incident</label>
                      <input type="text" class="input" required>
                    </div>
                    <div class="form-group">
                      <label class="form-label">Date of Incident</label>
                      <input type="date" class="input" required>
                    </div>
                  </div>
                  <div class="form-group mb-4">
                    <label class="form-label">Description of Incident</label>
                    <textarea class="input" rows="4" required></textarea>
                  </div>
                  <div class="grid grid-cols-2 gap-4 mb-4">
                    <div class="form-group">
                      <label class="form-label">Witness Information (if any)</label>
                      <input type="text" class="input">
                    </div>
                    <div class="form-group">
                      <label class="form-label">Your Contact Information</label>
                      <input type="text" class="input" required>
                    </div>
                  </div>
                  <div class="form-group mb-4">
                    <label class="form-label">Evidence (Photos/Videos)</label>
                    <div class="flex items-center gap-2">
                      <button type="button" class="btn btn-outline">
                        <i class="fas fa-upload"></i> Attach Evidence
                      </button>
                      <span id="fileNameDisplay" class="text-gray"></span>
                    </div>
                  </div>
                  <button type="submit" class="btn btn-primary">Submit Complaint</button>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Adoption Page -->
    <div class="page" id="adoption-page">
      <div class="container">
        <h1 class="section-title">Adopt a Stray</h1>
        
        <div class="mb-4 flex flex-col gap-4" style="margin-bottom: 2rem;">
          <div style="display: flex; gap: 1rem; flex-wrap: wrap;">
            <input type="text" id="adoptionSearchInput" placeholder="Search by name, type, breed, or location" class="input" style="flex: 1; min-width: 200px;">
            <button id="adoptionSearchButton" class="btn btn-primary" style="display: flex; align-items: center;">
              <i class="fas fa-search" style="margin-right: 0.5rem;"></i> Search
            </button>
          </div>
        </div>

        <div class="tabs">
          <div class="tab-list">
            <div class="tab active" data-tab="adoptionList">List View</div>
            <div class="tab" data-tab="adoptionMap">Map View</div>
          </div>

          <div class="tab-content active" id="adoptionListTab">
            <div class="grid grid-cols-3" id="animalsList">
              <!-- Animal cards will be dynamically inserted here -->
            </div>
          </div>

          <div class="tab-content" id="adoptionMapTab">
            <div id="adoptionMap" style="height: 400px;"></div>
          </div>
        </div>
      </div>
    </div>

    <!-- Donation Page -->
    <div class="page" id="donation-page">
      <div class="container">
        <h1 class="section-title">Donate to Help Strays</h1>
        
        <div id="donationList" class="grid grid-cols-3">
          <!-- NGO cards will be dynamically inserted here -->
        </div>

        <div id="donationForm" style="display: none;" class="card" style="max-width: 500px; margin: 0 auto;">
          <div class="card-header">
            <div class="card-title">Donate to <span id="donationNgoName"></span></div>
          </div>
          <div class="card-content">
            <div class="form-group mb-4">
              <label class="form-label">Select Amount (₹)</label>
              <div class="grid grid-cols-3 gap-2">
                <button type="button" class="btn btn-outline donation-amount-btn" data-amount="100">₹100</button>
                <button type="button" class="btn btn-outline donation-amount-btn" data-amount="500">₹500</button>
                <button type="button" class="btn btn-outline donation-amount-btn" data-amount="1000">₹1000</button>
              </div>
            </div>
          </div>
          <div class="card-footer">
            <button id="confirmDonationBtn" class="btn btn-primary btn-block mb-2">
              <i class="fas fa-heart"></i> Confirm Donation
            </button>
            <button id="cancelDonationBtn" class="btn btn-outline btn-block">Cancel</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Join Community Page -->
    <div class="page" id="join-community-page">
      <div class="container">
        <h1 class="section-title">Join Our Community</h1>
        
        <div class="grid grid-cols-2 gap-8">
          <div class="card">
            <div class="card-header">
              <div class="card-title">Sign Up</div>
            </div>
            <div class="card-content">
              <form id="communityForm">
                <div class="form-group mb-4">
                  <label class="form-label">Full Name *</label>
                  <input type="text" class="input" id="communityName" required>
                </div>
                <div class="form-group mb-4">
                  <label class="form-label">Email *</label>
                  <input type="email" class="input" id="communityEmail" required>
                </div>
                <div class="form-group mb-4">
                  <label class="form-label">Password *</label>
                  <input type="password" class="input" id="communityPassword" required>
                </div>
                <div class="form-group mb-4">
                  <label class="form-label">Phone Number</label>
                  <input type="tel" class="input" id="communityPhone">
                </div>
                <div class="form-group mb-4">
                  <label class="form-label">Address</label>
                  <input type="text" class="input" id="communityAddress">
                </div>
                <button type="submit" class="btn btn-primary btn-block">
                  <i class="fas fa-user-plus"></i> Join Now
                </button>
              </form>
            </div>
          </div>

          <div>
            <div class="card mb-4">
              <div class="card-header">
                <div class="card-title">Your Digital ID Card</div>
              </div>
              <div class="card-content">
                <div style="border: 2px solid #0369a1; padding: 1rem; border-radius: 0.5rem;">
                  <div class="flex items-center justify-between mb-4">
                    <i class="fas fa-paw" style="font-size: 2rem; color: #0369a1;"></i>
                    <h3 style="font-size: 1.25rem; font-weight: 700; color: #0369a1;">Stray Care</h3>
                  </div>
                  <div style="line-height: 1.8;">
                    <p><strong>Name:</strong> <span id="idCardName">Your Name</span></p>
                    <p><strong>Member ID:</strong> SC<span id="idCardNumber">12345</span></p>
                    <p><strong>Join Date:</strong> <span id="idCardDate">2023-01-01</span></p>
                    <p><strong>Email:</strong> <span id="idCardEmail">your.email@example.com</span></p>
                    <p><strong>Phone:</strong> <span id="idCardPhone">Not provided</span></p>
                  </div>
                </div>
              </div>
            </div>

            <div class="card">
              <div class="card-header">
                <div class="card-title">Submit a Response</div>
              </div>
              <div class="card-content">
                <form id="responseForm">
                  <div class="form-group mb-4">
                    <label class="form-label">Your Response</label>
                    <textarea class="input" rows="4" placeholder="Share your thoughts, questions, or feedback..." required></textarea>
                  </div>
                  <button type="submit" class="btn btn-primary btn-block">
                    <i class="fas fa-paper-plane"></i> Submit Response
                  </button>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Profile Page -->
    <div class="page" id="profile-page">
      <div class="container">
        <h1 class="section-title">Your Profile</h1>
        
        <div class="card" style="max-width: 800px; margin: 0 auto;">
          <div class="card-content">
            <form id="profileForm">
              <div class="form-group mb-4">
                <div class="flex items-center gap-4">
                  <i class="fas fa-user" style="font-size: 1.5rem; color: #0369a1;"></i>
                  <input type="text" class="input" placeholder="Full Name" id="profileName">
                </div>
              </div>
              <div class="form-group mb-4">
                <div class="flex items-center gap-4">
                  <i class="fas fa-envelope" style="font-size: 1.5rem; color: #0369a1;"></i>
                  <input type="email" class="input" placeholder="Email" id="profileEmail">
                </div>
              </div>
              <div class="form-group mb-4">
                <div class="flex items-center gap-4">
                  <i class="fas fa-phone" style="font-size: 1.5rem; color: #0369a1;"></i>
                  <input type="tel" class="input" placeholder="Phone Number" id="profilePhone">
                </div>
              </div>
              <div class="form-group mb-4">
                <div class="flex items-center gap-4">
                  <i class="fas fa-map-marker-alt" style="font-size: 1.5rem; color: #0369a1;"></i>
                  <input type="text" class="input" placeholder="Address" id="profileAddress">
                </div>
              </div>
              <button type="submit" class="btn btn-primary btn-block">
                <i class="fas fa-save"></i> Save Changes
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>

    <!-- Sign In Page -->
    <div class="page" id="signin-page">
      <div class="container">
        <div class="auth-container">
          <div class="auth-logo">
            <img src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/Blue_Retro_Animals___Pets_Logo-removebg-preview-Hp48SBWnWC7zZIwbRz5gmxpjGoEebQ.png" alt="Stray Care Logo">
            <h2 class="auth-title">Sign in to Stray Care</h2>
          </div>
          <div class="auth-form">
            <form id="signinForm">
              <div class="form-group mb-4">
                <input type="email" class="input" placeholder="Email address" required>
              </div>
              <div class="form-group mb-4">
                <input type="password" class="input" placeholder="Password" required>
              </div>
              <div class="flex justify-between mb-4">
                <a href="#" class="auth-link">Forgot your password?</a>
              </div>
              <button type="submit" class="btn btn-primary btn-block">Sign in</button>
            </form>
          </div>
          <div class="auth-footer">
            Don't have an account? <a href="#" class="auth-link" onclick="showPage('signup')">Sign up</a>
          </div>
        </div>
      </div>
    </div>

    <!-- Sign Up Page -->
    <div class="page" id="signup-page">
      <div class="container">
        <div class="auth-container">
          <div class="auth-logo">
            <img src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/Blue_Retro_Animals___Pets_Logo-removebg-preview-Hp48SBWnWC7zZIwbRz5gmxpjGoEebQ.png" alt="Stray Care Logo">
            <h2 class="auth-title">Create your account</h2>
          </div>
          <div class="auth-form">
            <form id="signupForm">
              <div class="form-group mb-4">
                <input type="text" class="input" placeholder="Full name" required>
              </div>
              <div class="form-group mb-4">
                <input type="email" class="input" placeholder="Email address" required>
              </div>
              <div class="form-group mb-4">
                <input type="password" class="input" placeholder="Password" required>
              </div>
              <button type="submit" class="btn btn-primary btn-block">Sign up</button>
            </form>
          </div>
          <div class="auth-footer">
            Already have an account? <a href="#" class="auth-link" onclick="showPage('signin')">Sign in</a>
          </div>
        </div>
      </div>
    </div>
  </main>

  <!-- Footer -->
  <footer>
    <div class="container">
      <div class="footer-grid">
        <div>
          <h3 class="footer-title">Stray Care</h3>
          <p style="font-size: 0.875rem;">Helping stray animals find food, shelter, and care.</p>
        </div>
        <div>
          <h3 class="footer-title">Quick Links</h3>
          <ul class="footer-links">
            <li><a href="#" class="footer-link" onclick="showPage('home')">Home</a></li>
            <li><a href="#" class="footer-link" onclick="showPage('help-center')">Medical Help</a></li>
            <li><a href="#" class="footer-link" onclick="showPage('help-center')">Legal Help</a></li>
            <li><a href="#" class="footer-link" onclick="showPage('donation')">Donation</a></li>
          </ul>
        </div>
        <div>
          <h3 class="footer-title">Contact Us</h3>
          <div style="font-size: 0.875rem;">
            <div class="footer-contact">
              <i class="fas fa-envelope"></i>
              <span>support@straycare.org</span>
            </div>
            <div class="footer-contact">
              <i class="fas fa-phone"></i>
              <span>+91 9876543210</span>
            </div>
          </div>
          <div class="footer-social">
            <a href="#"><i class="fab fa-facebook"></i></a>
            <a href="#"><i class="fab fa-twitter"></i></a>
            <a href="#"><i class="fab fa-instagram"></i></a>
          </div>
        </div>
      </div>
      <div class="footer-bottom">
        <p>&copy; 2024 Stray Care. All rights reserved.</p>
      </div>
    </div>
  </footer>

  <!-- Chatbot -->
  <div class="chatbot">
    <div class="chatbot-button" id="chatbotButton">
      <i class="fas fa-comment"></i>
    </div>
    <div class="chatbot-window" id="chatbotWindow">
      <div class="chatbot-header">
        <div>Stray Care Support</div>
        <button class="chatbot-close" id="chatbotClose">
          <i class="fas fa-times"></i>
        </button>
      </div>
      <div class="chatbot-messages" id="chatbotMessages">
        <div class="chatbot-message chatbot-message-bot">
          Hello! How can I help you today?
        </div>
      </div>
      <div class="chatbot-input-container">
        <input type="text" class="chatbot-input" id="chatbotInput" placeholder="Type a message...">
        <button class="chatbot-send" id="chatbotSend">
          <i class="fas fa-paper-plane"></i>
        </button>
      </div>
    </div>
  </div>

  <!-- Toast Container -->
  <div class="toast-container" id="toastContainer"></div>

  <!-- Leaflet JS for maps -->
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

  <script>
    // Data
    const hotels = [
      { name: "Hotel Taj", food: "5 kg", status: "Available", lat: 18.9217, lng: 72.8332 },
      { name: "Grand Hyatt", food: "3 kg", status: "Processing", lat: 19.0748, lng: 72.8856 },
      { name: "The Oberoi", food: "7 kg", status: "Available", lat: 18.9265, lng: 72.8213 },
      { name: "Marriott", food: "4 kg", status: "Collected", lat: 19.1071, lng: 72.8227 },
      { name: "The Westin", food: "6 kg", status: "Available", lat: 19.1187, lng: 72.8478 },
      { name: "Four Seasons", food: "2 kg", status: "Processing", lat: 18.9942, lng: 72.8236 },
    ];

    const messages = [
      { name: "Hotel Taj", message: "We have 5kg of food available for collection." },
      { name: "Grand Hyatt", message: "Your collection request has been approved." },
      { name: "The Oberoi", message: "New food available for collection." },
    ];

    const animals = [
      { id: 1, name: "Buddy", type: "Dog", breed: "Labrador", age: 2, location: "Mumbai", image: "/placeholder.svg?height=200&width=200" },
      { id: 2, name: "Whiskers", type: "Cat", breed: "Persian", age: 1, location: "Delhi", image: "/placeholder.svg?height=200&width=200" },
      { id: 3, name: "Rocky", type: "Dog", breed: "German Shepherd", age: 3, location: "Bangalore", image: "/placeholder.svg?height=200&width=200" },
      { id: 4, name: "Mittens", type: "Cat", breed: "Siamese", age: 2, location: "Chennai", image: "/placeholder.svg?height=200&width=200" },
      { id: 5, name: "Max", type: "Dog", breed: "Beagle", age: 1, location: "Kolkata", image: "/placeholder.svg?height=200&width=200" },
      { id: 6, name: "Luna", type: "Cat", breed: "Maine Coon", age: 4, location: "Hyderabad", image: "/placeholder.svg?height=200&width=200" },
    ];

    const ngos = [
      { name: "PAL Foundation", description: "Help us make a difference in the lives of stray animals." },
      { name: "Samast Mahajan NGO", description: "Supporting stray animals across Mumbai." },
      { name: "Ahinsa Trust Clinic", description: "Providing medical care to injured strays." },
      { name: "Karuna Trust", description: "Feeding and sheltering stray animals in need." },
      { name: "Asha Animal Helpline", description: "24/7 emergency services for stray animals." },
      { name: "Raksha Animals Helpline", description: "Rescue and rehabilitation for stray animals." },
    ];

    // DOM Elements
    const mobileMenuButton = document.getElementById('mobileMenuButton');
    const mobileMenu = document.getElementById('mobileMenu');
    const imageSlider = document.getElementById('imageSlider');
    const sliderPrev = document.getElementById('sliderPrev');
    const sliderNext = document.getElementById('sliderNext');
    const sliderNav = document.getElementById('sliderNav');
    const slides = imageSlider.querySelectorAll('.slide');
    const locationInput = document.getElementById('locationInput');
    const searchButton = document.getElementById('searchButton');
    const hotelsList = document.getElementById('hotelsList');
    const tabs = document.querySelectorAll('.tab');
    const tabContents = document.querySelectorAll('.tab-content');
    const messageList = document.getElementById('messageList');
    const messageInput = document.getElementById('messageInput');
    const sendMessageButton = document.getElementById('sendMessageButton');
    const chatbotButton = document.getElementById('chatbotButton');
    const chatbotWindow = document.getElementById('chatbotWindow');
    const chatbotClose = document.getElementById('chatbotClose');
    const chatbotMessages = document.getElementById('chatbotMessages');
    const chatbotInput = document.getElementById('chatbotInput');
    const chatbotSend = document.getElementById('chatbotSend');
    const toastContainer = document.getElementById('toastContainer');
    const animalsList = document.getElementById('animalsList');
    const donationList = document.getElementById('donationList');
    const donationForm = document.getElementById('donationForm');
    const donationNgoName = document.getElementById('donationNgoName');
    const confirmDonationBtn = document.getElementById('confirmDonationBtn');
    const cancelDonationBtn = document.getElementById('cancelDonationBtn');
    const communityForm = document.getElementById('communityForm');
    const idCardName = document.getElementById('idCardName');
    const idCardNumber = document.getElementById('idCardNumber');
    const idCardDate = document.getElementById('idCardDate');
    const idCardEmail = document.getElementById('idCardEmail');
    const idCardPhone = document.getElementById('idCardPhone');
    const responseForm = document.getElementById('responseForm');
    const profileForm = document.getElementById('profileForm');
    const signinForm = document.getElementById('signinForm');
    const signupForm = document.getElementById('signupForm');
    const crueltyForm = document.getElementById('crueltyForm');
    const selectedHotelCard = document.getElementById('selectedHotelCard');

    // Initialize Map
    let map = null;
    let adoptionMap = null;
    let markers = [];
    let selectedHotel = null;

    function initMap() {
      if (!map) {
        map = L.map('map').setView([19.076, 72.8777], 12);
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
          attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(map);
      }
      
      // Clear existing markers
      markers.forEach(marker => map.removeLayer(marker));
      markers = [];
      
      // Add new markers
      hotels.forEach(hotel => {
        const marker = L.marker([hotel.lat, hotel.lng]).addTo(map);
        marker.bindPopup(`
          <strong>${hotel.name}</strong><br>
          Food: ${hotel.food}<br>
          Status: ${hotel.status}
        `);
        marker.on('click', () => {
          handleMarkerClick(hotel);
        });
        markers.push(marker);
      });
    }

    function initAdoptionMap() {
      if (!adoptionMap) {
        adoptionMap = L.map('adoptionMap').setView([20.5937, 78.9629], 5);
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
          attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(adoptionMap);
      }
      
      // Add random markers for animals
      animals.forEach(animal => {
        const lat = Math.random() * (28.7041 - 8.4778) + 8.4778; // Random lat within India
        const lng = Math.random() * (97.3956 - 68.7932) + 68.7932; // Random lng within India
        
        const marker = L.marker([lat, lng]).addTo(adoptionMap);
        marker.bindPopup(`
          <strong>${animal.name}</strong><br>
          Type: ${animal.type}<br>
          Breed: ${animal.breed}<br>
          Location: ${animal.location}
        `);
      });
    }

    // Page Navigation
    function showPage(pageId) {
      // Hide all pages
      document.querySelectorAll('.page').forEach(page => {
        page.classList.remove('active');
      });
      
      // Show selected page
      document.getElementById(`${pageId}-page`).classList.add('active');
      
      // Update navigation active state
      document.querySelectorAll('.nav-link').forEach(link => {
        link.classList.remove('active');
      });
      
      const navLink = document.getElementById(`nav-${pageId}`);
      if (navLink) {
        navLink.classList.add('active');
      }
      
      // Close mobile menu if open
      mobileMenu.classList.remove('active');
      
      // Initialize page-specific content
      if (pageId === 'home') {
        renderHotelCards();
        renderMessages();
      } else if (pageId === 'help-center') {
        // Initialize help center tabs
        document.querySelector('.tab[data-tab="medical"]').click();
      } else if (pageId === 'adoption') {
        renderAnimalCards();
        setTimeout(() => {
          initAdoptionMap();
        }, 100);
      } else if (pageId === 'donation') {
        renderDonationCards();
      } else if (pageId === 'join-community') {
        // Generate random ID card number
        idCardNumber.textContent = Math.floor(Math.random() * 90000) + 10000;
        idCardDate.textContent = new Date().toLocaleDateString();
      }
      
      // Scroll to top
      window.scrollTo(0, 0);
    }

    // Mobile Menu Toggle
    mobileMenuButton.addEventListener('click', () => {
      mobileMenu.classList.toggle('active');
    });

    // Image Slider
    let currentSlide = 0;
    const totalSlides = slides.length;

    function showSlide(index) {
      slides.forEach(slide => slide.classList.remove('active'));
      const dots = sliderNav.querySelectorAll('.slider-dot');
      dots.forEach(dot => dot.classList.remove('active'));
      
      currentSlide = (index + totalSlides) % totalSlides;
      slides[currentSlide].classList.add('active');
      dots[currentSlide].classList.add('active');
    }

    sliderPrev.addEventListener('click', () => {
      showSlide(currentSlide - 1);
    });

    sliderNext.addEventListener('click', () => {
      showSlide(currentSlide + 1);
    });

    // Initialize slider dots
    sliderNav.querySelectorAll('.slider-dot').forEach(dot => {
      dot.addEventListener('click', () => {
        const index = parseInt(dot.getAttribute('data-index'));
        showSlide(index);
      });
    });

    // Auto slide
    setInterval(() => {
      showSlide(currentSlide + 1);
    }, 5000);

    // Tab Switching
    tabs.forEach(tab => {
      tab.addEventListener('click', () => {
        const tabName = tab.getAttribute('data-tab');
        const tabGroup = tab.closest('.tab-list');
        const tabContents = tabGroup.nextElementSibling.querySelectorAll('.tab-content');
        
        // Remove active class from all tabs in this group
        tabGroup.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
        
        // Remove active class from all tab contents in this group
        tabContents.forEach(content => content.classList.remove('active'));
        
        // Add active class to clicked tab
        tab.classList.add('active');
        
        // Add active class to corresponding tab content
        const tabContent = document.getElementById(`${tabName}Tab`);
        if (tabContent) {
          tabContent.classList.add('active');
        }
        
        // Initialize map if map tab
        if (tabName === 'map') {
          setTimeout(() => {
            initMap();
          }, 100);
        } else if (tabName === 'adoptionMap') {
          setTimeout(() => {
            initAdoptionMap();
          }, 100);
        }
      });
    });

    // Render Hotel Cards
    function renderHotelCards() {
      hotelsList.innerHTML = '';
      
      hotels.forEach(hotel => {
        const card = document.createElement('div');
        card.className = 'card';
        card.innerHTML = `
          <div class="card-header">
            <div class="card-title">${hotel.name}</div>
          </div>
          <div class="card-content">
            <p>Available food: ${hotel.food}</p>
            <p class="text-${hotel.status === 'Available' ? 'green' : hotel.status === 'Processing' ? 'yellow' : 'gray'}">
              Status: ${hotel.status}
            </p>
          </div>
          <div class="card-footer">
            <div class="flex flex-col gap-2">
              <button class="btn btn-outline message-hotel-btn" data-hotel="${hotel.name}">
                <i class="fas fa-comment"></i> Message
              </button>
              <button class="btn btn-primary collect-food-btn" data-hotel="${hotel.name}" ${hotel.status !== 'Available' ? 'disabled' : ''}>
                Collect Food
              </button>
            </div>
          </div>
        `;
        hotelsList.appendChild(card);
      });
      
      // Add event listeners to buttons
      document.querySelectorAll('.message-hotel-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          const hotelName = btn.getAttribute('data-hotel');
          addMessage('You', `Hello ${hotelName}, I would like to collect food.`);
        });
      });
      
      document.querySelectorAll('.collect-food-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          const hotelName = btn.getAttribute('data-hotel');
          handleCollectFood(hotelName);
        });
      });
    }

    // Render Animal Cards
    function renderAnimalCards() {
      animalsList.innerHTML = '';
      
      animals.forEach(animal => {
        const card = document.createElement('div');
        card.className = 'card';
        card.innerHTML = `
          <div class="card-header">
            <div class="card-title">${animal.name}</div>
          </div>
          <div class="card-content">
            <img src="${animal.image}" alt="${animal.name}" style="width: 100%; height: 200px; object-fit: cover; border-radius: 0.5rem; margin-bottom: 1rem;">
            <p><strong>Type:</strong> ${animal.type}</p>
            <p><strong>Breed:</strong> ${animal.breed}</p>
            <p><strong>Age:</strong> ${animal.age} ${animal.age === 1 ? 'year' : 'years'}</p>
            <p class="flex items-center mt-2">
              <i class="fas fa-map-marker-alt mr-2"></i>
              ${animal.location}
            </p>
          </div>
          <div class="card-footer">
            <button class="btn btn-primary btn-block adopt-btn" data-animal="${animal.name}">
              <i class="fas fa-heart"></i> Adopt ${animal.name}
            </button>
          </div>
        `;
        animalsList.appendChild(card);
      });
      
      // Add event listeners to buttons
      document.querySelectorAll('.adopt-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          const animalName = btn.getAttribute('data-animal');
          showToast('success', `Thank you for your interest in adopting ${animalName}! We'll contact you soon.`);
        });
      });
    }

    // Render Donation Cards
    function renderDonationCards() {
      donationList.innerHTML = '';
      
      ngos.forEach(ngo => {
        const card = document.createElement('div');
        card.className = 'card';
        card.innerHTML = `
          <div class="card-header">
            <div class="card-title">${ngo.name}</div>
          </div>
          <div class="card-content">
            <p>${ngo.description}</p>
          </div>
          <div class="card-footer">
            <button class="btn btn-primary btn-block donate-btn" data-ngo="${ngo.name}">
              <i class="fas fa-heart"></i> Donate Now
            </button>
          </div>
        `;
        donationList.appendChild(card);
      });
      
      // Add event listeners to buttons
      document.querySelectorAll('.donate-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          const ngoName = btn.getAttribute('data-ngo');
          showDonationForm(ngoName);
        });
      });
    }

    // Show Donation Form
    function showDonationForm(ngoName) {
      donationList.style.display = 'none';
      donationForm.style.display = 'block';
      donationNgoName.textContent = ngoName;
      
      // Reset donation amount buttons
      document.querySelectorAll('.donation-amount-btn').forEach(btn => {
        btn.classList.remove('btn-primary');
        btn.classList.add('btn-outline');
      });
      document.querySelector('.donation-amount-btn[data-amount="100"]').classList.remove('btn-outline');
      document.querySelector('.donation-amount-btn[data-amount="100"]').classList.add('btn-primary');
    }

    // Handle marker click
    function handleMarkerClick(hotel) {
      selectedHotel = hotel;
      showToast('info', `Selected ${hotel.name}`);
      
      selectedHotelCard.style.display = 'block';
      selectedHotelCard.innerHTML = `
        <div class="card">
          <div class="card-header">
            <div class="card-title">${hotel.name}</div>
          </div>
          <div class="card-content">
            <p>Available food: ${hotel.food}</p>
            <p class="text-${hotel.status === 'Available' ? 'green' : hotel.status === 'Processing' ? 'yellow' : 'gray'}">
              Status: ${hotel.status}
            </p>
          </div>
          <div class="card-footer">
            <div class="flex flex-col gap-2">
              <button id="mapMessageBtn" class="btn btn-outline">
                <i class="fas fa-comment"></i> Message
              </button>
              <button id="mapCollectBtn" class="btn btn-primary" ${hotel.status !== 'Available' ? 'disabled' : ''}>
                Collect Food
              </button>
            </div>
          </div>
        </div>
      `;
      
      document.getElementById('mapMessageBtn').addEventListener('click', () => {
        addMessage('You', `Hello ${hotel.name}, I would like to collect food.`);
      });
      
      document.getElementById('mapCollectBtn').addEventListener('click', () => {
        handleCollectFood(hotel.name);
      });
    }

    // Handle collect food
    function handleCollectFood(hotelName) {
      showToast('success', `Food collection request sent to ${hotelName}`);
      
      // Update hotel status
      hotels.forEach(hotel => {
        if (hotel.name === hotelName) {
          hotel.status = 'Processing';
        }
      });
      
      // Re-render hotel cards and update map
      renderHotelCards();
      if (map) {
        initMap();
      }
      
      // Update selected hotel card if visible
      if (selectedHotel && selectedHotel.name === hotelName) {
        selectedHotel.status = 'Processing';
        handleMarkerClick(selectedHotel);
      }
    }

    // Render Messages
    function renderMessages() {
      messageList.innerHTML = '';
      
      messages.forEach(msg => {
        const messageDiv = document.createElement('div');
        messageDiv.className = `message ${msg.name === 'You' ? 'message-user' : 'message-other'}`;
        
        messageDiv.innerHTML = `
          <div class="message-sender">${msg.name}</div>
          <div class="message-text">${msg.message}</div>
          ${msg.name !== 'You' ? `
            <div class="flex justify-end mt-2">
              <button class="btn btn-outline reply-btn" data-name="${msg.name}">Reply</button>
            </div>
          ` : ''}
        `;
        
        messageList.appendChild(messageDiv);
      });
      
      // Scroll to bottom
      messageList.scrollTop = messageList.scrollHeight;
      
      // Add event listeners to reply buttons
      document.querySelectorAll('.reply-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          const name = btn.getAttribute('data-name');
          addMessage('You', `Hello ${name}, thank you for the information.`);
        });
      });
    }

    // Add Message
    function addMessage(name, message) {
      messages.push({ name, message });
      renderMessages();
      
      if (name === 'You') {
        // Simulate response
        setTimeout(() => {
          messages.push({
            name: 'Hotel Support',
            message: 'Thank you for your message. We\'ll get back to you shortly.'
          });
          renderMessages();
        }, 1000);
      }
    }

    // Search Button
    searchButton.addEventListener('click', () => {
      const location = locationInput.value.trim();
      if (!location) {
        showToast('error', 'Please enter a location');
        return;
      }
      
      showToast('success', `Searching for hotels near ${location}`);
      // In a real app, this would filter hotels based on location
    });

    // Send Message
    sendMessageButton.addEventListener('click', sendMessage);
    messageInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        sendMessage();
      }
    });

    function sendMessage() {
      const message = messageInput.value.trim();
      if (!message) return;
      
      addMessage('You', message);
      messageInput.value = '';
    }

    // Chatbot
    chatbotButton.addEventListener('click', () => {
      chatbotWindow.classList.add('active');
    });

    chatbotClose.addEventListener('click', () => {
      chatbotWindow.classList.remove('active');
    });

    chatbotSend.addEventListener('click', sendChatbotMessage);
    chatbotInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        sendChatbotMessage();
      }
    });

    function sendChatbotMessage() {
      const message = chatbotInput.value.trim();
      if (!message) return;
      
      // Add user message
      const userMessageDiv = document.createElement('div');
      userMessageDiv.className = 'chatbot-message chatbot-message-user';
      userMessageDiv.textContent = message;
      chatbotMessages.appendChild(userMessageDiv);
      
      chatbotInput.value = '';
      
      // Scroll to bottom
      chatbotMessages.scrollTop = chatbotMessages.scrollHeight;
      
      // Simulate bot response
      setTimeout(() => {
        const botMessageDiv = document.createElement('div');
        botMessageDiv.className = 'chatbot-message chatbot-message-bot';
        botMessageDiv.textContent = getChatbotResponse(message);
        chatbotMessages.appendChild(botMessageDiv);
        
        // Scroll to bottom
        chatbotMessages.scrollTop = chatbotMessages.scrollHeight;
      }, 1000);
    }

    function getChatbotResponse(message) {
      const lowerMessage = message.toLowerCase();
      
      if (lowerMessage.includes('hello') || lowerMessage.includes('hi') || lowerMessage.includes('hey')) {
        return 'Hello! How can I help you today?';
      }
      
      if (lowerMessage.includes('help') || lowerMessage.includes('support') || lowerMessage.includes('assist')) {
        return 'I can help you with food collection, medical emergencies, donations, and more. What do you need assistance with?';
      }
      
      if (lowerMessage.includes('food') || lowerMessage.includes('collect') || lowerMessage.includes('waste')) {
        return 'You can find nearby hotels with available food on our home page. Would you like me to guide you through the process?';
      }
      
      if (lowerMessage.includes('medical') || lowerMessage.includes('emergency') || lowerMessage.includes('vet')) {
        return 'For medical emergencies, please visit our Medical Help page or call our 24/7 helpline at 9833684434.';
      }
      
      if (lowerMessage.includes('donate') || lowerMessage.includes('donation') || lowerMessage.includes('money')) {
        return 'Thank you for considering a donation! You can find various NGOs on our Donation page. Would you like me to direct you there?';
      }
      
      return 'I\'m not sure how to help with that. Would you like to speak with a human representative?';
    }

    // Toast Notifications
    function showToast(type, message) {
      const toast = document.createElement('div');
      toast.className = `toast toast-${type}`;
      toast.innerHTML = `
        <i class="fas fa-${type === 'success' ? 'check-circle' : type === 'error' ? 'exclamation-circle' : 'info-circle'}"></i>
        <span>${message}</span>
      `;
      
      toastContainer.appendChild(toast);
      
      // Remove toast after 3 seconds
      setTimeout(() => {
        toast.remove();
      }, 3000);
    }

    // Form Submissions
    if (communityForm) {
      communityForm.addEventListener('submit', (e) => {
        e.preventDefault();
        const name = document.getElementById('communityName').value;
        const email = document.getElementById('communityEmail').value;
        
        // Update ID card
        idCardName.textContent = name || 'Your Name';
        idCardEmail.textContent = email || 'your.email@example.com';
        idCardPhone.textContent = document.getElementById('communityPhone').value || 'Not provided';
        
        showToast('success', 'Successfully joined the community!');
        setTimeout(() => {
          showPage('home');
        }, 2000);
      });
    }

    if (responseForm) {
      responseForm.addEventListener('submit', (e) => {
        e.preventDefault();
        showToast('success', 'Response submitted successfully!');
        e.target.reset();
      });
    }

    if (profileForm) {
      profileForm.addEventListener('submit', (e) => {
        e.preventDefault();
        showToast('success', 'Profile updated successfully!');
      });
    }

    if (signinForm) {
      signinForm.addEventListener('submit', (e) => {
        e.preventDefault();
        showToast('success', 'Successfully signed in!');
        setTimeout(() => {
          showPage('home');
        }, 2000);
      });
    }

    if (signupForm) {
      signupForm.addEventListener('submit', (e) => {
        e.preventDefault();
        showToast('success', 'Successfully signed up!');
        setTimeout(() => {
          showPage('signin');
        }, 2000);
      });
    }

    if (crueltyForm) {
      crueltyForm.addEventListener('submit', (e) => {
        e.preventDefault();
        showToast('success', 'Complaint submitted successfully');
        e.target.reset();
      });
    }

    // Donation buttons
    if (confirmDonationBtn) {
      confirmDonationBtn.addEventListener('click', () => {
        const ngoName = donationNgoName.textContent;
        const amount = document.querySelector('.donation-amount-btn.btn-primary').getAttribute('data-amount');
        showToast('success', `Thank you for donating ₹${amount} to ${ngoName}!`);
        donationForm.style.display = 'none';
        donationList.style.display = 'grid';
      });
    }

    if (cancelDonationBtn) {
      cancelDonationBtn.addEventListener('click', () => {
        donationForm.style.display = 'none';
        donationList.style.display = 'grid';
      });
    }

    document.addEventListener('click', (e) => {
      if (e.target.classList.contains('donation-amount-btn')) {
        document.querySelectorAll('.donation-amount-btn').forEach(btn => {
          btn.classList.remove('btn-primary');
          btn.classList.add('btn-outline');
        });
        e.target.classList.remove('btn-outline');
        e.target.classList.add('btn-primary');
      }
    });

    // Initialize
    renderHotelCards();
    renderMessages();
    renderAnimalCards();
    renderDonationCards();

    // Set active nav link
    document.getElementById('nav-home').classList.add('active');

    // Generate random ID card number
    idCardNumber.textContent = Math.floor(Math.random() * 90000) + 10000;
    idCardDate.textContent = new Date().toLocaleDateString();
  </script>
</body>
</html>
