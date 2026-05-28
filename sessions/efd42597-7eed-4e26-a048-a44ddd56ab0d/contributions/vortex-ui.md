# DELIVERABLE

## FILE 1: index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Weather Dashboard</title>
  <style>
    /* ===== RESET & VARIABLES ===== */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    :root {
      --color-primary: #1e88e5;
      --color-primary-dark: #1565c0;
      --color-bg: #f5f5f5;
      --color-bg-card: #ffffff;
      --color-text: #212121;
      --color-text-secondary: #616161;
      --color-border: #e0e0e0;
      --color-error: #d32f2f;
      --color-success: #388e3c;
      
      --spacing-xs: 0.5rem;
      --spacing-sm: 1rem;
      --spacing-md: 1.5rem;
      --spacing-lg: 2rem;
      
      --radius: 8px;
      --shadow-sm: 0 2px 4px rgba(0, 0, 0, 0.1);
      --shadow-md: 0 4px 8px rgba(0, 0, 0, 0.15);
      
      --font-size-sm: 0.875rem;
      --font-size-base: 1rem;
      --font-size-lg: 1.25rem;
      --font-size-xl: 1.5rem;
      --font-size-2xl: 2rem;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
      background-color: var(--color-bg);
      color: var(--color-text);
      line-height: 1.6;
    }

    main {
      max-width: 1200px;
      margin: 0 auto;
      padding: var(--spacing-md);
    }

    /* ===== ACCESSIBILITY ===== */
    .sr-only {
      position: absolute;
      width: 1px;
      height: 1px;
      padding: 0;
      margin: -1px;
      overflow: hidden;
      clip: rect(0, 0, 0, 0);
      white-space: nowrap;
      border-width: 0;
    }

    a:focus,
    button:focus,
    input:focus {
      outline: 3px solid var(--color-primary);
      outline-offset: 2px;
    }

    /* ===== SEARCH SECTION ===== */
    .search-section {
      margin-bottom: var(--spacing-lg);
    }

    .search-form {
      display: flex;
      gap: var(--spacing-sm);
      flex-wrap: wrap;
      align-items: flex-start;
    }

    .search-input {
      flex: 1;
      min-width: 200px;
      padding: var(--spacing-sm);
      font-size: var(--font-size-base);
      border: 2px solid var(--color-border);
      border-radius: var(--radius);
      background-color: var(--color-bg-card);
      color: var(--color-text);
      transition: border-color 200ms ease;
    }

    .search-input:hover {
      border-color: var(--color-primary);
    }

    .search-input:focus {
      border-color: var(--color-primary);
      outline: none;
      box-shadow: 0 0 0 3px rgba(30, 136, 229, 0.1);
    }

    .search-button {
      padding: var(--spacing-sm) var(--spacing-md);
      font-size: var(--font-size-base);
      font-weight: 600;
      background-color: var(--color-primary);
      color: #ffffff;
      border: none;
      border-radius: var(--radius);
      cursor: pointer;
      transition: background-color 200ms ease;
    }

    .search-button:hover {
      background-color: var(--color-primary-dark);
    }

    .search-button:active {
      transform: scale(0.98);
    }

    .search-status {
      margin-top: var(--spacing-sm);
      font-size: var(--font-size-sm);
      color: var(--color-text-secondary);
      min-height: 1.25rem;
      width: 100%;
    }

    /* ===== WEATHER CARD (CURRENT) ===== */
    .weather-card {
      background-color: var(--color-bg-card);
      border-radius: var(--radius);
      box-shadow: var(--shadow-md);
      padding: var(--spacing-md);
      margin-bottom: var(--spacing-lg);
    }

    .weather-header {
      border-bottom: 2px solid var(--color-border);
      padding-bottom: var(--spacing-md);
      margin-bottom: var(--spacing-md);
    }

    .location-name {
      font-size: var(--font-size-2xl);
      font-weight: 700;
      color: var(--color-text);
      margin-bottom: var(--spacing-xs);
    }

    .timezone-label {
      font-size: var(--font-size-sm);
      color: var(--color-text-secondary);
      font-style: italic;
    }

    /* ===== METRICS LIST (DEFINITION LIST) ===== */
    .metrics-list {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: var(--spacing-md);
    }

    @media (max-width: 640px) {
      .metrics-list {
        grid-template-columns: 1fr;
      }
    }

    .metric-item {
      display: contents;
    }

    .metric-label {
      font-weight: 600;
      color: var(--color-text-secondary);
      font-size: var(--font-size-sm);
      text-transform: uppercase;
      letter-spacing: 0.5px;
    }

    .metric-value {
      font-size: var(--font-size-lg);
      font-weight: 500;
      color: var(--color-text);
      display: flex;
      align-items: center;
      gap: var(--spacing-xs);
    }

    .temperature-value {
      font-size: var(--font-size-2xl);
      font-weight: 700;
    }

    .weather-condition {
      justify-content: flex-start;
    }

    .condition-emoji {
      font-size: var(--font-size-2xl);
      line-height: 1;
    }

    .condition-label {
      font-weight: 600;
    }

    /* ===== FORECAST GRID (5-DAY) ===== */
    .forecast-section {
      margin-bottom: var(--spacing-lg);
    }

    .forecast-heading {
      font-size: var(--font-size-xl);
      font-weight: 700;
      margin-bottom: var(--spacing-md);
      color: var(--color-text);
    }

    .forecast-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
      gap: var(--spacing-sm);
      list-style: none;
    }

    @media (max-width: 320px) {
      .forecast-grid {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    .forecast-item {
      background-color: var(--color-bg-card);
      border-radius: var(--radius);
      box-shadow: var(--shadow-sm);
      padding: var(--spacing-sm);
      min-height: 180px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      border-left: 4px solid var(--color-primary);
    }

    .forecast-day {
      font-weight: 700;
      font-size: var(--font-size-base);
      color: var(--color-text);
    }

    .forecast-date {
      font-size: var(--font-size-sm);
      color: var(--color-text-secondary);
      margin-bottom: var(--spacing-xs);
    }

    .forecast-condition {
      display: flex;
      align-items: center;
      gap: var(--spacing-xs);
      margin: var(--spacing-sm) 0;
    }

    .forecast-emoji {
      font-size: var(--font-size-2xl);
      line-height: 1;
    }

    .forecast-label {
      font-size: var(--font-size-sm);
      font-weight: 600;
      color: var(--color-text);
    }

    .forecast-temps {
      display: flex;
      gap: var(--spacing-md);
      font-weight: 600;
      margin-top: var(--spacing-xs);
    }

    .temp-max {
      color: var(--color-primary);
    }

    .temp-min {
      color: var(--color-text-secondary);
    }

    /* ===== ERROR SECTION ===== */
    .error-container {
      background-color: #ffebee;
      border-left: 4px solid var(--color-error);
      border-radius: var(--radius);
      padding: var(--spacing-md);
      margin-bottom: var(--spacing-lg);
    }

    .error-message {
      color: var(--color-error);
      font-weight: 500;
    }

    /* ===== RESPONSIVE ===== */
    @media (max-width: 640px) {
      main {
        padding: var(--spacing-sm);
      }

      .search-form {
        flex-direction: column;
      }

      .search-input {
        min-width: 100%;
      }

      .search-button {
        width: 100%;
      }

      .location-name {
        font-size: var(--font-size-xl);
      }
    }
  </style>
</head>
<body>
  <main role="main" aria-label="Weather Dashboard Application">
    
    <!-- SEARCH SECTION -->
    <section class="search-section" aria-labelledby="search-heading">
      <h1 id="search-heading" class="sr-only">Weather Search</h1>
      <form id="search-form" class="search-form" aria-label="Search for a city">
        <label for="city-input" class="sr-only">City Name</label>
        <input 
          id="city-input"
          type="search"
          class="search-input"
          placeholder="Enter city name (e.g., London, Paris)"
          list="city-suggestions"
          autocomplete="off"
          aria-describedby="search-status"
          aria-label="City search input"
          required
        />
        <datalist id="city-suggestions">
          <!-- Dynamically populated by JavaScript from geocodingResults -->
        </datalist>
        <button type="submit" class="search-button" aria-label="Search weather for entered city">
          Search
        </button>
      </form>
      <div id="search-status" class="search-status" role="status" aria-live="polite" aria-atomic="true">
        <!-- "Loading cities..." or status message injected here -->
      </div>
    </section>

    <!-- CURRENT WEATHER SECTION -->
    <article id="current-weather" class="weather-card current-weather" aria-label="Current Weather Conditions" hidden>
      <div class="weather-header">
        <h2 id="location-name" class="location-name"><!-- City name injected here --></h2>
        <p id="timezone-label" class="timezone-label"><!-- e.g., "Weather for London (GMT)" --></p>
      </div>
      
      <div class="current-metrics">
        <dl class="metrics-list">
          
          <!-- Temperature -->
          <div class="metric-item">
            <dt class="metric-label">Temperature</dt>
            <dd class="metric-value temperature-value" id="temp-value" aria-label="Current temperature">
              <span class="value-text">--</span>°C
            </dd>
          </div>

          <!-- Weather Condition -->
          <div class="metric-item">
            <dt class="metric-label">Condition</dt>
            <dd class="metric-value weather-condition" id="weather-condition" aria-label="Current weather condition">
              <span class="condition-emoji" id="condition-emoji" aria-hidden="true">--</span>
              <span class="condition-label" id="condition-label">--</span>
            </dd>
          </div>

          <!-- Humidity -->
          <div class="metric-item">
            <dt class="metric-label">Humidity</dt>
            <dd class="metric-value humidity-value" id="humidity-value" aria-label="Relative humidity">
              <span class="value-text">--</span>%
            </dd>
          </div>

          <!-- Wind Speed -->
          <div class="metric-item">
            <dt class="metric-label">Wind Speed</dt>
            <dd class="metric-value wind-value" id="wind-value" aria-label="Wind speed">
              <span class="value-text">--</span> km/h
            </dd>
          </div>

        </dl>
      </div>
    </article>

    <!-- 5-DAY FORECAST SECTION -->
    <section id="forecast-section" class="forecast-section" aria-labelledby="forecast-heading" hidden>
      <h2 id="forecast-heading" class="forecast-heading">5-Day Forecast</h2>
      <ul id="forecast-grid" role="list" class="forecast-grid">
        <!-- Populated dynamically: each <li> = one day forecast card -->
      </ul>
    </section>

    <!-- ERROR/STATUS SECTION -->
    <aside id="error-container" class="error-container" role="status" aria-live="assertive" aria-atomic="true" hidden>
      <p id="error-message" class="error-message"><!-- Error text injected here --></p>
    </aside>

  </main>

  <script>
    // ============================================
    // WEATHER DASHBOARD - COMPLETE IMPLEMENTATION
    // ============================================

    // ===== STATE MANAGEMENT (Single Source of Truth) =====
    const AppState = {
      searchQuery: '',
      geocodingResults: [],        // Array of { name, country, latitude, longitude, population }
      selectedCity: null,          // User-selected result: { name, latitude, longitude }
      currentWeather: null,        // { temperature, humidity, weatherCode, windSpeed, timezone }
      forecast: null,              // Array of 5 days: { date, dayOfWeek, maxTemp, minTemp, weatherCode }
      loadingGeocoding: false,     // True while geocoding API call in flight
      loadingWeather: false,       // True while weather API call in flight
      error: null,                 // Error message string
      errorType: null,             // 'geocoding' | 'weather' | null
    };

    // ===== WMO WEATHER CODE MAPPING (ISO 4677 Standard) =====
    const WMO_CODES = {
      0: { label: 'Clear Sky', emoji: '☀️' },
      1: { label: 'Mainly Clear', emoji: '🌤️' },
      2: { label: 'Partly Cloudy', emoji