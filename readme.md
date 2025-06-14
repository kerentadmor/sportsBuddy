<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SportsBuddy AI - מוצא מתקני ספורט</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            direction: rtl;
        }

        .chat-container {
            width: 400px;
            height: 700px;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.15);
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        .chat-header {
            background: linear-gradient(135deg, #4CAF50, #45a049);
            color: white;
            padding: 20px;
            text-align: center;
            position: relative;
        }

        .header-title {
            font-size: 20px;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .header-subtitle {
            font-size: 12px;
            opacity: 0.9;
        }

        .new-chat-btn {
            position: absolute;
            top: 15px;
            left: 15px;
            background: rgba(255,255,255,0.2);
            border: 1px solid rgba(255,255,255,0.3);
            color: white;
            padding: 6px 10px;
            border-radius: 15px;
            font-size: 11px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .new-chat-btn:hover {
            background: rgba(255,255,255,0.3);
        }

        .status-badge {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(255,255,255,0.2);
            border: 1px solid rgba(255,255,255,0.3);
            color: white;
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 10px;
        }

        .messages-container {
            flex: 1;
            padding: 15px;
            overflow-y: auto;
            background: #f8f9fa;
        }

        .message {
            margin-bottom: 15px;
            display: flex;
            align-items: flex-start;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .message.bot { justify-content: flex-start; }
        .message.user { justify-content: flex-end; }

        .message-bubble {
            max-width: 80%;
            padding: 12px 16px;
            border-radius: 16px;
            font-size: 14px;
            line-height: 1.4;
            box-shadow: 0 2px 6px rgba(0,0,0,0.1);
        }

        .message.bot .message-bubble {
            background: white;
            border: 1px solid #e1e8ed;
            margin-right: 8px;
        }

        .message.user .message-bubble {
            background: #4CAF50;
            color: white;
            margin-left: 8px;
        }

        .facility-card {
            background: white;
            border: 1px solid #e9ecef;
            border-radius: 12px;
            padding: 15px;
            margin: 10px 0;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }

        .facility-name {
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 8px;
            font-size: 15px;
        }

        .facility-type {
            background: #e8f5e8;
            color: #2e7d2e;
            padding: 3px 8px;
            border-radius: 8px;
            font-size: 11px;
            display: inline-block;
            margin-bottom: 8px;
        }

        .facility-details {
            font-size: 13px;
            color: #666;
            line-height: 1.5;
        }

        .facility-link {
            display: inline-block;
            background: #4CAF50;
            color: white;
            padding: 8px 12px;
            border-radius: 16px;
            text-decoration: none;
            font-size: 12px;
            margin-top: 10px;
            margin-left: 8px;
            transition: background 0.3s;
        }

        .facility-link:hover {
            background: #45a049;
        }

        .typing-indicator {
            display: none;
            padding: 10px 16px;
            background: white;
            border: 1px solid #e1e8ed;
            border-radius: 16px;
            margin-right: 8px;
            max-width: 60px;
        }

        .typing-dots {
            display: flex;
            gap: 4px;
        }

        .typing-dots span {
            width: 6px;
            height: 6px;
            background: #999;
            border-radius: 50%;
            animation: typing 1.4s infinite;
        }

        .typing-dots span:nth-child(2) { animation-delay: 0.2s; }
        .typing-dots span:nth-child(3) { animation-delay: 0.4s; }

        @keyframes typing {
            0%, 60%, 100% { transform: translateY(0); }
            30% { transform: translateY(-8px); }
        }

        .input-container {
            display: flex;
            padding: 15px;
            background: white;
            border-top: 1px solid #e1e8ed;
        }

        .message-input {
            flex: 1;
            border: 1px solid #ddd;
            border-radius: 20px;
            padding: 10px 16px;
            font-size: 14px;
            outline: none;
            transition: border-color 0.3s;
            direction: rtl;
        }

        .message-input:focus {
            border-color: #4CAF50;
        }

        .send-btn {
            background: #4CAF50;
            color: white;
            border: none;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            margin-left: 10px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }

        .send-btn:hover {
            background: #45a049;
            transform: scale(1.05);
        }

        @media (max-width: 450px) {
            .chat-container {
                width: 100%;
                height: 100vh;
                border-radius: 0;
            }
        }

        .loading-indicator {
            text-align: center;
            padding: 20px;
            color: #666;
            font-style: italic;
        }

        .show-more-btn {
            background: #f0f8ff;
            border: 2px solid #4CAF50;
            color: #4CAF50;
            padding: 12px 20px;
            border-radius: 20px;
            font-size: 14px;
            font-weight: bold;
            cursor: pointer;
            display: inline-block;
            margin: 10px 0;
            transition: all 0.3s;
            text-align: center;
            width: 100%;
        }

        .show-more-btn:hover {
            background: #4CAF50;
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(76, 175, 80, 0.3);
        }

        .error-message {
            background: #ffebee;
            color: #c62828;
            padding: 10px;
            border-radius: 8px;
            margin: 10px 0;
            border: 1px solid #ffcdd2;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <div class="chat-header">
            <button class="new-chat-btn" onclick="startNewConversation()">🔄 שיחה חדשה</button>
            <div class="status-badge" id="statusBadge">טוען נתונים...</div>
            <div class="header-title">SportsBuddy AI</div>
            <div class="header-subtitle">מוצא מתקני ספורט בישראל - עם מזג אוויר!</div>
        </div>
        
        <div class="messages-container" id="messagesContainer">
            <div class="message bot">
                <div class="message-bubble">
                    <div class="loading-indicator" id="loadingMessage">
                        🔄 טוען נתוני מתקני ספורט...<br>
                        אנא המתן...
                    </div>
                </div>
            </div>
            
            <div class="typing-indicator" id="typingIndicator">
                <div class="typing-dots">
                    <span></span>
                    <span></span>
                    <span></span>
                </div>
            </div>
        </div>
        
        <div class="input-container">
            <input type="text" class="message-input" id="messageInput" placeholder="כתוב מה אתה מחפש..." onkeypress="handleKeyPress(event)" disabled>
            <button class="send-btn" onclick="sendMessage()" disabled id="sendButton">
                ➤
            </button>
        </div>
    </div>

    <script>
        // Global variables
        let sportsData = [];
        let isDataLoaded = false;
        let dataSource = 'unknown';
        
        // Weather API configuration
        const WEATHER_API_KEY = '0822972733a5c06eb6d8f1e51dc0cec3';
        const WEATHER_API_URL = 'https://api.openweathermap.org/data/2.5/weather';
        
        // Conversation state
        let conversationState = {
            intent: null,
            slots: {
                location: null,
                facility_type: null,
                accessibility: null,
                parking: null,
                lighting: null,
                time: null
            },
            weatherChecked: false,
            weatherData: null,
            searchResults: [],
            currentPage: 0,
            resultsPerPage: 3,
            showMorePerPage: 10
        };

        // Outdoor facilities that need weather check
        const OUTDOOR_FACILITIES = ['כדורגל', 'טניס', 'כדורעף', 'מיני'];

        // Enhanced facility mappings
        const FACILITY_MAPPINGS = {
            'אולם': ['אולם ספורט קטן', 'אולם ספורט בינוני', 'אולם ספורט גדול', 'אולם התעמלות', 'אולם ספורט', 'אולם'],
            'ספורט': ['אולם ספורט', 'מגרש ספורט', 'מתקן ספורט'],
            'התעמלות': ['אולם התעמלות', 'אולם ספורט', 'התעמלות'],
            'בריכה': ['בריכת שחיה - 25X12.5 מ\'', 'בריכת שחיה - 20X50 מ\'', 'בריכת שחיה במידות אחרות', 'בריכת שחיה', 'בריכה', 'בריכת'],
            'שחייה': ['בריכת שחיה - 25X12.5 מ\'', 'בריכת שחיה - 20X50 מ\'', 'בריכת שחיה במידות אחרות', 'בריכת שחיה', 'בריכה', 'שחיה'],
            'שחיה': ['בריכת שחיה - 25X12.5 מ\'', 'בריכת שחיה - 20X50 מ\'', 'בריכת שחיה במידות אחרות', 'בריכת שחיה', 'בריכה', 'שחיה'],
            'כדורגל': ['מגרש כדורגל', 'אצטדיון כדורגל', 'מגרש ספורט משולב', 'כדורגל', 'מגרש דשא'],
            'כדורסל': ['מגרש כדורסל', 'אולם ספורט', 'כדורסל'],
            'טניס': ['מגרש טניס', 'טניס'],
            'כדורעף': ['מגרש חול קבוע לכדורעף', 'מגרש כדורעף', 'כדורעף', 'חול'],
            'כושר': ['מכון לכושר גופני', 'מתקן פתוח לכושר גופני', 'כושר', 'פיטנס'],
            'מיני': ['מגרש מיני פיץ\'', 'מיני'],
            'חול': ['מגרש חול קבוע', 'חול']
        };
        
        // Current date helper
        function getCurrentDate() {
            const now = new Date();
            const days = ['ראשון', 'שני', 'שלישי', 'רביעי', 'חמישי', 'שישי', 'שבת'];
            const dayName = days[now.getDay()];
            const date = now.getDate().toString().padStart(2, '0');
            const month = (now.getMonth() + 1).toString().padStart(2, '0');
            const year = now.getFullYear();
            return `יום ${dayName}, ${date}/${month}/${year}`;
        }

        // Weather API integration with time consideration
        async function getWeatherDataForTime(cityName, requestedTime) {
            try {
                console.log('🌤️ Fetching weather for:', cityName, 'at time:', requestedTime);
                
                // Convert Hebrew city name to English for better API compatibility
                const englishCityName = getEnglishCityName(cityName);
                console.log('🌍 Using English city name:', englishCityName);
                
                const cleanCityName = englishCityName.replace(/[־\-]/, ' ').trim();
                
                // Determine if we need current weather or forecast
                const needsForecast = ['מחר', 'מחרתיים', 'השבוע', 'סוף השבוע'].includes(requestedTime);
                
                if (needsForecast) {
                    // Try to get 5-day forecast for future times
                    const forecastUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${encodeURIComponent(cleanCityName)}&appid=${WEATHER_API_KEY}&units=metric&lang=he`;
                    console.log('🔗 Weather Forecast API URL:', forecastUrl);
                    
                    const response = await fetch(forecastUrl);
                    console.log('📡 Forecast API Response status:', response.status);
                    
                    if (response.ok) {
                        const data = await response.json();
                        console.log('✅ Forecast data received');
                        
                        // Find the most relevant forecast entry for the requested time
                        const relevantForecast = findRelevantForecast(data.list, requestedTime);
                        
                        if (relevantForecast) {
                            return {
                                temp: Math.round(relevantForecast.main.temp),
                                description: relevantForecast.weather[0].description,
                                main: relevantForecast.weather[0].main,
                                cityName: data.city.name,
                                forecastTime: requestedTime,
                                isForecast: true
                            };
                        }
                    }
                }
                
                // Fallback to current weather API
                const currentUrl = `https://api.openweathermap.org/data/2.5/weather?q=${encodeURIComponent(cleanCityName)}&appid=${WEATHER_API_KEY}&units=metric&lang=he`;
                console.log('🔗 Current Weather API URL:', currentUrl);
                
                const response = await fetch(currentUrl);
                console.log('📡 Current API Response status:', response.status);
                
                if (!response.ok) {
                    console.log('❌ Weather API failed:', response.status, response.statusText);
                    
                    try {
                        const errorData = await response.json();
                        console.log('❌ Error details:', errorData);
                    } catch (e) {
                        console.log('❌ Could not parse error response');
                    }
                    
                    return null;
                }
                
                const data = await response.json();
                console.log('✅ Current weather data received:', data);
                
                return {
                    temp: Math.round(data.main.temp),
                    description: data.weather[0].description,
                    main: data.weather[0].main,
                    cityName: data.name,
                    forecastTime: requestedTime,
                    isForecast: false
                };
                
            } catch (error) {
                console.error('💥 Weather API error:', error);
                
                if (error.name === 'TypeError' && error.message.includes('fetch')) {
                    console.error('🚫 Likely CORS error - try running on a web server instead of file://');
                }
                
                return null;
            }
        }

        // Find the most relevant forecast entry for the requested time
        function findRelevantForecast(forecastList, requestedTime) {
            if (!forecastList || forecastList.length === 0) return null;
            
            const now = new Date();
            let targetDate = new Date();
            
            // Calculate target date based on requested time
            switch (requestedTime) {
                case 'מחר':
                    targetDate.setDate(now.getDate() + 1);
                    targetDate.setHours(12); // Default to noon
                    break;
                case 'מחרתיים':
                    targetDate.setDate(now.getDate() + 2);
                    targetDate.setHours(12);
                    break;
                default:
                    // For other times, try to find closest forecast (usually within next 24 hours)
                    targetDate.setHours(now.getHours() + 3); // 3 hours from now as default
                    break;
            }
            
            // Find forecast entry closest to target time
            let closestForecast = forecastList[0];
            let minTimeDiff = Math.abs(new Date(forecastList[0].dt * 1000) - targetDate);
            
            for (const forecast of forecastList) {
                const forecastTime = new Date(forecast.dt * 1000);
                const timeDiff = Math.abs(forecastTime - targetDate);
                
                if (timeDiff < minTimeDiff) {
                    minTimeDiff = timeDiff;
                    closestForecast = forecast;
                }
            }
            
            return closestForecast;
        }

        // Legacy function for backward compatibility
        async function getWeatherData(cityName) {
            return await getWeatherDataForTime(cityName, 'עכשיו');
        }

        // Context-aware weather feedback with time consideration
        function getContextualWeatherFeedback(weatherData) {
            if (!weatherData) return null;
            
            const { temp, main, description, cityName, forecastTime, isForecast } = weatherData;
            
            // Create time-appropriate message header
            let timeContext = '';
            if (isForecast) {
                timeContext = ` ל${getTimeDisplayName(forecastTime)}`;
            } else if (forecastTime && forecastTime !== 'עכשיו') {
                timeContext = ` (תחזית עבור ${getTimeDisplayName(forecastTime)} מבוססת על מזג אוויר נוכחי)`;
            }
            
            let feedback = `🌤️ <strong>מזג האוויר ב${cityName}${timeContext}:</strong> ${temp}°C, ${description}<br><br>`;
            
            // Rain/Storm conditions
            if (main === 'Rain' || main === 'Thunderstorm' || main === 'Snow') {
                if (forecastTime === 'מחר' || forecastTime === 'מחרתיים') {
                    feedback += '🌧️ <strong>צפוי גשם – מומלץ לשקול מתקן ספורט מקורה.</strong><br>';
                } else {
                    feedback += '🌧️ <strong>יורד גשם כרגע – מומלץ לשקול מתקן ספורט מקורה.</strong><br>';
                }
                feedback += 'מתקנים מקורים כמו אולמות ספורט ובריכות יהיו אידיאליים!';
            }
            // Extreme heat
            else if (temp > 32) {
                if (forecastTime && forecastTime !== 'עכשיו') {
                    feedback += '🔥 <strong>צפוי חום קיצוני. מומלץ להיזהר מהשמש, לשתות הרבה מים, ולהתאמן בשעות הקרירות יותר.</strong><br><br>';
                } else {
                    feedback += '🔥 <strong>חום קיצוני כרגע. מומלץ להיזהר מהשמש, לשתות הרבה מים, ולהתאמן בשעות הקרירות יותר.</strong><br><br>';
                }
                feedback += '💡 שקול לחפש מתקן מקורה או לתכנן פעילות בשעות הבוקר המוקדמות!';
            }
            // Very cold
            else if (temp < 10) {
                if (forecastTime && forecastTime !== 'עכשיו') {
                    feedback += '🥶 <strong>צפוי קור - הקפד להתחמם היטב לפני הפעילות ולהלביש שכבות.</strong><br>';
                } else {
                    feedback += '🥶 <strong>קר בחוץ - הקפד להתחמם היטב לפני הפעילות ולהלביש שכבות.</strong><br>';
                }
                feedback += 'מתקנים מקורים יהיו נוחים יותר במזג אוויר כזה.';
            }
            // Perfect weather
            else if (temp >= 18 && temp <= 25) {
                if (forecastTime && forecastTime !== 'עכשיו') {
                    feedback += '☀️ <strong>צפוי מזג אוויר מושלם - אפשר להתאמן בכל מקום!</strong><br>';
                } else {
                    feedback += '☀️ <strong>מזג אוויר מושלם - אפשר להתאמן בכל מקום!</strong><br>';
                }
                feedback += 'תנאים אידיאליים לפעילות בחוץ - מגרשי כדורגל, טניס, או כל פעילות אחרת!';
            }
            // Warm but manageable
            else {
                if (forecastTime && forecastTime !== 'עכשיו') {
                    feedback += '🌤️ <strong>צפוי מזג אוויר נעים לפעילות בחוץ.</strong><br>';
                } else {
                    feedback += '🌤️ <strong>מזג אוויר נעים לפעילות בחוץ.</strong><br>';
                }
                if (temp > 28) {
                    feedback += '💧 רק זכור לשתות מים במהלך הפעילות.';
                } else {
                    feedback += 'תנאים טובים לכל סוגי הפעילויות!';
                }
            }
            
            return feedback;
        }

        // Weather context for search results with time consideration
        function getWeatherContextForResults(weatherData, facilityType, resultsCount) {
            if (!weatherData) return null;
            
            const { temp, main, cityName, forecastTime, isForecast } = weatherData;
            let context = '';
            
            // Create time-appropriate context
            let timePhrase = '';
            if (forecastTime && forecastTime !== 'עכשיו') {
                if (isForecast) {
                    timePhrase = ` ל${getTimeDisplayName(forecastTime)}`;
                } else {
                    timePhrase = ` (תחזית עבור ${getTimeDisplayName(forecastTime)})`;
                }
            }
            
            // Outdoor facilities
            if (OUTDOOR_FACILITIES.includes(facilityType)) {
                if (main === 'Rain' || main === 'Thunderstorm' || main === 'Snow') {
                    if (forecastTime && forecastTime !== 'עכשיו') {
                        context = `🌧️ <strong>לתשומת לבכם:</strong> צפוי גשם ב${cityName}${timePhrase}. המתקנים שמצאתי הם חיצוניים - שקלו לחפש מתקן מקורה במקום.`;
                    } else {
                        context = `🌧️ <strong>לתשומת לבכם:</strong> יורד גשם ב${cityName} כרגע. המתקנים שמצאתי הם חיצוניים - שקלו לחפש מתקן מקורה במקום.`;
                    }
                } else if (temp > 32) {
                    if (forecastTime && forecastTime !== 'עכשיו') {
                        context = `🔥 <strong>חשוב לדעת:</strong> צפוי חום קיצוני ב${cityName}${timePhrase} (${temp}°C). אם תבחרו להתאמן בחוץ, הקפידו לשתות מים, להתאמן בשעות קרירות, ולהגן מהשמש.`;
                    } else {
                        context = `🔥 <strong>חשוב לדעת:</strong> חום קיצוני ב${cityName} (${temp}°C). אם תבחרו להתאמן בחוץ, הקפידו לשתות מים, להתאמן בשעות קרירות, ולהגן מהשמש.`;
                    }
                } else if (temp < 10) {
                    if (forecastTime && forecastTime !== 'עכשיו') {
                        context = `🥶 <strong>טיפ חם:</strong> צפוי קור ב${cityName}${timePhrase} (${temp}°C). הקפידו להתחמם היטב ולהלביש שכבות לפני הפעילות בחוץ.`;
                    } else {
                        context = `🥶 <strong>טיפ חם:</strong> קר ב${cityName} (${temp}°C). הקפידו להתחמם היטב ולהלביש שכבות לפני הפעילות בחוץ.`;
                    }
                } else if (temp >= 18 && temp <= 25) {
                    context = `☀️ <strong>תנאים מושלמים!</strong> מזג אוויר אידיאלי ב${cityName}${timePhrase} (${temp}°C) לפעילות בחוץ. תהנו!`;
                } else if (temp > 28) {
                    context = `🌤️ <strong>תנאים טובים</strong> ב${cityName}${timePhrase} (${temp}°C). זכרו לשתות מים במהלך הפעילות בחוץ.`;
                }
            } 
            // Indoor facilities
            else {
                if (main === 'Rain' || main === 'Thunderstorm' || main === 'Snow') {
                    if (forecastTime && forecastTime !== 'עכשיו') {
                        context = `🌧️ <strong>בחירה מצוינת!</strong> עם הגשם הצפוי ב${cityName}${timePhrase}, מתקן מקורה הוא הפתרון המושלם.`;
                    } else {
                        context = `🌧️ <strong>בחירה מצוינת!</strong> עם הגשם בחוץ ב${cityName}, מתקן מקורה הוא הפתרון המושלם.`;
                    }
                } else if (temp > 32) {
                    if (forecastTime && forecastTime !== 'עכשיו') {
                        context = `🔥 <strong>בחירה חכמה!</strong> עם החום הקיצוני הצפוי ב${cityName}${timePhrase} (${temp}°C), מתקן מקורה ממוזג יהיה הרבה יותר נוח.`;
                    } else {
                        context = `🔥 <strong>בחירה חכמה!</strong> עם החום הקיצוני ב${cityName} (${temp}°C), מתקן מקורה ממוזג יהיה הרבה יותר נוח.`;
                    }
                } else if (temp >= 18 && temp <= 25) {
                    context = `☀️ <strong>גמישות מלאה!</strong> מזג אוויר מושלם ב${cityName}${timePhrase}, אבל מתקן מקורה נותן לכם בקרה מלאה על תנאי ההתאמנות.`;
                }
            }
            
            return context;
        }

        // Context-aware weather feedback for indoor facilities with time consideration
        function getContextualWeatherFeedbackForIndoor(weatherData, facilityType) {
            if (!weatherData) return null;
            
            const { temp, main, description, cityName, forecastTime, isForecast } = weatherData;
            
            // Create time-appropriate message header
            let timeContext = '';
            if (isForecast) {
                timeContext = ` ל${getTimeDisplayName(forecastTime)}`;
            } else if (forecastTime && forecastTime !== 'עכשיו') {
                timeContext = ` (עבור ${getTimeDisplayName(forecastTime)})`;
            }
            
            let feedback = `🌤️ <strong>מזג האוויר ב${cityName}${timeContext}:</strong> ${temp}°C, ${description}<br><br>`;
            
            // Provide context based on weather even for indoor activities
            if (main === 'Rain' || main === 'Thunderstorm' || main === 'Snow') {
                feedback += '🌧️ <strong>בחירה מצוינת!</strong> ';
                if (forecastTime && forecastTime !== 'עכשיו') {
                    feedback += 'עם הגשם הצפוי בחוץ, מתקן מקורה הוא הפתרון המושלם. ';
                } else {
                    feedback += 'עם הגשם בחוץ, מתקן מקורה הוא הפתרון המושלם. ';
                }
                
                if (facilityType === 'בריכה') {
                    feedback += 'בריכה מקורה תאפשר לך לשחות בנוחות גם כשיורד גשם!';
                } else {
                    feedback += 'אולם ספורט מקורה יאפשר לך להתאמן בנוחות למרות מזג האוויר!';
                }
            }
            else if (temp > 32) {
                feedback += '🔥 <strong>בחירה חכמה!</strong> ';
                if (forecastTime && forecastTime !== 'עכשיו') {
                    feedback += 'עם החום הקיצוני הצפוי בחוץ, מתקן מקורה עם מיזוג אוויר יהיה הרבה יותר נוח. ';
                } else {
                    feedback += 'עם החום הקיצוני בחוץ, מתקן מקורה עם מיזוג אוויר יהיה הרבה יותר נוח. ';
                }
                
                if (facilityType === 'בריכה') {
                    feedback += 'בריכה מקורה תאפשר לך להתרענן ולהתאמן בקרירות!';
                } else {
                    feedback += 'אולם ספורט ממוזג יאפשר לך להתאמן בנוחות!';
                }
            }
            else if (temp < 10) {
                feedback += '🥶 <strong>בחירה נבונה!</strong> ';
                if (forecastTime && forecastTime !== 'עכשיו') {
                    feedback += 'עם הקור הצפוי בחוץ, מתקן מקורה וחמים יהיה הרבה יותר נעים להתאמן בו.';
                } else {
                    feedback += 'עם הקור בחוץ, מתקן מקורה וחמים יהיה הרבה יותר נעים להתאמן בו.';
                }
            }
            else if (temp >= 18 && temp <= 25) {
                feedback += '☀️ <strong>מזג אוויר מושלם בחוץ</strong>, אבל מתקן מקורה יתן לך גמישות לא להיות תלוי במזג האוויר. ';
                if (facilityType === 'בריכה') {
                    feedback += 'בריכה תהיה נהדרת בכל מזג אוויר!';
                } else {
                    feedback += 'תוכל להתאמן בנוחות בכל תנאי מזג האוויר!';
                }
            }
            else {
                feedback += '🌤️ <strong>מזג אוויר נעים</strong>, אבל מתקן מקורה נותן יציבות ובקרה על תנאי ההתאמנות.';
            }
            
            return feedback;
        }

        // Enhanced weather-based recommendations
        function getWeatherRecommendation(weatherData, facilityType) {
            if (!weatherData || !OUTDOOR_FACILITIES.includes(facilityType)) {
                return null;
            }
            
            const { temp, main, description } = weatherData;
            let recommendation = '';
            let suggestIndoor = false;
            let precautions = [];
            
            // Rain/Storm conditions
            if (main === 'Rain' || main === 'Thunderstorm' || main === 'Snow') {
                recommendation = `🌧️ כרגע יורד גשם ב${weatherData.cityName} - <strong>לא מומלץ</strong> לפעילות בחוץ!`;
                suggestIndoor = true;
            }
            // Extreme heat
            else if (temp > 32) {
                recommendation = `🔥 <strong>אזהרה: חום קיצוני</strong> ב${weatherData.cityName} (${temp}°C)!`;
                precautions = [
                    '💧 שתו הרבה מים לפני, במהלך ואחרי הפעילות',
                    '🧴 השתמשו בקרם הגנה חזק',
                    '👕 לבשו בגדים בהירים ונושמים',
                    '⏰ העדיפו שעות מוקדמות או מאוחרות'
                ];
                suggestIndoor = true;
            }
            // Very cold
            else if (temp < 5) {
                recommendation = `🥶 קר מאוד בחוץ ב${weatherData.cityName} (${temp}°C)`;
                precautions = [
                    '🧥 הלבישו שכבות חמות',
                    '🧤 אל תשכחו כפפות וכובע',
                    '🏃‍♂️ התחממו היטב לפני הפעילות'
                ];
                suggestIndoor = true;
            }
            // Cold but manageable
            else if (temp < 15) {
                recommendation = `🌡️ קריר בחוץ ב${weatherData.cityName} (${temp}°C)`;
                precautions = [
                    '🧥 הביאו שכבת ביגוד נוספת',
                    '🏃‍♂️ התחממו היטב לפני הפעילות'
                ];
            }
            // Perfect weather
            else if (temp >= 18 && temp <= 25) {
                recommendation = `☀️ מזג אוויר מושלם ב${weatherData.cityName} (${temp}°C) - אידיאלי לפעילות בחוץ!`;
            }
            // Warm but good
            else {
                recommendation = `🌤️ מזג אוויר נעים ב${weatherData.cityName} (${temp}°C) - טוב לפעילות בחוץ`;
                if (temp > 28) {
                    precautions = ['💧 הקפידו לשתות מים'];
                }
            }
            
            return { recommendation, suggestIndoor, precautions };
        }

        // Enhanced JSON loading
        async function loadSportsData() {
            try {
                updateStatusBadge('טוען נתונים...');
                
                const possibleUrls = [
                    './sports_facilities.json',
                    './facilities.json',
                    './data.json'
                ];
                
                for (const url of possibleUrls) {
                    try {
                        const response = await fetch(url);
                        if (response.ok) {
                            const data = await response.json();
                            if (Array.isArray(data) && data.length > 0 && data[0]['ישוב']) {
                                sportsData = data;
                                dataSource = url;
                                isDataLoaded = true;
                                updateStatusBadge(`${sportsData.length} מתקנים`);
                                showSuccessMessage();
                                enableInterface();
                                return;
                            }
                        }
                    } catch (fetchError) {
                        console.log(`❌ ${url} - ${fetchError.message}`);
                    }
                }
                
                loadFallbackData();
            } catch (error) {
                loadFallbackData();
            }
        }

        function loadFallbackData() {
            sportsData = [
                {
                    "ישוב": "תל אביב-יפו",
                    "סוג מתקן": "אולם ספורט קטן – 15x24 מ'",
                    "שם המתקן": "בית הצורי - דוגמה",
                    "שכונה-רובע": "מרכז העיר",
                    "רחוב": "הצורי",
                    "מספר בית": 1,
                    "גוף מפעיל המתקן": "עיריית תל אביב-יפו",
                    "טלפון איש קשר": "03-1234567",
                    "פנוי לפעילות": "בתיאום בלבד",
                    "תאורה קיימת": "כן",
                    "נגישות לנכים": "כן",
                    "חניה לרכבים": "כן",
                    "מצב המתקן": "תקין ופעיל"
                }
            ];
            
            dataSource = 'fallback';
            isDataLoaded = true;
            updateStatusBadge(`${sportsData.length} דוגמה`);
            showFallbackMessage();
            enableInterface();
        }

        function updateStatusBadge(text) {
            document.getElementById('statusBadge').textContent = text;
        }

        function enableInterface() {
            document.getElementById('messageInput').disabled = false;
            document.getElementById('sendButton').disabled = false;
            document.getElementById('messageInput').placeholder = 'כתוב מה אתה מחפש...';
        }

        function showSuccessMessage() {
            const loadingMsg = document.getElementById('loadingMessage');
            if (loadingMsg) {
                loadingMsg.innerHTML = `
                    ✅ <strong>שלום! איך אני יכול לעזור לך היום?</strong><br><br>
                    אני כאן כדי לעזור לך למצוא את מתקן הספורט המושלם בישראל.
                `;
            }
        }

        function showFallbackMessage() {
            const loadingMsg = document.getElementById('loadingMessage');
            if (loadingMsg) {
                loadingMsg.innerHTML = `
                    ⚠️ <strong>שלום! איך אני יכול לעזור לך?</strong><br><br>
                    אני עובד כרגע עם נתוני דוגמה.<br><br>
                    💡 <strong>כדי להשתמש בנתונים המלאים:</strong><br>
                    1. שמור את הקובץ כ-<code>sports_facilities.json</code><br>
                    2. הנח אותו באותה תיקייה<br>
                    3. רענן את הדף
                `;
            }
        }

        // Enhanced intent recognition
        function recognizeIntent(message) {
            const msgLower = message.toLowerCase();
            
            const smallTalkPatterns = {
                greeting: ['שלום', 'היי', 'הי', 'בוקר טוב', 'ערב טוב'],
                thanks: ['תודה', 'תודה רבה', 'thanks'],
                how_are_you: ['מה נשמע', 'איך הולך', 'מה שלומך', 'איך אתה מרגיש', 'איך אתה', 'מה המצב'],
                what_day: ['איזה יום', 'מה היום', 'מה התאריך'],
                capabilities: ['מה אתה יכול', 'מי אתה', 'מה אתה עושה'],
                weather_specific: ['מה מזג האוויר', 'איך מזג האוויר', 'מזג אוויר', 'מזג האוויר היום', 'איך מזג האוויר היום'],
                off_topic: ['פוליטיקה', 'בחירות', 'כלכלה', 'חדשות'],
                show_more: ['עוד', 'הצג עוד', 'עוד מתקנים', 'המשך', 'יותר']
            };
            
            for (const [intent, patterns] of Object.entries(smallTalkPatterns)) {
                if (patterns.some(pattern => msgLower.includes(pattern))) {
                    return intent;
                }
            }
            
            const searchPatterns = ['מחפש', 'רוצה', 'אני רוצה', 'צריך', 'איפה', 'למצוא'];
            const facilityPatterns = ['אולם', 'בריכה', 'מגרש', 'ספורט', 'כדורגל', 'טניס'];
            
            if (searchPatterns.some(p => msgLower.includes(p)) || 
                facilityPatterns.some(p => msgLower.includes(p))) {
                return 'search_facility';
            }
            
            return 'search_facility';
        }

        async function handleSmallTalk(intent, message) {
            const responses = {
                greeting: 'שלום וברכה! 😊 איך אפשר לעזור לך למצוא מתקן ספורט היום?',
                thanks: 'תמיד בשמחה! 🙌 אני כאן אם תצטרך מתקן ספורט כלשהו.',
                how_are_you: 'הכל מצוין, תודה ששאלת! 😊 מוכן לעזור לך למצוא מתקן ספורט נהדר!',
                what_day: `היום ${getCurrentDate()} 📅 יום נהדר לפעילות ספורט! רוצה שאמצא לך מתקן?`,
                capabilities: 'אני צ\'אט בוט שמתמחה בעזרה עם מתקני ספורט בישראל 🤸‍♂️<br>אני יכול לעזור לך למצוא מתקנים ולבדוק מזג אוויר!',
                off_topic: 'זה לא בתחום ההתמחות שלי, אבל אני כאן כדי לעזור לך למצוא מתקני ספורט בישראל! 😊'
            };
            
            // Handle weather-specific queries
            if (intent === 'weather_specific') {
                const location = findLocationInData(message);
                const timeFromMessage = extractTimeFromMessage(message) || 'עכשיו';
                
                if (location) {
                    let weatherMessage = 'רגע אחד, בודק את מזג האוויר';
                    if (timeFromMessage !== 'עכשיו') {
                        weatherMessage += ` ב${location} ל${getTimeDisplayName(timeFromMessage)}`;
                    } else {
                        weatherMessage += ` ב${location}`;
                    }
                    weatherMessage += '... 🌤️';
                    
                    addMessage(weatherMessage, 'bot');
                    showTypingIndicator();
                    
                    const weatherData = await getWeatherDataForTime(location, timeFromMessage);
                    hideTypingIndicator();
                    
                    if (weatherData) {
                        const weatherMessage = getContextualWeatherFeedback(weatherData);
                        addMessage(weatherMessage, 'bot');
                    } else {
                        addMessage(`מצטער, לא הצלחתי לקבל נתוני מזג אוויר עבור ${location} כרגע. 😕<br><br>💡 <strong>אפשרויות:</strong><br>🔄 נסה שוב בעוד כמה דקות<br>🌐 ודא שיש חיבור לאינטרנט<br>📋 בדוק ב-console של הדפדפן (F12) לפרטים נוספים`, 'bot');
                    }
                } else {
                    addMessage('איפה אתה רוצה לבדוק את מזג האוויר? 🌤️<br><br>💬 לדוגמה: "מה מזג האוויר בתל אביב?"', 'bot');
                }
                return null;
            }
            
            return responses[intent] || 'איך אני יכול לעזור לך היום? 😊';
        }

        function findLocationInData(message) {
            if (!sportsData || sportsData.length === 0) return null;
            
            const msgLower = message.toLowerCase();
            const settlements = [...new Set(sportsData.map(f => f['ישוב']).filter(Boolean))];
            
            // Try to find location in data
            for (const settlement of settlements) {
                if (msgLower.includes(settlement.toLowerCase())) {
                    return settlement;
                }
            }
            
            // Enhanced city patterns with English names for weather API
            const cityPatterns = {
                'תל אביב-יפו': ['תל אביב', 'תא', 'tel aviv'],
                'ירושלים': ['ירושלים', 'jerusalem'],
                'חיפה': ['חיפה', 'haifa'],
                'באר שבע': ['באר שבע', 'beer sheva'],
                'פתח תקווה': ['פתח תקווה', 'petah tikva'],
                'נתניה': ['נתניה', 'netanya'],
                'אשדוד': ['אשדוד', 'ashdod'],
                'ראשון לציון': ['ראשון לציון', 'rishon lezion'],
                'חולון': ['חולון', 'holon'],
                'רחובות': ['רחובות', 'rehovot']
            };
            
            for (const [city, patterns] of Object.entries(cityPatterns)) {
                if (patterns.some(pattern => msgLower.includes(pattern))) {
                    return city;
                }
            }
            
            return null;
        }

        // Convert Hebrew city names to English for weather API
        function getEnglishCityName(hebrewCity) {
            const cityTranslations = {
                'תל אביב-יפו': 'Tel Aviv',
                'תל אביב': 'Tel Aviv',
                'ירושלים': 'Jerusalem',
                'חיפה': 'Haifa',
                'באר שבע': 'Beer Sheva',
                'פתח תקווה': 'Petah Tikva',
                'נתניה': 'Netanya',
                'אשדוד': 'Ashdod',
                'ראשון לציון': 'Rishon LeZion',
                'חולון': 'Holon',
                'רחובות': 'Rehovot'
            };
            
            return cityTranslations[hebrewCity] || hebrewCity;
        }

        function findFacilityTypeInData(message) {
            const msgLower = message.toLowerCase();
            
            const searchPatterns = [
                { patterns: ['בריכ', 'שחיי', 'שחיה'], match: 'בריכה' },
                { patterns: ['אולם', 'התעמלות'], match: 'אולם' },
                { patterns: ['כדורגל', 'דשא'], match: 'כדורגל' },
                { patterns: ['כדורסל'], match: 'כדורסל' },
                { patterns: ['טניס'], match: 'טניס' },
                { patterns: ['כדורעף', 'חול'], match: 'כדורעף' },
                { patterns: ['כושר', 'פיטנס'], match: 'כושר' }
            ];
            
            for (const { patterns, match } of searchPatterns) {
                if (patterns.some(pattern => msgLower.includes(pattern))) {
                    return match;
                }
            }
            
            return null;
        }

        // Comprehensive information extraction for first messages
        function extractAllAvailableInfo(message) {
            const msgLower = message.toLowerCase();
            console.log('🔍 Comprehensive extraction from:', msgLower);
            
            const extractedInfo = {
                facility_type: null,
                location: null,
                time: null,
                accessibility: null,
                parking: null,
                lighting: null
            };
            
            // Extract facility type using enhanced keyword matching
            extractedInfo.facility_type = findFacilityTypeInData(message);
            
            // Extract location
            extractedInfo.location = findLocationInData(message);
            
            // Extract time with enhanced patterns
            extractedInfo.time = extractTimeFromMessage(message);
            
            // Extract accessibility needs - be more liberal in first message
            if (msgLower.includes('נגיש') || msgLower.includes('מוגבל') || 
                msgLower.includes('כיסא גלגל') || msgLower.includes('נכים')) {
                extractedInfo.accessibility = 'כן';
            } else if (msgLower.includes('לא נגיש') || msgLower.includes('בלי נגיש')) {
                extractedInfo.accessibility = 'לא';
            }
            
            // Extract parking needs
            if (msgLower.includes('חני') || msgLower.includes('רכב') || 
                msgLower.includes('מקום חניה') || msgLower.includes('פארקינג')) {
                extractedInfo.parking = 'כן';
            } else if (msgLower.includes('בלי חני') || msgLower.includes('לא חני')) {
                extractedInfo.parking = 'לא';
            }
            
            // Extract lighting needs
            if (msgLower.includes('תאור') || msgLower.includes('אור') || 
                msgLower.includes('לילה') || msgLower.includes('בחושך') ||
                msgLower.includes('מואר')) {
                extractedInfo.lighting = 'כן';
            } else if (msgLower.includes('בלי תאור') || msgLower.includes('לא תאור')) {
                extractedInfo.lighting = 'לא';
            }
            
            // Enhanced context detection for implicit requests
            
            // Detect implicit facility requests
            if (!extractedInfo.facility_type) {
                // Look for sport activity mentions that imply facility types
                if (msgLower.includes('לשחות') || msgLower.includes('שחייה')) {
                    extractedInfo.facility_type = 'בריכת שחייה (50X20)';
                } else if (msgLower.includes('לרוץ') || msgLower.includes('ריצה') || msgLower.includes('ג\'וגינג')) {
                    extractedInfo.facility_type = 'מסלול אתלטיקה קלה';
                } else if (msgLower.includes('להתאמן') && !extractedInfo.facility_type) {
                    extractedInfo.facility_type = 'אולם ספורט בינוני';
                }
            }
            
            // Detect implicit time requests
            if (!extractedInfo.time) {
                if (msgLower.includes('דחוף') || msgLower.includes('מיידי') || msgLower.includes('מהר')) {
                    extractedInfo.time = 'עכשיו';
                } else if (msgLower.includes('שבת') || msgLower.includes('סופ"ש') || msgLower.includes('סוף השבוע')) {
                    extractedInfo.time = 'סוף השבוע';
                }
            }
            
            console.log('📝 Comprehensive extraction results:', extractedInfo);
            return extractedInfo;
        }

        function extractEntities(message) {
            const msgLower = message.toLowerCase();
            
            // Extract basic entities for follow-up messages
            const entities = {};
            
            // Only extract if not already set - avoid overriding first message extractions
            if (!conversationState.slots.facility_type) {
                entities.facility_type = findFacilityTypeInData(message);
            }
            
            if (!conversationState.slots.location) {
                entities.location = findLocationInData(message);
            }
            
            // Extract time entities
            if (!conversationState.slots.time) {
                const timeEntity = extractTimeFromMessage(message);
                if (timeEntity) {
                    entities.time = timeEntity;
                }
            }
            
            // Extract yes/no responses based on conversation flow
            const isYesResponse = msgLower.includes('כן') || msgLower.includes('yes') || 
                                msgLower.includes('בוודאי') || msgLower.includes('חשוב לי');
            const isNoResponse = msgLower.includes('לא') || msgLower.includes('no') || 
                               msgLower.includes('לא חשוב') || msgLower.includes('בלי');
            
            // Extract accessibility needs
            if (msgLower.includes('נגיש') || msgLower.includes('מוגבל') || msgLower.includes('כיסא גלגל')) {
                entities.accessibility = 'כן';
            } else if (msgLower.includes('לא נגיש') || msgLower.includes('בלי נגיש')) {
                entities.accessibility = 'לא';
            } else if (conversationState.slots.accessibility === null) {
                if (isYesResponse) entities.accessibility = 'כן';
                if (isNoResponse) entities.accessibility = 'לא';
            }
            
            // Extract parking needs - only if accessibility is already set
            if (msgLower.includes('חני') || msgLower.includes('רכב')) {
                entities.parking = 'כן';
            } else if (msgLower.includes('בלי חני') || msgLower.includes('לא חני')) {
                entities.parking = 'לא';
            } else if (conversationState.slots.parking === null && conversationState.slots.accessibility !== null) {
                if (isYesResponse) entities.parking = 'כן';
                if (isNoResponse) entities.parking = 'לא';
            }
            
            // Extract lighting needs - only if parking is already set
            if (msgLower.includes('תאור') || msgLower.includes('אור') || msgLower.includes('לילה')) {
                entities.lighting = 'כן';
            } else if (msgLower.includes('בלי תאור') || msgLower.includes('לא תאור')) {
                entities.lighting = 'לא';
            } else if (conversationState.slots.lighting === null && conversationState.slots.parking !== null) {
                if (isYesResponse) entities.lighting = 'כן';
                if (isNoResponse) entities.lighting = 'לא';
            }
            
            return entities;
        }

        // Extract time information from user message - Enhanced for first messages
        function extractTimeFromMessage(message) {
            const msgLower = message.toLowerCase();
            
            // Enhanced time patterns in Hebrew for first messages
            const timePatterns = {
                'עכשיו': 'עכשיו',
                'כעת': 'עכשיו', 
                'עכסיו': 'עכשיו',
                'מיידי': 'עכשיו',
                'דחוף': 'עכשיו',
                'מהר': 'עכשיו',
                'היום': 'היום',
                'הלילה': 'לילה',
                'הערב': 'ערב',
                'הבוקר': 'בוקר',
                'מחר': 'מחר',
                'מחרתיים': 'מחרתיים',
                'בערב': 'ערב',
                'בבוקר': 'בוקר',
                'בצהריים': 'צהריים',
                'אחר הצהריים': 'אחר הצהריים',
                'אחרי הצהריים': 'אחר הצהריים',
                'בלילה': 'לילה',
                'השבוע': 'השבוע',
                'סוף השבוע': 'סוף השבוע',
                'סופ"ש': 'סוף השבוע',
                'בסופ"ש': 'סוף השבוע',
                'שבת': 'סוף השבוע',
                'בשבת': 'סוף השבוע',
                'יום שישי': 'סוף השבוע',
                'יום שבת': 'סוף השבוע',
                'בעוד שעה': 'בעוד שעה',
                'בעוד שעתיים': 'בעוד שעתיים',
                'תוך כדי': 'עכשיו',
                'כרגע': 'עכשיו',
                'במהלך היום': 'היום',
                'במהלך הערב': 'ערב',
                'למחר': 'מחר',
                'למחרת': 'מחר'
            };
            
            // Check for specific time patterns
            for (const [pattern, normalized] of Object.entries(timePatterns)) {
                if (msgLower.includes(pattern)) {
                    console.log('⏰ Found time pattern:', pattern, '→', normalized);
                    return normalized;
                }
            }
            
            // Check for hour patterns (e.g., "ב-8", "בשעה 10", "ב8 בבוקר")
            const hourMatches = msgLower.match(/(?:בשעה|ב[־\-]?)(\d{1,2})/);
            if (hourMatches) {
                const hour = parseInt(hourMatches[1]);
                if (hour >= 6 && hour <= 11) return 'בוקר';
                if (hour >= 12 && hour <= 17) return 'צהריים';
                if (hour >= 18 && hour <= 22) return 'ערב';
                if (hour >= 23 || hour <= 5) return 'לילה';
            }
            
            // Check for day-specific patterns
            const dayPatterns = [
                { pattern: /ביום ראשון|יום ראשון/i, value: 'השבוע' },
                { pattern: /ביום שני|יום שני/i, value: 'השבוע' },
                { pattern: /ביום שלישי|יום שלישי/i, value: 'השבוע' },
                { pattern: /ביום רביעי|יום רביעי/i, value: 'השבוע' },
                { pattern: /ביום חמישי|יום חמישי/i, value: 'השבוע' },
                { pattern: /ביום שישי|יום שישי/i, value: 'סוף השבוע' },
                { pattern: /ביום שבת|יום שבת/i, value: 'סוף השבוע' }
            ];
            
            for (const dayPattern of dayPatterns) {
                if (dayPattern.pattern.test(message)) {
                    console.log('📅 Found day pattern:', dayPattern.value);
                    return dayPattern.value;
                }
            }
            
            return null;
        }

        function updateSlots(entities) {
            for (const [key, value] of Object.entries(entities)) {
                if (value && conversationState.slots[key] === null) {
                    conversationState.slots[key] = value;
                }
            }
        }

        function getMissingSlots() {
            const required = ['location', 'facility_type', 'time', 'accessibility', 'parking', 'lighting'];
            return required.filter(slot => conversationState.slots[slot] === null);
        }

        function askForMissingInfo(missingSlots) {
            if (missingSlots.includes('location')) {
                return 'באיזה עיר או יישוב אתה מעוניין? 🏙️';
            }
            if (missingSlots.includes('facility_type')) {
                // Enhanced facility type suggestion with examples
                const availableTypes = getAvailableFacilityTypes();
                let facilityPrompt = 'איזה סוג מתקן ספורט אתה מחפש? 🏃‍♂️<br><br>';
                
                if (availableTypes.length > 0) {
                    facilityPrompt += '💡 <strong>דוגמאות למתקנים זמינים:</strong><br>';
                    // Show most common facility types as examples
                    const examples = availableTypes.slice(0, 6);
                    facilityPrompt += examples.map(type => `• ${type}`).join('<br>');
                    facilityPrompt += '<br><br>💬 פשוט כתוב מה אתה מחפש!';
                }
                
                return facilityPrompt;
            }
            if (missingSlots.includes('time')) {
                return 'מתי אתה רוצה להתאמן? ⏰<br><br>💬 לדוגמה: "עכשיו", "מחר בבוקר", "היום בערב", "בעוד שעה"';
            }
            if (missingSlots.includes('accessibility')) {
                return 'האם יש צורך בנגישות לאנשים עם מוגבלויות? (כן/לא) ♿';
            }
            if (missingSlots.includes('parking')) {
                return 'האם חשוב לך שיהיה חניה? (כן/לא) 🚗';
            }
            if (missingSlots.includes('lighting')) {
                return 'האם יש צורך בתאורה? (כן/לא) 💡';
            }
            return null;
        }

        function filterFacilities() {
            if (!sportsData || sportsData.length === 0) return [];
            
            return sportsData.filter(facility => {
                // Filter by location
                if (conversationState.slots.location) {
                    const targetLocation = conversationState.slots.location.toLowerCase();
                    const facilityLocation = (facility['ישוב'] || '').toLowerCase();
                    if (!facilityLocation.includes(targetLocation)) return false;
                }
                
                // Filter by facility type
                if (conversationState.slots.facility_type) {
                    const targetTypes = FACILITY_MAPPINGS[conversationState.slots.facility_type] || [];
                    const facilityType = (facility['סוג מתקן'] || '').toLowerCase();
                    
                    const matches = targetTypes.some(type => 
                        facilityType.includes(type.toLowerCase())
                    );
                    
                    if (!matches) return false;
                }
                
                // Filter by time availability
                if (conversationState.slots.time) {
                    const availability = facility['פנוי לפעילות'] || '';
                    const timeCompatible = checkTimeCompatibility(conversationState.slots.time, availability);
                    if (!timeCompatible.compatible) return false;
                }
                
                // Filter by accessibility
                if (conversationState.slots.accessibility === 'כן') {
                    const accessibility = (facility['נגישות לנכים'] || '').toLowerCase();
                    if (!accessibility.includes('כן')) return false;
                }
                
                // Filter by parking
                if (conversationState.slots.parking === 'כן') {
                    const parking = (facility['חניה לרכבים'] || '').toLowerCase();
                    if (!parking.includes('כן')) return false;
                }
                
                // Filter by lighting
                if (conversationState.slots.lighting === 'כן') {
                    const lighting = (facility['תאורה קיימת'] || '').toLowerCase();
                    if (!lighting.includes('כן')) return false;
                }
                
                return true;
            });
        }

        // Check if requested time is compatible with facility availability
        function checkTimeCompatibility(requestedTime, facilityAvailability) {
            const availability = facilityAvailability.toLowerCase();
            
            // Handle "בתיאום בלבד" or similar appointment-only cases
            if (availability.includes('בתיאום') || availability.includes('תיאום')) {
                return { 
                    compatible: true, 
                    note: 'זמין בתיאום מראש בלבד' 
                };
            }
            
            // Handle "פתוח לקהל" - always available
            if (availability.includes('פתוח לקהל') || availability.includes('פתוח')) {
                return { compatible: true, note: null };
            }
            
            // Handle specific time restrictions
            const timeMap = {
                'בוקר': ['בוקר', 'שעות הבוקר', '06:', '07:', '08:', '09:', '10:', '11:'],
                'צהריים': ['צהריים', 'אחר הצהריים', '12:', '13:', '14:', '15:', '16:', '17:'],
                'ערב': ['ערב', 'שעות הערב', '18:', '19:', '20:', '21:', '22:'],
                'לילה': ['לילה', 'שעות הלילה', '23:', '00:', '01:', '02:', '03:', '04:', '05:']
            };
            
            // Check if facility specifies time restrictions
            if (availability.includes('רק') || availability.includes('אחר הצהריים') || 
                availability.includes('בוקר') || availability.includes('ערב')) {
                
                const requestedTimeCategory = requestedTime;
                const patterns = timeMap[requestedTimeCategory] || [requestedTimeCategory];
                
                // Check if any pattern matches the facility availability
                const hasMatch = patterns.some(pattern => availability.includes(pattern));
                
                if (!hasMatch) {
                    // Check opposite restrictions
                    if (availability.includes('רק בוקר') && ['ערב', 'לילה', 'צהריים'].includes(requestedTimeCategory)) {
                        return { 
                            compatible: false, 
                            note: `המתקן פתוח רק בשעות הבוקר, לא זמין ${getTimeDisplayName(requestedTimeCategory)}` 
                        };
                    }
                    if (availability.includes('רק ערב') && ['בוקר', 'צהריים'].includes(requestedTimeCategory)) {
                        return { 
                            compatible: false, 
                            note: `המתקן פתוח רק בשעות הערב, לא זמין ${getTimeDisplayName(requestedTimeCategory)}` 
                        };
                    }
                    if (availability.includes('אחר הצהריים') && requestedTimeCategory === 'בוקר') {
                        return { 
                            compatible: false, 
                            note: 'המתקן פתוח רק אחר הצהריים, לא זמין בבוקר' 
                        };
                    }
                }
            }
            
            // Default: assume compatible
            return { compatible: true, note: null };
        }

        function getTimeDisplayName(timeCategory) {
            const names = {
                'בוקר': 'בבוקר',
                'צהריים': 'בצהריים', 
                'אחר הצהריים': 'אחר הצהריים',
                'ערב': 'בערב',
                'לילה': 'בלילה',
                'עכשיו': 'כעת',
                'היום': 'היום',
                'מחר': 'מחר',
                'מחרתיים': 'מחרתיים',
                'השבוע': 'השבוע',
                'סוף השבוע': 'בסוף השבוע',
                'בעוד שעה': 'בעוד שעה',
                'בעוד שעתיים': 'בעוד שעתיים'
            };
            return names[timeCategory] || timeCategory;
        }

        // Pagination functions
        function displayPaginatedResults(facilities, startFromBeginning = true) {
            if (startFromBeginning) {
                conversationState.searchResults = facilities;
                conversationState.currentPage = 0;
            }
            
            // For first page: show 3 results. For subsequent pages: show 10 results
            const isFirstPage = conversationState.currentPage === 0;
            const pageSize = isFirstPage ? conversationState.resultsPerPage : conversationState.showMorePerPage;
            
            const startIndex = isFirstPage ? 0 : conversationState.resultsPerPage + (conversationState.currentPage - 1) * conversationState.showMorePerPage;
            const endIndex = startIndex + pageSize;
            const currentPageResults = conversationState.searchResults.slice(startIndex, endIndex);
            
            if (currentPageResults.length === 0) {
                addMessage('זה כל המתקנים שמצאתי! 😊', 'bot');
                return;
            }
            
            // Show results message only on first page
            if (conversationState.currentPage === 0) {
                const totalResults = conversationState.searchResults.length;
                let resultsMessage = totalResults === 1 ? 
                    '🎯 מצאתי את המתקן המושלם בשבילך:' :
                    `🎯 מצאתי ${totalResults} מתקנים שמתאימים לבקשתך. מציג ${Math.min(3, totalResults)} ראשונים:`;
                addMessage(resultsMessage, 'bot');
            }
            
            // Display current page results
            currentPageResults.forEach((facility, index) => {
                setTimeout(() => {
                    addMessage(createFacilityCard(facility), 'bot');
                }, index * 200);
            });
            
            // Calculate remaining results correctly
            const remainingResults = conversationState.searchResults.length - endIndex;
            
            // Show "more results" button only if there are actually more results
            if (remainingResults > 0) {
                setTimeout(() => {
                    const showMoreButton = `
                        <div class="show-more-btn" onclick="showMoreResults()">
                            👉 הצג עוד ${Math.min(remainingResults, conversationState.showMorePerPage)} מתקנים 🔍
                        </div>
                        <div style="text-align: center; font-size: 12px; color: #666; margin-top: 5px;">
                            עוד ${remainingResults} מתקנים זמינים
                        </div>
                    `;
                    addMessage(showMoreButton, 'bot');
                }, currentPageResults.length * 200 + 500);
            }
            
            conversationState.currentPage++;
        }

        function showMoreResults() {
            displayPaginatedResults(null, false);
        }

        function createFacilityCard(facility) {
            const facilityName = facility['שם המתקן'] || 'שם לא זמין';
            const facilityType = facility['סוג מתקן'] || 'סוג לא צוין';
            const city = facility['ישוב'] || 'עיר לא צוינה';
            const phone = facility['טלפון איש קשר'];
            const availability = facility['פנוי לפעילות'] || 'לא צוין';
            
            // Only show phone if it exists and is not "לא קיים"
            const hasValidPhone = phone && phone !== 'לא קיים' && phone.trim() !== '';
            
            // Check time compatibility for display
            let timeNote = '';
            if (conversationState.slots.time) {
                const timeCheck = checkTimeCompatibility(conversationState.slots.time, availability);
                if (timeCheck.note) {
                    timeNote = `<br>⏰ ${timeCheck.note}`;
                }
            }
            
            const googleMapsUrl = `https://www.google.com/maps/search/${encodeURIComponent(facilityName + ', ' + city)}`;
            
            return `<div class="facility-card">
                <div class="facility-name">${facilityName}</div>
                <div class="facility-type">${facilityType}</div>
                <div class="facility-details">
                    📍 <strong>${city}</strong><br>
                    ${hasValidPhone ? '📞 ' + phone + '<br>' : ''}
                    🕐 <strong>זמינות:</strong> ${availability}${timeNote}
                </div>
                <a href="${googleMapsUrl}" target="_blank" class="facility-link">🗺️ מפות גוגל</a>
                ${hasValidPhone ? `<a href="tel:${phone}" class="facility-link">📞 התקשר</a>` : ''}
            </div>`;
        }

        async function processMessage(message) {
            if (!isDataLoaded) {
                addMessage('⚠️ נתוני המתקנים עדיין נטענים. אנא המתן...', 'bot');
                return;
            }
            
            showTypingIndicator();
            
            try {
                const intent = recognizeIntent(message);
                hideTypingIndicator();
                
                // Handle small talk
                if (['greeting', 'thanks', 'how_are_you', 'what_day', 'capabilities', 'weather_specific', 'off_topic'].includes(intent)) {
                    const response = await handleSmallTalk(intent, message);
                    if (response) {
                        addMessage(response, 'bot');
                    }
                    return;
                }
                
                // Handle show more results
                if (intent === 'show_more') {
                    if (conversationState.searchResults.length > 0) {
                        showMoreResults();
                    } else {
                        addMessage('אין עוד תוצאות להציג. בוא נחפש משהו חדש! 😊', 'bot');
                    }
                    return;
                }
                
                // Handle search facility
                if (intent === 'search_facility') {
                    if (conversationState.intent === null) {
                        conversationState.intent = 'search_facility';
                    }
                    
                    // Smart analysis - extract everything immediately
                    const entities = extractEntities(message);
                    updateSlots(entities);
                    
                    const hasBasicInfo = conversationState.slots.location && conversationState.slots.facility_type;
                    
                    // Weather check for outdoor facilities
                    if (hasBasicInfo && !conversationState.weatherChecked && 
                        OUTDOOR_FACILITIES.includes(conversationState.slots.facility_type)) {
                        
                        addMessage('רגע אחד, בודק את מזג האוויר... 🌤️', 'bot');
                        showTypingIndicator();
                        
                        const weatherData = await getWeatherData(conversationState.slots.location);
                        conversationState.weatherData = weatherData;
                        conversationState.weatherChecked = true;
                        
                        hideTypingIndicator();
                        
                        if (weatherData) {
                            // Use BOTH context-aware feedback AND recommendations
                            const contextualFeedback = getContextualWeatherFeedback(weatherData);
                            const weatherRec = getWeatherRecommendation(weatherData, conversationState.slots.facility_type);
                            
                            if (contextualFeedback) {
                                addMessage(contextualFeedback, 'bot');
                            }
                            
                            if (weatherRec && weatherRec.suggestIndoor) {
                                addMessage('💡 **המלצה:** רוצה שאחפש לך מתקן מקורה במקום? (אולם ספורט, מתקן כושר, וכו\') 🏢', 'bot');
                                return;
                            }
                        }
                    }
                    
                    // Also add context-aware feedback when we have location but indoor facility
                    if (hasBasicInfo && !conversationState.weatherChecked && 
                        !OUTDOOR_FACILITIES.includes(conversationState.slots.facility_type)) {
                        
                        addMessage('רגע אחד, בודק את מזג האוויר... 🌤️', 'bot');
                        showTypingIndicator();
                        
                        const weatherData = await getWeatherData(conversationState.slots.location);
                        conversationState.weatherData = weatherData;
                        conversationState.weatherChecked = true;
                        
                        hideTypingIndicator();
                        
                        if (weatherData) {
                            const contextualFeedback = getContextualWeatherFeedbackForIndoor(weatherData, conversationState.slots.facility_type);
                            if (contextualFeedback) {
                                addMessage(contextualFeedback, 'bot');
                            }
                        }
                    }
                    
                    // Handle weather suggestion responses
                    if (conversationState.weatherChecked && message.toLowerCase().includes('מקורה')) {
                        conversationState.slots.facility_type = 'אולם';
                        addMessage('מעולה! מחפש לך מתקן מקורה... 🏢', 'bot');
                    }
                    
                    const missingSlots = getMissingSlots();
                    
                    // If we still need basic info, ask for it
                    if (missingSlots.length > 0) {
                        const question = askForMissingInfo(missingSlots);
                        if (question) {
                            addMessage(question, 'bot');
                            return;
                        }
                    }
                    
                    // We have all info - perform search
                    const facilities = filterFacilities();
                    
                    if (facilities.length > 0) {
                        // Add weather context to results if we have weather data
                        if (conversationState.weatherData && conversationState.weatherChecked) {
                            const contextualSummary = getWeatherContextForResults(
                                conversationState.weatherData, 
                                conversationState.slots.facility_type,
                                facilities.length
                            );
                            if (contextualSummary) {
                                addMessage(contextualSummary, 'bot');
                            }
                        }
                        
                        // Use pagination to display results
                        displayPaginatedResults(facilities, true);
                    } else {
                        addMessage('לא מצאתי מתקנים שמתאימים לבקשתך. 😔<br><br>💡 נסה לשנות את הקריטריונים או לכתוב בקשה אחרת.', 'bot');
                    }
                    
                    // Reset conversation state
                    conversationState = {
                        intent: null,
                        slots: {
                            location: null,
                            facility_type: null,
                            accessibility: null,
                            parking: null,
                            lighting: null,
                            time: null
                        },
                        weatherChecked: false,
                        weatherData: null,
                        searchResults: [],
                        currentPage: 0,
                        resultsPerPage: 3,
                        showMorePerPage: 10
                    };
                }
                
            } catch (error) {
                hideTypingIndicator();
                console.error('Error processing message:', error);
                addMessage('⚠️ מצטער, אירעה שגיאה. אנא נסה שוב.', 'bot');
            }
        }

        // UI functions
        function handleKeyPress(event) {
            if (event.key === 'Enter' && !event.shiftKey) {
                event.preventDefault();
                sendMessage();
            }
        }

        function sendMessage() {
            const input = document.getElementById('messageInput');
            const message = input.value.trim();
            
            if (message && isDataLoaded) {
                addMessage(message, 'user');
                input.value = '';
                setTimeout(() => processMessage(message), 500);
            }
        }

        function addMessage(content, sender) {
            const messagesContainer = document.getElementById('messagesContainer');
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${sender}`;
            
            const bubbleDiv = document.createElement('div');
            bubbleDiv.className = 'message-bubble';
            bubbleDiv.innerHTML = content;
            
            messageDiv.appendChild(bubbleDiv);
            messagesContainer.insertBefore(messageDiv, document.getElementById('typingIndicator'));
            messagesContainer.scrollTop = messagesContainer.scrollHeight;
        }

        function showTypingIndicator() {
            document.getElementById('typingIndicator').style.display = 'block';
            const messagesContainer = document.getElementById('messagesContainer');
            messagesContainer.scrollTop = messagesContainer.scrollHeight;
        }

        function hideTypingIndicator() {
            document.getElementById('typingIndicator').style.display = 'none';
        }

        function startNewConversation() {
            const messagesContainer = document.getElementById('messagesContainer');
            const typingIndicator = document.getElementById('typingIndicator');
            
            while (messagesContainer.firstChild && messagesContainer.firstChild !== typingIndicator) {
                messagesContainer.removeChild(messagesContainer.firstChild);
            }
            
            conversationState = {
                intent: null,
                slots: {
                    location: null,
                    facility_type: null,
                    accessibility: null,
                    parking: null,
                    lighting: null,
                    time: null
                },
                weatherChecked: false,
                weatherData: null,
                searchResults: [],
                currentPage: 0,
                resultsPerPage: 3,
                showMorePerPage: 10
            };
            
            setTimeout(() => {
                addMessage('שלום! איך אני יכול לעזור לך היום? 😊<br><br>אני כאן כדי לעזור לך למצוא את מתקן הספורט המושלם בישראל.', 'bot');
            }, 500);
        }

        // Initialize the application
        document.addEventListener('DOMContentLoaded', async function() {
            console.log('🚀 SportsBuddy AI - Initializing...');
            await loadSportsData();
        });

        // Make functions globally accessible
        window.handleKeyPress = handleKeyPress;
        window.sendMessage = sendMessage;
        window.startNewConversation = startNewConversation;
        window.showMoreResults = showMoreResults;
    </script>
</body>
</html>