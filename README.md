<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Comentarios</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            padding: 0;
        }
        h1 {
            text-align: center;
        }
        .comment-section {
            max-width: 600px;
            margin: 0 auto;
        }
        .comment-form {
            margin-bottom: 20px;
        }
        .comment-form textarea {
            width: 100%;
            height: 100px;
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            resize: none;
            font-size: 16px;
        }
        .comment-form button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
        }
        .comment-form button:hover {
            background-color: #45a049;
        }
        .comments-list {
            list-style-type: none;
            padding: 0;
        }
        .comments-list li {
            background-color: #f9f9f9;
            border: 1px solid #ddd;
            margin-bottom: 10px;
            padding: 10px;
            border-radius: 5px;
        }
        .comment-date {
            font-size: 12px;
            color: #666;
            margin-top: 5px;
        }
    </style>
</head>
<body>
    <h1>Sistema de Comentarios</h1>
    <div class="comment-section">
        <form class="comment-form" id="commentForm">
            <textarea id="commentInput" placeholder="Escribe tu comentario aquí..." required></textarea>
            <button type="submit">Enviar</button>
        </form>
        <ul class="comments-list" id="commentsList"></ul>
    </div>
    <script>
        // Elementos del DOM
        const commentForm = document.getElementById('commentForm');
        const commentInput = document.getElementById('commentInput');
        const commentsList = document.getElementById('commentsList');

        // Cargar comentarios desde LocalStorage
        function loadComments() {
            const comments = JSON.parse(localStorage.getItem('comments')) || [];
            commentsList.innerHTML = '';
            comments.forEach(comment => {
                const commentItem = document.createElement('li');
                commentItem.innerHTML = `
                    <p>${comment.text}</p>
                    <div class="comment-date">${new Date(comment.date).toLocaleString()}</div>
                `;
                commentsList.appendChild(commentItem);
            });
        }

        // Guardar comentarios en LocalStorage
        function saveComment(comment) {
            const comments = JSON.parse(localStorage.getItem('comments')) || [];
            comments.push(comment);
            localStorage.setItem('comments', JSON.stringify(comments));
        }

        // Manejar el envío del formulario
        commentForm.addEventListener('submit', (e) => {
            e.preventDefault();
            const commentText = commentInput.value.trim();
            if (commentText === '') return;

            const newComment = {
                text: commentText,
                date: new Date().toISOString()
            };

            saveComment(newComment);
            loadComments();
            commentInput.value = '';
        });

        // Cargar comentarios al iniciar
        loadComments();
    </script>
</body>
</html>
