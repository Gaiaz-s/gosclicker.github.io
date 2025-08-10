# gosclicker.github.io

<!DOCTYPE html>
<html lang="ru">
<head>
  <h2> ГОС-Клик <h2>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Гос-Клик</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: white;
            color: black;
            font-family: 'Arial', sans-serif;
            overflow: hidden;
        }
        #clicker {
            width: 150px;
            height: 150px;
            border-radius: 50%;
            background-color: #1E90FF;
            color: white;
            font-size: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border: 5px solid #8A2BE2;
            box-shadow: 0 4px 20px rgba(255, 105, 180, 0.5);
            cursor: pointer;
            transition: transform 0.1s, box-shadow 0.2s;
        }
        #clicker:hover {
            transform: scale(1.05);
            box-shadow: 0 8px 30px rgba(255, 105, 180, 0.7);
        }
        #score {
            margin-top: 20px;
            font-size: 32px;
        }
        .card-container {
            display: flex;
            margin-top: 20px;
        }
        .card {
            width: 120px;
            height: 180px;
            border-radius: 10px;
            background-color: white;
            border: 2px solid #ccc;
            margin: 0 10px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.5);
            cursor: pointer;
            transition: transform 0.1s, box-shadow 0.2s;
        }
        .card:hover {
            transform: scale(1.05);
            box-shadow: 0 4px 20px rgba(255, 105, 180, 0.5);
        }
        .card-title {
            font-size: 18px;
            margin-bottom: 10px;
        }
        .card-cost {
            font-size: 16px;
        }
        #footer {
            margin-top: 20px;
            font-size: 16px;
            color: #aaa;
        }
        #controls {
            margin-top: 20px;
        }
        button {
            padding: 10px 15px;
            margin: 5px;
            border-radius: 5px;
            border: none;
            background-color: #ff4081;
            color: black;
            cursor: pointer;
        }
        button:hover {
            background-color: #1E90FF;
        }
        #profit {
            margin-top: 20px;
            font-size: 20px;
        }
        #menu {
            display: flex;
            justify-content: space-around;
            width: 100%;
            margin-top: 20px;
        }
    </style>
</head>
<body>

<div id="clicker"></div>
<div id="score">DQ: 100</div>
<div id="profit">Прибыль за час: 0</div>

<div class="card-container">
    <div class="card" data-cost="100" data-increment="1">
        <div class="card-title">STANDART</div>
        <div class="card-cost">100 DQ</div>
    </div>
    <div class="card" data-cost="500" data-increment="5">
        <div class="card-title">MEDIUM</div>
        <div class="card-cost">500 DQ</div>
    </div>
    <div class="card" data-cost="900" data-increment="10">
        <div class="card-title">DYNAMIC</div>
        <div class="card-cost">900 DQ</div>
    </div>

  <div class="card" data-cost="2000" data-increment="10">
        <div class="card-title">BUSINESS</div>
        <div class="card-cost">2000 DQ</div>
    </div>

    <div class="card" data-cost="2100" data-increment="10">
        <div class="card-title">MEGA</div>
        <div class="card-cost">2100 DQ</div>
    </div>
</div>

<div id="controls">
    <button id="saveBtn">Сохранить прогресс</button>
    <button id="resetBtn">Удалить аккаунт</button>
</div>


<script>
    let score = localStorage.getItem('score') ? parseInt(localStorage.getItem('score')) : 0; // Загружаем сохраненный счет
    let clickValue = 1;

    document.getElementById('score').innerText = 'DQ: ' + score;

    document.getElementById('clicker').addEventListener('click', function() {
        score += clickValue;
        document.getElementById('score').innerText = 'DQ: ' + score;
    });

    const cards = document.querySelectorAll('.card');
    
    cards.forEach(card => {
        card.addEventListener('click', function() {
            const cost = parseInt(this.getAttribute('data-cost'));
            const increment = parseInt(this.getAttribute('data-increment'));
            
            if (score >= cost) {
                score -= cost; // Уменьшаем количество монет
                clickValue += increment; // Увеличиваем значение клика
                document.getElementById('score').innerText = 'DQ: ' + score;

                alert(`Вы купили ${this.querySelector('.card-title').innerText}! Теперь ваш клик стоит ${clickValue} монет.`);
                this.style.pointerEvents = 'none'; // Деактивируем карточку после покупки
                this.style.opacity = '0.5'; // Уменьшаем видимость карточки
            } else {
                alert('Недостаточно DQ для покупки!');
            }
        });
    });

    document.getElementById('saveBtn').addEventListener('click', function() {
        localStorage.setItem('score', score);
        alert('Прогресс сохранен!');
    });

    document.getElementById('resetBtn').addEventListener('click', function() {
        if (confirm('Вы уверены, что хотите удалить аккаунт? Это действие необратимо!')) {
            localStorage.removeItem('score');
            score = 0; // Сбрасываем счет
            clickValue = 1; // Сбрасываем значение клика
            document.getElementById('score').innerText = 'DQ: ' + score;

            // Восстанавливаем карточки
            cards.forEach(card => {
                card.style.pointerEvents = 'auto';
                card.style.opacity = '1';
            });
            
            alert('Аккаунт удален. Начните заново!');
        }
    });

    document.getElementById('miniGameBtn').addEventListener('click', function() {
        alert('Здесь будет мини-игра!'); // Здесь можно добавить логику мини-игры
    });

    document.getElementById('shopBtn').addEventListener('click', function() {
        alert('Здесь будет магазин!'); // Здесь можно добавить логику магазина
    });

    // Автоматическая прибыль за час
    setInterval(() => {
        const hourlyProfit = Math.floor(clickValue * (3600 / 1000)); // Примерная формула прибыли за час
        score += hourlyProfit; 
        document.getElementById('score').innerText = 'Монеты: ' + score;
        alert(`Вы заработали ${hourlyProfit} монет за час!`);
    
