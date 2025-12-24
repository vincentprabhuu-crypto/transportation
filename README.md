# transportation
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Public Transport Crowd Prediction</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
            background: linear-gradient(135deg, #EFF6FF 0%, #E0E7FF 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        /* Header */
        .header {
            text-align: center;
            margin-bottom: 48px;
        }

        .header-icons {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            margin-bottom: 16px;
        }

        .header h1 {
            font-size: 2.5rem;
            color: #312E81;
            font-weight: 700;
            margin-bottom: 12px;
        }

        .header p {
            color: #4B5563;
            font-size: 1.125rem;
            max-width: 800px;
            margin: 0 auto;
        }

        .icon {
            width: 40px;
            height: 40px;
            color: #4F46E5;
        }

        /* Grid Layout */
        .grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 32px;
        }

        @media (min-width: 1024px) {
            .grid {
                grid-template-columns: 1fr 1fr;
            }
        }

        /* Card */
        .card {
            background: white;
            border-radius: 16px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            padding: 32px;
        }

        .card h2 {
            font-size: 1.5rem;
            font-weight: 700;
            color: #111827;
            margin-bottom: 24px;
        }

        .card h3 {
            font-size: 1.25rem;
            font-weight: 700;
            color: #111827;
            margin-bottom: 24px;
        }

        /* Form */
        .form-group {
            margin-bottom: 24px;
        }

        .form-label {
            display: block;
            color: #374151;
            font-weight: 600;
            margin-bottom: 12px;
            font-size: 0.875rem;
        }

        .label-with-icon {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .transport-buttons {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 16px;
        }

        .transport-btn {
            padding: 16px;
            border-radius: 12px;
            border: 2px solid #E5E7EB;
            background: white;
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            font-size: 1rem;
            font-weight: 500;
        }

        .transport-btn:hover {
            border-color: #9CA3AF;
        }

        .transport-btn.active {
            border-color: #4F46E5;
            background: #EEF2FF;
            color: #4338CA;
        }

        .form-select,
        .form-input {
            width: 100%;
            padding: 12px;
            border: 2px solid #E5E7EB;
            border-radius: 12px;
            font-size: 1rem;
            transition: border-color 0.2s;
            background: white;
        }

        .form-select:focus,
        .form-input:focus {
            outline: none;
            border-color: #4F46E5;
        }

        .submit-btn {
            width: 100%;
            background: #4F46E5;
            color: white;
            padding: 14px 24px;
            border: none;
            border-radius: 12px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.2s;
        }

        .submit-btn:hover {
            background: #4338CA;
        }

        /* Empty State */
        .empty-state {
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 400px;
            text-align: center;
        }

        .empty-state svg {
            width: 64px;
            height: 64px;
            opacity: 0.5;
            margin-bottom: 16px;
            color: #9CA3AF;
        }

        .empty-state p {
            color: #9CA3AF;
            font-size: 1.125rem;
        }

        /* Results */
        .hidden {
            display: none;
        }

        .crowd-card {
            padding: 24px;
            border-radius: 12px;
            border: 2px solid;
            margin-bottom: 24px;
        }

        .crowd-card.empty {
            background: #F0FDF4;
            border-color: #BBF7D0;
        }

        .crowd-card.normal {
            background: #FFFBEB;
            border-color: #FDE68A;
        }

        .crowd-card.overcrowded {
            background: #FEF2F2;
            border-color: #FECACA;
        }

        .crowd-header {
            display: flex;
            align-items: center;
            gap: 12px;
            margin-bottom: 16px;
        }

        .crowd-icon {
            font-size: 2rem;
        }

        .crowd-text p:first-child {
            color: #6B7280;
            font-size: 0.875rem;
        }

        .crowd-label {
            font-size: 1.5rem;
            font-weight: 700;
        }

        .crowd-label.empty {
            color: #15803D;
        }

        .crowd-label.normal {
            color: #A16207;
        }

        .crowd-label.overcrowded {
            color: #B91C1C;
        }

        .occupancy-section {
            margin-top: 16px;
        }

        .occupancy-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            font-size: 0.875rem;
        }

        .occupancy-bar-container {
            width: 100%;
            height: 12px;
            background: #E5E7EB;
            border-radius: 9999px;
            overflow: hidden;
        }

        .occupancy-bar {
            height: 100%;
            transition: width 0.5s ease;
        }

        .occupancy-bar.empty {
            background: #22C55E;
        }

        .occupancy-bar.normal {
            background: #EAB308;
        }

        .occupancy-bar.overcrowded {
            background: #EF4444;
        }

        .details-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 16px;
            margin-bottom: 24px;
        }

        .detail-box {
            background: #F9FAFB;
            border-radius: 12px;
            padding: 16px;
        }

        .detail-header {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 8px;
            font-size: 0.75rem;
            color: #6B7280;
        }

        .detail-value {
            color: #111827;
            font-weight: 600;
            font-size: 0.875rem;
        }

        .recommendation {
            background: #EEF2FF;
            border: 1px solid #C7D2FE;
            border-radius: 12px;
            padding: 16px;
        }

        .recommendation-title {
            color: #312E81;
            font-weight: 700;
            margin-bottom: 8px;
        }

        .recommendation-text {
            color: #4338CA;
            font-size: 0.875rem;
            line-height: 1.5;
        }

        /* Chart */
        .chart-container {
            margin-top: 32px;
        }

        .chart-canvas {
            width: 100%;
            height: 300px;
            margin-bottom: 24px;
        }

        .legend {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 24px;
            flex-wrap: wrap;
            font-size: 0.875rem;
        }

        .legend-item {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .legend-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
        }

        .legend-dot.empty {
            background: #22C55E;
        }

        .legend-dot.normal {
            background: #EAB308;
        }

        .legend-dot.overcrowded {
            background: #EF4444;
        }

        .legend-text {
            color: #6B7280;
        }

        .small-icon {
            width: 16px;
            height: 16px;
        }

        @media (max-width: 768px) {
            .header h1 {
                font-size: 1.75rem;
            }

            .header p {
                font-size: 1rem;
            }

            .details-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <div class="header">
            <div class="header-icons">
                <svg class="icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 20l-5.447-2.724A1 1 0 013 16.382V5.618a1 1 0 011.447-.894L9 7m0 13l6-3m-6 3V7m6 10l4.553 2.276A1 1 0 0021 18.382V7.618a1 1 0 00-.553-.894L15 4m0 13V4m0 0L9 7"/>
                </svg>
                <h1>Public Transport Crowd Prediction</h1>
                <svg class="icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7h12m0 0l-4-4m4 4l-4 4m0 6H4m0 0l4 4m-4-4l4-4"/>
                </svg>
            </div>
            <p>Plan your journey better by checking real-time crowd predictions for buses and trains</p>
        </div>

        <!-- Main Grid -->
        <div class="grid">
            <!-- Left Column - Form -->
            <div>
                <div class="card">
                    <h2>Check Crowd Levels</h2>
                    
                    <form id="predictionForm">
                        <!-- Transport Type -->
                        <div class="form-group">
                            <label class="form-label">Transport Type</label>
                            <div class="transport-buttons">
                                <button type="button" id="busBtn" class="transport-btn active">
                                    <svg class="small-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7h12m0 0l-4-4m4 4l-4 4m0 6H4m0 0l4 4m-4-4l4-4"/>
                                    </svg>
                                    <span>Bus</span>
                                </button>
                                <button type="button" id="trainBtn" class="transport-btn">
                                    <svg class="small-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 20l-5.447-2.724A1 1 0 013 16.382V5.618a1 1 0 011.447-.894L9 7m0 13l6-3m-6 3V7m6 10l4.553 2.276A1 1 0 0021 18.382V7.618a1 1 0 00-.553-.894L15 4m0 13V4m0 0L9 7"/>
                                    </svg>
                                    <span>Train</span>
                                </button>
                            </div>
                        </div>

                        <!-- Route Selection -->
                        <div class="form-group">
                            <label class="form-label" id="routeLabel">Select Bus Route</label>
                            <select id="routeSelect" class="form-select" required>
                                <option value="">Choose a route...</option>
                            </select>
                        </div>

                        <!-- Day Selection -->
                        <div class="form-group">
                            <label class="form-label label-with-icon">
                                <svg class="small-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
                                </svg>
                                Day of Week
                            </label>
                            <select id="daySelect" class="form-select">
                                <option value="Monday">Monday</option>
                                <option value="Tuesday">Tuesday</option>
                                <option value="Wednesday">Wednesday</option>
                                <option value="Thursday">Thursday</option>
                                <option value="Friday">Friday</option>
                                <option value="Saturday">Saturday</option>
                                <option value="Sunday">Sunday</option>
                            </select>
                        </div>

                        <!-- Time Selection -->
                        <div class="form-group">
                            <label class="form-label label-with-icon">
                                <svg class="small-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"/>
                                </svg>
                                Time
                            </label>
                            <input type="time" id="timeInput" value="09:00" class="form-input" required />
                        </div>

                        <!-- Submit Button -->
                        <button type="submit" class="submit-btn">Get Prediction</button>
                    </form>
                </div>
            </div>

            <!-- Right Column - Results -->
            <div>
                <!-- Empty State -->
                <div id="emptyState" class="card empty-state">
                    <div>
                        <svg fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 20l-5.447-2.724A1 1 0 013 16.382V5.618a1 1 0 011.447-.894L9 7m0 13l6-3m-6 3V7m6 10l4.553 2.276A1 1 0 0021 18.382V7.618a1 1 0 00-.553-.894L15 4m0 13V4m0 0L9 7"/>
                        </svg>
                        <p>Select your route and time to see predictions</p>
                    </div>
                </div>

                <!-- Prediction Results -->
                <div id="resultContainer" class="hidden">
                    <div class="card">
                        <h2>Prediction Result</h2>

                        <!-- Crowd Level Card -->
                        <div id="crowdCard" class="crowd-card">
                            <div class="crowd-header">
                                <span id="crowdIcon" class="crowd-icon"></span>
                                <div class="crowd-text">
                                    <p>Crowd Level</p>
                                    <p id="crowdLabel" class="crowd-label"></p>
                                </div>
                            </div>

                            <!-- Occupancy Bar -->
                            <div class="occupancy-section">
                                <div class="occupancy-header">
                                    <span>Estimated Occupancy</span>
                                    <span id="occupancyPercent"></span>
                                </div>
                                <div class="occupancy-bar-container">
                                    <div id="occupancyBar" class="occupancy-bar"></div>
                                </div>
                            </div>
                        </div>

                        <!-- Details Grid -->
                        <div class="details-grid">
                            <div class="detail-box">
                                <div class="detail-header">
                                    <svg class="small-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"/>
                                    </svg>
                                    Route
                                </div>
                                <div id="routeDetail" class="detail-value"></div>
                            </div>

                            <div class="detail-box">
                                <div class="detail-header">
                                    <svg class="small-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"/>
                                    </svg>
                                    Day
                                </div>
                                <div id="dayDetail" class="detail-value"></div>
                            </div>

                            <div class="detail-box">
                                <div class="detail-header">
                                    <span>%</span>
                                    Confidence
                                </div>
                                <div id="confidenceDetail" class="detail-value"></div>
                            </div>

                            <div class="detail-box">
                                <div class="detail-header">
                                    <span>⏰</span>
                                    Time
                                </div>
                                <div id="timeDetail" class="detail-value"></div>
                            </div>
                        </div>

                        <!-- Recommendation -->
                        <div class="recommendation">
                            <div class="recommendation-title">💡 Recommendation</div>
                            <div id="recommendation" class="recommendation-text"></div>
                        </div>
                    </div>

                    <!-- Chart -->
                    <div class="card chart-container">
                        <h3>Daily Crowd Trends - <span id="chartDay"></span></h3>
                        <canvas id="crowdChart" class="chart-canvas"></canvas>

                        <!-- Legend -->
                        <div class="legend">
                            <div class="legend-item">
                                <div class="legend-dot empty"></div>
                                <span class="legend-text">Empty (&lt;30%)</span>
                            </div>
                            <div class="legend-item">
                                <div class="legend-dot normal"></div>
                                <span class="legend-text">Normal (30-75%)</span>
                            </div>
                            <div class="legend-item">
                                <div class="legend-dot overcrowded"></div>
                                <span class="legend-text">Overcrowded (&gt;75%)</span>
                            </div>
                        </div>
                    </div>
    