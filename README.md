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

function createModal(imageUrl, altText) {
    const modal = document.getElementById("modal");
    const modalImg = document.getElementById("modalImg");
    const caption = document.getElementById("caption");
    modal.style.display = "block";
    modalImg.src = imageUrl;
    caption.innerText = altText;

    document.querySelector(".close").onclick = () => {
        modal.style.display = "none";
    };

    window.onclick = function(event) {
        if (event.target === modal) {
            modal.style.display = "none";
        }
    };
}

async function searchImages(randomOne = false, page = 1) {
    const select = document.getElementById("danceSelect");
    const input = document.getElementById("customDance");
    const query = input.value || select.value;

    if (!query) {
        alert("Введите или выберите стиль танца!");
        return;
    }

    currentQuery = query;
    currentPage = page;

    const count = randomOne ? 1 : perPage;
    const apiKey = "F_vHuLfFcEBDIbzCeh06HkfAC4EWaEcSQsUinVzU-E8";
    const url = `https://api.unsplash.com/search/photos?query=${encodeURIComponent(query)}&client_id=${apiKey}&per_page=${count}&page=${page}`;

    try {
        const response = await fetch(url);
        const data = await response.json();
        const imagesDiv = document.getElementById("images");
        imagesDiv.innerHTML = "";

        if (data.results.length === 0) {
            imagesDiv.innerHTML = "<p>Ничего не найдено 😢</p>";
            document.getElementById("pagination").style.display = "none";
            return;
        }

        data.results.forEach((photo) => {
            const container = document.createElement("div");
            container.classList.add("image-card");
            container.style.padding = "10px";
            container.style.border = "1px solid #ddd";
            container.style.borderRadius = "10px";
            container.style.background = "#f9f9f9";

            const img = document.createElement("img");
            img.src = photo.urls.small;
            img.alt = photo.alt_description || "dance image";
            img.style.cursor = "pointer";
            img.onclick = () => createModal(photo.urls.regular, img.alt);

            const info = document.createElement("div");
            info.style.marginTop = "8px";
            info.innerHTML = ` 
                <strong>Автор:</strong> <a href="${photo.user.links.html}" target="_blank">${photo.user.name}</a><br>
                <strong>Лайки:</strong> ❤️ ${photo.likes}<br>
                <strong>Дата:</strong> ${new Date(photo.created_at).toLocaleDateString()}<br>
                <strong>Описание:</strong> ${photo.description || photo.alt_description || "—"}<br>
                <strong>Ссылка:</strong> <a href="${photo.links.html}" target="_blank">Открыть на Unsplash</a><br><br>
            `;

            const openBtn = document.createElement("button");
            openBtn.innerText = "Открыть";
            openBtn.style.padding = "5px 10px";
            openBtn.style.border = "none";
            openBtn.style.background = "#007bff";
            openBtn.style.color = "white";
            openBtn.style.borderRadius = "5px";
            openBtn.style.cursor = "pointer";
            openBtn.onclick = () => createModal(photo.urls.regular, img.alt);

            container.appendChild(img);
            container.appendChild(info);
            container.appendChild(openBtn);
            imagesDiv.appendChild(container);
        });

        document.getElementById("pagination").style.display = !randomOne ? "flex" : "none";
        document.getElementById("pageInfo").innerText = `Страница ${currentPage}`;
    } catch (error) {
        console.error("Ошибка при загрузке:", error);
        alert("Что-то пошло не так. Попробуйте позже.");
    }
}

function randomDance() {
    const dances = [
        "Ballet",
        "Hip hop dance",
        "Breakdance",
        "Salsa dance",
        "Contemporary dance",
        "Tango",
        "Waltz",
        "Flamenco",
    ];
    const random = dances[Math.floor(Math.random() * dances.length)];
    document.getElementById("danceSelect").value = random;
    document.getElementById("customDance").value = "";
    searchImages(true);
}


