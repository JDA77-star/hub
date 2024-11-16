# hub
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Juego de Tres en Raya</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background url('k.jpg')no-repeat center center fixed;
            background-size: cover;
        }
        .game-container {
            background-color: rgba(0, 0, 0, 0.8); /* Fondo oscuro semitransparente */
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.5);
            text-align: center;
        }
        .game-board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
            margin: 20px auto;
        }
        .cell {
            width: 100px;
            height: 100px;
            background-color: #fff;
            border: 2px solid #000;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2rem;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s, background-color 0.2s;
        }
        .cell.taken {
            cursor: not-allowed;
            background-color: #ccc;
        }
        .cell:hover:not(.taken) {
            background-color: #f0f0f0;
            transform: scale(1.1);
        }
        .message {
            font-size: 1.5rem;
            color: white;
            margin: 20px 0;
        }
        .reset-button {
            padding: 10px 20px;
            font-size: 1rem;
            background-color: #007BFF;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .reset-button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <div class="game-board" id="board"></div>
        <div class="message" id="message">Turno del jugador X</div>
        <button class="reset-button" id="resetButton">Reiniciar Juego</button>
    </div>

    <script>
        let currentPlayer = 'X';
        let gameBoard = ['', '', '', '', '', '', '', '', ''];
        const boardElement = document.getElementById('board');
        const messageElement = document.getElementById('message');
        const resetButton = document.getElementById('resetButton');

        function renderBoard() {
            boardElement.innerHTML = '';
            gameBoard.forEach((cell, index) => {
                const cellElement = document.createElement('div');
                cellElement.classList.add('cell');
                if (cell) {
                    cellElement.textContent = cell;
                    cellElement.classList.add('taken');
                }
                cellElement.addEventListener('click', () => handlePlayerMove(index));
                boardElement.appendChild(cellElement);
            });
        }

        function handlePlayerMove(index) {
            if (gameBoard[index] || checkWinner(gameBoard)) return;

            gameBoard[index] = currentPlayer;
            renderBoard();

            if (checkWinner(gameBoard)) {
                messageElement.textContent = `¡Jugador ${currentPlayer} gana!`;
            } else if (gameBoard.every(cell => cell)) {
                messageElement.textContent = '¡Empate!';
            } else {
                currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
                messageElement.textContent = `Turno del jugador ${currentPlayer}`;
                if (currentPlayer === 'O') {
                    setTimeout(() => computerMove(), 500);
                }
            }
        }

        function computerMove() {
            if (checkWinner(gameBoard)) return;

            const bestMove = minimax(gameBoard, 'O');
            gameBoard[bestMove.index] = 'O';
            renderBoard();

            if (checkWinner(gameBoard)) {
                messageElement.textContent = '¡La computadora gana!';
            } else if (gameBoard.every(cell => cell)) {
                messageElement.textContent = '¡Empate!';
            } else {
                currentPlayer = 'X';
                messageElement.textContent = `Turno del jugador ${currentPlayer}`;
            }
        }

        function minimax(board, player) {
            const availableMoves = getAvailableMoves(board);

            if (checkWinner(board, 'X')) return { score: -10 };
            if (checkWinner(board, 'O')) return { score: 10 };
            if (availableMoves.length === 0) return { score: 0 };

            const moves = [];
            for (let i = 0; i < availableMoves.length; i++) {
                const move = availableMoves[i];
                const newBoard = [...board];
                newBoard[move] = player;

                const result = minimax(newBoard, player === 'O' ? 'X' : 'O');
                moves.push({ index: move, score: result.score });
            }

            let bestMove;
            if (player === 'O') {
                let bestScore = -Infinity;
                for (let i = 0; i < moves.length; i++) {
                    if (moves[i].score > bestScore) {
                        bestScore = moves[i].score;
                        bestMove = moves[i];
                    }
                }
            } else {
                let bestScore = Infinity;
                for (let i = 0; i < moves.length; i++) {
                    if (moves[i].score < bestScore) {
                        bestScore = moves[i].score;
                        bestMove = moves[i];
                    }
                }
            }
            return bestMove;
        }

        function getAvailableMoves(board) {
            return board.map((cell, index) => cell === '' ? index : null).filter(index => index !== null);
        }

        function checkWinner(board, player = null) {
            const winPatterns = [
                [0, 1, 2],
                [3, 4, 5],
                [6, 7, 8],
                [0, 3, 6],
                [1, 4, 7],
                [2, 5, 8],
                [0, 4, 8],
                [2, 4, 6]
            ];

            if (player) {
                return winPatterns.some(pattern =>
                    pattern.every(index => board[index] === player)
                );
            }

            return winPatterns.some(pattern => {
                const [a, b, c] = pattern;
                return board[a] && board[a] === board[b] && board[a] === board[c];
            });
        }

        function resetGame() {
            gameBoard = ['', '', '', '', '', '', '', '', ''];
            currentPlayer = 'X';
            messageElement.textContent = `Turno del jugador ${currentPlayer}`;
            renderBoard();
        }

        resetButton.addEventListener('click', resetGame);

        resetGame();
    </script>
<br>
<br>
<Center>

<FONT FACE="ARIAL" COLOR="BLACK" SIZE=5> <H1>  Esta pagina con este minijuego fue creado con I.A </H1> </FONT>
<A HREF=""> <FONT FACE="ARIAL" COLOR="BLACK" SIZE=5> <H1>  Verdad que tiene potencial? </H1> </FONT> </A>
</center>
</body>
</html>

