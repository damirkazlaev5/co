<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Простая соцсеть</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'Arial', sans-serif;
      background-color: #f5f7fa;
      color: #2d3436;
      line-height: 1.5;
    }
    .container {
      max-width: 600px;
      margin: 20px auto;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #0984e3;
      margin-bottom: 20px;
      font-size: 24px;
    }
    .input-section {
      background: white;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
      margin-bottom: 20px;
    }
    #userName, #message {
      width: 100%;
      padding: 12px;
      margin: 8px 0;
      border: 1px solid #dfe4ea;
      border-radius: 4px;
      font-size: 14px;
    }
    button {
      background-color: #0984e3;
      color: white;
      border: none;
      padding: 12px 20px;
      border-radius: 4px;
      cursor: pointer;
      font-size: 14px;
    }
    button:hover { background-color: #076aab; }
    .posts-section {
      background: white;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
      padding: 15px;
    }
    .post {
      padding: 12px;
      border-bottom: 1px solid #eaeaea;
    }
    .post:last-child { border-bottom: none; }
    .post-author {
      font-weight: bold;
      color: #0984e3;
      font-size: 14px;
    }
    .post-date {
      font-size: 12px;
      color: #636e72;
      margin: 4px 0 8px 0;
    }
    .post-content {
      color: #2d3436;
      line-height: 1.4;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Моя соцсеть</h1>

    <!-- Форма для ввода -->
    <div class="input-section">
      <input type="text" id="userName" placeholder="Ваше имя" value="Гость">
      <textarea id="message" rows="3" placeholder="Что хотите сказать?"></textarea>
      <button onclick="addPost()">Опубликовать</button>
    </div>

    <!-- Лента сообщений -->
    <div class="posts-section" id="postsContainer">
      <!-- Здесь будут посты -->
    </div>
  </div>

  <script>
    // Ключ для хранения в localStorage
    const STORAGE_KEY = 'socialNetworkPosts';

    // Загрузка постов из localStorage
    function loadPosts() {
      const stored = localStorage.getItem(STORAGE_KEY);
      return stored ? JSON.parse(stored) : [];
    }

    // Сохранение постов в localStorage
    function savePosts(posts) {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(posts));
    }

    // Добавление нового поста
    function addPost() {
      const user = document.getElementById('userName').value.trim() || 'Аноним';
      const content = document.getElementById('message').value.trim();

      if (!content) {
        alert('Напишите сообщение!');
        return;
      }

      const posts = loadPosts();
      posts.push({
        user: user,
        content: content,
        timestamp: Date.now()
      });

      savePosts(posts);
      renderPosts(); // Обновляем ленту
      document.getElementById('message').value = ''; // Очищаем поле
    }

    // Отображение постов
    function renderPosts() {
      const posts = loadPosts().sort((a, b) => b.timestamp - a.timestamp); // Новые сверху
      const container = document.getElementById('postsContainer');
      container.innerHTML = posts.map(post => {
        const date = new Date(post.timestamp).toLocaleString('ru', {
          day: '2-digit',
          month: 'short',
          year: 'numeric',
          hour: '2-digit',
          minute: '2-digit'
        });
        return `
          <div class="post">
            <div class="post-author">${post.user}</div>
            <div class="post-date">${date}</div>
            <div class="post-content">${post.content}</div>
          </div>
        `;
      }).join('');
    }

    // Автоматическое обновление каждые 1 секунду
    setInterval(renderPosts, 1000);

    // Инициализация при загрузке
    window.onload = renderPosts;
  </script>
</body>
</html>
