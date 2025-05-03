# song_finder
love music? use this site to find what you want!
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Поиск песен</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="container">
    <h1>Найди любую песню</h1>
    <input type="text" id="searchInput" placeholder="Введите название песни..." />
    <button onclick="searchSong()">Найти</button>
    <div id="results"></div>
  </div>
  <script src="script.js"></script>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  background: linear-gradient(to bottom, #007BFF, #B0E0E6);
  margin: 0;
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.container {
  background-color: white;
  padding: 40px;
  border-radius: 12px;
  box-shadow: 0 8px 16px rgba(0,0,0,0.2);
  text-align: center;
  width: 90%;
  max-width: 400px;
}

input {
  width: 100%;
  padding: 12px;
  margin: 12px 0;
  border: 1px solid #007BFF;
  border-radius: 6px;
}

button {
  padding: 10px 20px;
  background-color: #007BFF;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
}

button:hover {
  background-color: #0056b3;
}

#results {
  margin-top: 20px;
  text-align: left;
}
function searchSong() {
  const query = document.getElementById("searchInput").value.trim();
  const results = document.getElementById("results");

  if (!query) {
    results.innerHTML = "<p>Пожалуйста, введите название песни.</p>";
    return;
  }

  results.innerHTML = "<p>Загрузка результатов...</p>";

  fetch(`https://api.deezer.com/search?q=${encodeURIComponent(query)}&output=jsonp&limit=5`, {
    method: 'GET',
    mode: 'cors'
  })
    .then(response => response.text())
    .then(data => {
      // Deezer API возвращает JSONP, нужно вырезать JSON из обертки
      const json = JSON.parse(data.replace(/^.+?\(/, '').replace(/\);$/, ''));

      if (!json.data || json.data.length === 0) {
        results.innerHTML = "<p>Песни не найдены.</p>";
        return;
      }

      results.innerHTML = "<h3>Результаты поиска:</h3><ul>";

      json.data.forEach(track => {
        results.innerHTML += `
          <li>
            <strong>${track.title}</strong> — ${track.artist.name}  
            <br><a href="${track.link}" target="_blank">Слушать</a>
          </li>`;
      });

      results.innerHTML += "</ul>";
    })
    .catch(error => {
      results.innerHTML = "<p>Ошибка при поиске песен.</p>";
      console.error(error);
    });
}
