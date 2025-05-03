# dance-picture
//html
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

//css
</body>

</html>

* {
    box-sizing: border-box;
}

body {
    text-align: center;
    padding: 50px;
    font-family: 'Arial', sans-serif;
    background: linear-gradient(135deg, #fce4ec 0%, #f8bbd0 100%);
}

.top-bar {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    background-color: rgba(255, 255, 255, 0.8);
    backdrop-filter: blur(5px);
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding: 10px 20px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
    z-index: 1000;
}

.top-bar button {
    background-color: #ff7e5f;
    color: white;
    border: none;
    border-radius: 8px;
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
}

.top-bar button:hover {
    background-color: #feb47b;
}

h1 {
    font-size: 36px;
    margin-bottom: 20px;
}

input,
select,
button {
    padding: 10px;
    font-size: 18px;
    margin: 10px;
    border-radius: 8px;
    border: 2px solid #555;
}

button {
    background-color: #ff7e5f;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover {
    background-color: #feb47b;
}

#images {
    margin-top: 30px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
}

#images>div {
    width: 360px;
}

img {
    width: 350px;
    height: 350px;
    object-fit: cover;
    border-radius: 15px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
    transition: transform 0.3s;
}

img:hover {
    transform: scale(1.05);
}

#pagination {
    margin-top: 20px;
    font-size: 18px;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 15px;
}

.modal {
    display: none;
    position: fixed;
    z-index: 999;
    padding-top: 60px;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0, 0, 0, 0.8);
}

.modal-content {
    margin: auto;
    display: block;
    max-width: 90%;
    max-height: 80vh;
    border-radius: 15px;
}

#caption {
    text-align: center;
    color: #fff;
    padding: 15px;
    font-size: 18px;
}

.close {
    position: absolute;
    top: 30px;
    right: 45px;
    color: #fff;
    font-size: 40px;
    font-weight: bold;
    cursor: pointer;
}

//js
let currentPage = 1;
let currentQuery = "";
const perPage = 9;

