<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Real-Time Search</title>
    <style>
        body {
            background-color: #000;
            color: #fff;
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .search-container {
            width: 80%;
            max-width: 600px;
            margin: 50px auto;
        }

        #searchInput {
            width: 100%;
            padding: 15px;
            font-size: 16px;
            border: 2px solid #fff;
            border-radius: 25px;
            background-color: #000;
            color: #fff;
            outline: none;
        }

        #results {
            margin-top: 20px;
        }

        .result-item {
            background-color: #111;
            padding: 15px;
            margin-bottom: 10px;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        .result-item:hover {
            background-color: #222;
        }

        .result-title {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .result-snippet {
            font-size: 14px;
            color: #ccc;
        }

        .error {
            color: #ff4444;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="search-container">
        <input type="text" id="searchInput" placeholder="Search the web...">
        <div id="results"></div>
        <div id="error" class="error"></div>
    </div>

    <script>
        const searchInput = document.getElementById('searchInput');
        const resultsDiv = document.getElementById('results');
        const errorDiv = document.getElementById('error');

        // Debounce function to limit API calls
        function debounce(func, timeout = 300) {
            let timer;
            return (...args) => {
                clearTimeout(timer);
                timer = setTimeout(() => { func.apply(this, args); }, timeout);
            };
        }

        async function searchWikipedia(query) {
            if (!query) {
                resultsDiv.innerHTML = '';
                errorDiv.textContent = '';
                return;
            }

            try {
                const response = await fetch(
                    `https://en.wikipedia
