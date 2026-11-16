<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Крестики-нолики</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #282c34;
            color: white;
        }
        h1 {
            margin-bottom: 20px;
        }
        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
            background-color: #61dafb;
            padding: 5px;
            border-radius: 10px;
        }
        .cell {
            background-color: #282c34;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            font-weight: bold;
            color: #61dafb;
            cursor: pointer;
            user-select: none;
            border-radius: 5px;
        }
        .cell:hover {
            background-color: #3a3f4b;
        }
        .status {
            margin-top: 20px;
            font-size: 1.2rem;
            height: 30px;
        }
        button {
            margin-top: 15px;
            padding: 10px 20px;
            font-size: 1rem;
            background-color: #61dafb;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            color: #282c34;
        }
        button:hover {
            background-color: #4fa8c7;
        }
    </style>
</head>
<body>

    <h1>Крестики-нолики</h1>
    <div class="board" id="board">
        <div class="cell" data-index="0"></div>
        <div class="cell" data-index="1"></div>
        <div class="cell" data-index="2"></div>
        <div class="cell" data-index="3"></div>
        <div class="cell" data-index="4"></div>
        <div class="cell" data-index="5"></div>
        <div class="cell" data-index="6"></div>
        <div class="cell" data-index="7"></div>
        <div class="cell" data-index="8"></div>
    </div>
    <div class="status" id="status">Ход игрока: X</div>
    <button id="restartBtn">Начать заново</button>

    <script>
        const board = document.getElementById('board');
        const statusText = document.getElementById('status');
        const restartBtn = document.getElementById('restartBtn');
        const cells = document.querySelectorAll('.cell');

        let options = ['', '', '', '', '', '', '', '', ''];
        let currentPlayer = 'X';
        let running = true;

        const winConditions = [
            [0, 1, 2],
            [3, 4, 5],
            [6, 7, 8],
            [0, 3, 6],
            [1, 4, 7],
            [2, 5, 8],
            [0, 4, 8],
            [2, 4, 6]
        ];

        initializeGame();

        function initializeGame() {
            cells.forEach(cell => cell.addEventListener('click', cellClicked));
            restartBtn.addEventListener('click', restartGame);
            statusText.textContent = `Ход игрока: ${currentPlayer}`;
        }

        function cellClicked() {
            const cellIndex = this.getAttribute('data-index');

            if (options[cellIndex] !== '' || !running) {
                return;
            }

            updateCell(this, cellIndex);
            checkWinner();
        }

        function updateCell(cell, index) {
            options[index] = currentPlayer;
            cell.textContent = currentPlayer;
        }

        function changePlayer() {
            currentPlayer = (currentPlayer === 'X') ? 'O' : 'X';
            statusText.textContent = `Ход игрока: ${currentPlayer}`;
        }

        function checkWinner() {
            let roundWon = false;

            for (let i = 0; i < winConditions.length; i++) {
                const condition = winConditions[i];
                const cellA = options[condition[0]];
                const cellB = options[condition[1]];
                const cellC = options[condition[2]];

                if (cellA === '' || cellB === '' || cellC === '') {
                    continue;
                }
                if (cellA === cellB && cellB === cellC) {
                    roundWon = true;
                    break;
                }
            }

            if (roundWon) {
                statusText.textContent = `Игрок ${currentPlayer} победил! 🎉`;
                running = false;
            } else if (!options.includes('')) {
                statusText.textContent = 'Ничья! 🤝';
                running = false;
            } else {
                changePlayer();
            }
        }

        function restartGame() {
            currentPlayer = 'X';
            options = ['', '', '', '', '', '', '', '', ''];
            statusText.textContent = `Ход игрока: ${currentPlayer}`;
            cells.forEach(cell => cell.textContent = '');
            running = true;
        }
    </script>
</body>
</html>
