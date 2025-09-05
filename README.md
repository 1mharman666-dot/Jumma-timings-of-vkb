# Jumma-timings-of-vkb
Namaz and azan timings of jumma in Vikarabad local masjids 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vikarabad Prayer Timings</title>
    <!-- Use the Inter font from Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(to bottom, #d6e2f0, #e9f0f7);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 1rem;
        }
        .app-container {
            background-color: #ffffff;
            border-radius: 1.5rem;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            max-width: 600px;
            width: 100%;
            padding: 2rem;
            display: flex;
            flex-direction: column;
            gap: 2rem;
        }
        .header {
            text-align: center;
            color: #334155;
        }
        .header h1 {
            font-size: 2.25rem;
            font-weight: 700;
            margin-bottom: 0.5rem;
        }
        .header p {
            font-size: 1.125rem;
            color: #64748b;
        }
        .timing-card {
            background-color: #f8fafc;
            border-radius: 1rem;
            padding: 1.5rem;
            box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.06);
        }
        .timing-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
            font-size: 1.125rem;
            padding: 1rem 0;
            border-bottom: 1px solid #e2e8f0;
        }
        .timing-item:last-child {
            border-bottom: none;
        }
        .hadith-section {
            background-color: #e0f2f1;
            border: 2px solid #2dd4bf;
            border-radius: 1.5rem;
            padding: 2rem;
            text-align: center;
            font-style: italic;
            color: #0f766e;
        }
        @media (min-width: 640px) {
            .timing-item {
                flex-direction: row;
                justify-content: space-between;
                text-align: left;
            }
        }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen">

    <div class="app-container">

        <!-- Header Section -->
        <header class="header">
            <h1 class="text-3xl md:text-4xl font-extrabold text-gray-800">Vikarabad Prayer Timings</h1>
            <p class="text-lg md:text-xl text-gray-600">Jumma Azan & Namaz Timings for Local Masjids</p>
            <div id="date-time" class="mt-4 text-gray-500 text-sm md:text-base"></div>
        </header>

        <!-- Timings Section -->
        <section class="timing-card">
            <h2 class="text-2xl font-semibold text-gray-700 mb-4 text-center">Jumma Prayer Timings</h2>
            <div id="timings-list">
                <!-- Timings will be populated by JavaScript -->
            </div>
            <p class="mt-4 text-sm text-gray-500 text-center">Note: Timings can vary. Please verify with the local mosques for precise schedules.</p>
        </section>

        <!-- Hadith Section -->
        <section class="hadith-section">
            <h3 class="text-xl font-bold mb-4">A Hadith of the Prophet (PBUH)</h3>
            <p id="hadith-text" class="text-lg leading-relaxed"></p>
        </section>

    </div>

    <script>
        // Data for prayer timings. This data is read-only and not editable.
        // It is based on available search results, but local verification is recommended.
        const prayerTimings = [
            { name: "Jama Masjid Vikarabad", azan: "1:15 PM", namaz: "1:30 PM" },
            { name: "Masjid-E-Noor", azan: "1:15 PM", namaz: "1:30 PM" },
            { name: "Masjid E-Quba", azan: "1:15 PM", namaz: "1:30 PM" },
            { name: "Jamia Masjid Ahle Sunnath Val Jamaath", azan: "1:15 PM", namaz: "1:30 PM" },
            { name: "Masjid E Mohammadia", azan: "1:15 PM", namaz: "1:30 PM" },
            { name: "Masjid E Abubakar", azan: "1:15 PM", namaz: "1:30 PM" },
            { name: "Masjid E Arafat", azan: "1:15 PM", namaz: "1:30 PM" },
            { name: "Masjid-e-Hasan", azan: "1:15 PM", namaz: "1:30 PM" },
            { name: "Masjid E Osman Bin Affan", azan: "1:15 PM", namaz: "1:30 PM" }
        ];

        // A collection of Hadith related to prayer and faith.
        const hadiths = [
            "The Prophet (peace and blessings of Allah be upon him) said: 'The key to Paradise is prayer, and the key to prayer is purification.' - Hadith by Imam Ahmad",
            "The Prophet (PBUH) said: 'When the call to prayer is made, Satan turns his back and flees, passing wind so as not to hear it.' - Sahih al-Bukhari",
            "The Prophet (PBUH) said: 'Whoever prays Fajr, he is under the protection of Allah.' - Sahih Muslim",
            "The Messenger of Allah (PBUH) said: 'The head of the matter is Islam, its pillar is the prayer, and the highest part of its hump is Jihad.' - At-Tirmidhi"
        ];

        // Function to display current date and time
        function updateDateTime() {
            const now = new Date();
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            const dateString = now.toLocaleDateString('en-US', options);
            const timeString = now.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit', second: '2-digit' });
            document.getElementById('date-time').textContent = `${dateString} | ${timeString}`;
        }

        // Function to render timings to the page
        function renderTimings() {
            const timingsList = document.getElementById('timings-list');
            timingsList.innerHTML = '';
            prayerTimings.forEach(prayer => {
                const item = document.createElement('div');
                item.className = 'timing-item';
                item.innerHTML = `
                    <span class="font-bold text-lg text-gray-800 mb-2 md:mb-0">${prayer.name}</span>
                    <span class="flex flex-col md:flex-row items-center gap-2 text-gray-600 text-base md:text-sm">
                        <span>Azan: ${prayer.azan}</span>
                        <span>Namaz: ${prayer.namaz}</span>
                    </span>
                `;
                timingsList.appendChild(item);
            });
        }

        // Function to display a random Hadith
        function displayRandomHadith() {
            const hadithText = document.getElementById('hadith-text');
            const randomIndex = Math.floor(Math.random() * hadiths.length);
            hadithText.textContent = hadiths[randomIndex];
        }

        // Initial render and setup
        document.addEventListener('DOMContentLoaded', () => {
            updateDateTime();
            setInterval(updateDateTime, 1000);
            renderTimings();
            displayRandomHadith();
        });
    </script>
</body>
</html>
