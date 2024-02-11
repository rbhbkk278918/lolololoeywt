

<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Открытие сайта на текущей странице</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        label {
            display: block;
            margin-bottom: 10px;
        }
        input {
            width: calc(100% - 20px);
            padding: 8px;
            margin-bottom: 10px;
            box-sizing: border-box;
        }
        button {
            display: inline-block;
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
            margin-top: 10px;
            transition: background-color 0.3s ease-in-out, color 0.3s ease-in-out, transform 0.1s ease-in-out;
        }
        button:hover {
            background-color: #45a049;
        }
        button:active {
            transform: translateY(1px);
            box-shadow: none;
        }
        iframe {
            width: 100%;
            height: 500px;
            border: 1px solid #ccc;
            transition: height 0.5s ease-in-out;
        }
        .error {
            color: red;
            margin-top: 10px;
            font-weight: bold;
        }
        p a {
            color: #333;
            text-decoration: none;
            margin-right: 10px;
            padding: 8px 15px;
            border: 2px solid #333;
            border-radius: 5px;
            transition: background-color 0.3s ease-in-out, color 0.3s ease-in-out;
        }
        p a:hover {
            background-color: #333;
            color: #fff;
        }
        h2 {
            font-size: 1.5em;
            margin-bottom: 10px;
            color: #333;
        }
        main {
            border-top: 1px solid #ccc;
            padding-top: 20px;
            margin-top: 20px;
        }
        footer {
            text-align: center;
            padding: 20px;
            background-color: #f4f4f4;
            color: #666;
        }
        footer .error {
            color: red;
            font-weight: bold;
        }
        .controls {
            margin-top: 10px;
        }
    </style>
</head>
<body>

    <label for="websiteInput">Введите ссылку на сайт:</label>
    <input type="url" id="websiteInput" required placeholder="Введите ссылку на сайт">
    <button onclick="pasteFromClipboard('websiteInput')" aria-label="Вставить">Вставить</button>
    <button onclick="clearInput('websiteInput')" aria-label="Очистить">Очистить</button>
    <button onclick="openWebsite()" aria-label="Открыть сайт">Открыть сайт</button>

    <div id="error" class="error" aria-live="polite" role="alert"></div>
    
    <iframe id="websiteFrame" frameborder="0"></iframe>

    <h2>Реклама</h2>
    <div id="advertisement">
        <!-- Ваш код или содержимое рекламы -->
        <img src="advertisement_image.jpg" alt="Реклама">
    </div>

    <h2>Видео</h2>
    <input type="url" id="videoInput" placeholder="Введите ссылку на видео" onkeypress="handleVideoKeyPress(event)" aria-labelledby="videoError">
    <button onclick="pasteFromClipboard('videoInput')" aria-label="Вставить">Вставить</button>
    <button onclick="clearInput('videoInput')" aria-label="Очистить">Очистить</button>
    <button onclick="showVideo()" aria-label="Показать видео">Показать видео</button>

    <div id="videoContainer"></div>
    <div id="videoError" class="error" role="alert" aria-live="polite"></div>

    <div class="controls">
        <button onclick="playVideo()" aria-label="Воспроизвести">Воспроизвести</button>
        <button onclick="pauseVideo()" aria-label="Пауза">Пауза</button>
        <button onclick="stopVideo()" aria-label="Стоп">Стоп</button>
        <label for="seekInput">Перемотка (в секундах):</label>
        <input type="number" id="seekInput" placeholder="Введите время">
        <button onclick="seekVideo()" aria-label="Перемотать">Перемотать</button>
    </div>

    <script>
        function openWebsite() {
            var websiteUrl = document.getElementById('websiteInput').value;
            var errorDiv = document.getElementById('error');

            if (websiteUrl.trim() !== '') {
                // Очищаем сообщение об ошибке
                errorDiv.textContent = '';

                // Устанавливаем URL в iframe
                document.getElementById('websiteFrame').src = websiteUrl;
            } else {
                errorDiv.textContent = 'Пожалуйста, введите ссылку на сайт.';
            }
        }

        function pasteFromClipboard(inputId) {
            var inputElement = document.getElementById(inputId);
            navigator.clipboard.readText().then(function (clipboardText) {
                inputElement.value = clipboardText;
            });
        }

        function clearInput(inputId) {
            var inputElement = document.getElementById(inputId);
            inputElement.value = '';
        }

        function showVideo() {
            var videoUrl = document.getElementById('videoInput').value;
            var videoContainer = document.getElementById('videoContainer');
            var videoError = document.getElementById('videoError');

            // Очищаем сообщение об ошибке
            videoError.textContent = '';

            if (videoUrl.trim() !== '') {
                // Создаем элемент видео
                var video = document.createElement('video');
                video.width = "100%";
                video.height = "auto";
                video.controls = true;
                video.id = "mainVideo";

                // Создаем источник видео
                var source = document.createElement('source');
                source.src = videoUrl;
                source.type = 'video/mp4'; // Укажите подходящий тип видео

                // Добавляем источник к видео
                video.appendChild(source);

                // Очищаем контейнер и добавляем видео
                videoContainer.innerHTML = '';
                videoContainer.appendChild(video);
            } else {
                // Отображаем сообщение об ошибке, если URL не введен
                videoError.textContent = 'Пожалуйста, введите ссылку на видео.';
            }
        }

        function handleVideoKeyPress(event) {
            if (event.key === 'Enter') {
                showVideo();
            }
        }

        function playVideo() {
            var video = document.getElementById('mainVideo');
            if (video) {
                video.play();
            }
        }

        function pauseVideo() {
            var video = document.getElementById('mainVideo');
            if (video) {
                video.pause();
            }
        }

        function stopVideo() {
            var video = document.getElementById('mainVideo');
            if (video) {
                video.pause();
                video.currentTime = 0;
            }
        }

        function seekVideo() {
            var video = document.getElementById('mainVideo');
            var seekInput = document.getElementById('seekInput');
            if (video && seekInput.value.trim() !== '') {
                video.currentTime = parseFloat(seekInput.value);
            }
        }
    </script>

    <footer>
        <h2>Контакты</h2>
        <p>&copy; 2024 Разработчик Dylan9332789Z Все права защищены.</p>
    </footer>

</body>
</html>
