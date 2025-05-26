Пример:
```html
<!DOCTYPE html>
<html lang="ru">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Панель управления myBot{% endblock %}</title>

    <!-- Стили -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/css/toastr.min.css" rel="stylesheet">
    <link href="{{ url_for('static', filename='css/style.css') }}" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">
</head>

<body>

    <!-- Навигация -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-black border-bottom border-success px-4 fixed-top">
        <a class="navbar-brand text-success fw-bold" href="#"><i class="fas fa-robot"></i> myBot</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"
            aria-controls="navbarNav" aria-expanded="false" aria-label="Переключить навигацию">
            <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse justify-content-end" id="navbarNav">
            <ul class="navbar-nav">
                <li class="nav-item"><a class="nav-link text-success" href="/">Главная</a></li>
                <li class="nav-item"><a class="nav-link text-success" href="/logs">Бот</a></li>
                <li class="nav-item"><a class="nav-link text-success" href="/settings">Сервер</a></li>
                <li class="nav-item"><a class="nav-link text-success" href="/about">GitHub</a></li>
                <li class="nav-item"><a class="nav-link text-success" href="/about">Контакты</a></li>
            </ul>
        </div>
    </nav>

    <!-- Основной контент -->
    <main class="container">
        {% block content %}
        {% endblock %}
    </main>

    <!-- Модальное окно -->
    <div class="modal fade" id="diskModal" tabindex="-1" aria-labelledby="diskModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header bg-dark text-white">
                    <h5 class="modal-title" id="diskModalLabel">Состояние диска</h5>
                    <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal"
                        aria-label="Закрыть"></button>
                </div>
                <div class="modal-body">
                    <table class="table table-striped">
                        <thead>
                            <tr>
                                <th>Размер</th>
                                <th>Директория</th>
                            </tr>
                        </thead>
                        <tbody id="diskUsageTableBody"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- Скрипты -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/js/toastr.min.js"></script>
    <script src="{{ url_for('static', filename='js/scripts.js') }}"></script>
</body>

</html>
```
Далее каждая html-страница:
```html
{% extends 'base.html' %}
{% block title %}Главная - myBot{% endblock %}
{% block content %}

<!--Уникальный контент страницы-->

{% endblock %}
```
