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
