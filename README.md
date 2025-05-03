# dance-picture
<!DOCTYPE html>
<html lang="ru">

<head>
    <meta charset="UTF-8" />
    <title>Поиск Танцевальных Картинок</title>
    <link rel="stylesheet" href="api.css" />
</head>

<body>
    <div class="top-bar">
        <button id="homeBtn">Главная</button>
    </div>

    <!-- Первая надпись -->
    <h1 id="welcomeText" style="display: none;">Добро пожаловать на главную!</h1>

    <!-- Вторая надпись -->
    <p id="infoText" style="display: none;">Здесь вы можете найти картинки танцев разных стилей. Выберите стиль или воспользуйтесь случайным выбором!</p>

    <h1>Dance picture</h1>

    <input type="text" id="customDance" placeholder="Введите стиль танца" />
    <br />

    <select id="danceSelect">
        <option value="">-- Выберите стиль из списка --</option>
        <option value="Ballet">Балет</option>
        <option value="Hip hop dance">Хип-хоп</option>
        <option value="Breakdance">Брейк-данс</option>
        <option value="Salsa dance">Сальса</option>
        <option value="Contemporary dance">Контемп</option>
        <option value="Tango">Танго</option>
        <option value="Waltz">Вальс</option>
        <option value="Flamenco">Фламенко</option>
    </select>
    <br />

    <button onclick="searchImages()">Найти</button>
    <button onclick="randomDance()">Рандом</button>

    <div id="images"></div>

    <div id="pagination" style="display: none;">
        <button onclick="prevPage()">⬅ Назад</button>
        <span id="pageInfo"></span>
        <button onclick="nextPage()">Вперёд ➡</button>
    </div>

    <div id="modal" class="modal">
        <span class="close">&times;</span>
        <img class="modal-content" id="modalImg">
        <div id="caption"></div>
    </div>

    <script src="api.js"></script>
    <script>
        document.getElementById("homeBtn").addEventListener("click", () => {
            // Показываем обе надписи при нажатии на кнопку "Главная"
            document.getElementById("welcomeText").style.display = "block";
            document.getElementById("infoText").style.display = "block";
        });
    </script>
</body>

</html>
