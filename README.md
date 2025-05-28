<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Polityczne Bingo - Druga Tura Wyborów 2025</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1000px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
        }
        
        .header {
            background: linear-gradient(135deg, #c41e3a 0%, #2e8b57 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }
        
        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }
        
        .subtitle {
            font-size: 1.2em;
            opacity: 0.9;
        }
        
        .candidates {
            display: flex;
            justify-content: space-around;
            padding: 20px;
            background: #f8f9fa;
            border-bottom: 3px solid #dee2e6;
        }
        
        .candidate {
            text-align: center;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
            font-size: 1.1em;
        }
        
        .trzaskowski {
            background: linear-gradient(135deg, #ff6b35, #f7931e);
            color: white;
        }
        
        .nawrocki {
            background: linear-gradient(135deg, #4361ee, #3f37c9);
            color: white;
        }
        
        .bingo-container {
            padding: 30px;
        }
        
        .bingo-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            margin: 20px 0;
        }
        
        .bingo-cell {
            aspect-ratio: 1;
            border: 2px solid #dee2e6;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 8px;
            font-size: 0.85em;
            font-weight: 500;
            background: white;
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }
        
        .bingo-cell:hover {
            background: #f8f9fa;
            transform: scale(1.02);
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }
        
        .bingo-cell.marked {
            background: linear-gradient(135deg, #28a745, #20c997);
            color: white;
            border-color: #28a745;
            transform: scale(0.95);
        }
        
        .bingo-cell.marked::after {
            content: '✓';
            position: absolute;
            top: 5px;
            right: 8px;
            font-size: 1.2em;
            font-weight: bold;
        }
        
        .center-cell {
            background: linear-gradient(135deg, #ffd700, #ffed4e) !important;
            color: #333;
            font-weight: bold;
            border-color: #ffd700;
        }
        
        .controls {
            text-align: center;
            margin-top: 20px;
        }
        
        .btn {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 25px;
            font-size: 1em;
            cursor: pointer;
            margin: 0 10px;
            transition: all 0.3s ease;
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }
        
        .instructions {
            background: #e9ecef;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
        }
        
        .winner-message {
            background: linear-gradient(135deg, #28a745, #20c997);
            color: white;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 20px;
            display: none;
        }
        
        @media (max-width: 768px) {
            .bingo-cell {
                font-size: 0.7em;
                padding: 4px;
            }
            
            .header h1 {
                font-size: 2em;
            }
            
            .candidates {
                flex-direction: column;
                gap: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🗳️ Polityczne Bingo</h1>
            <div class="subtitle">Druga Tura Wyborów Prezydenckich 2025</div>
        </div>
        
        <div class="candidates">
            <div class="candidate trzaskowski">
                🟠 Rafał Trzaskowski<br>
                <small>(Koalicja Obywatelska)</small>
            </div>
            <div class="candidate nawrocki">
                🔵 Karol Nawrocki<br>
                <small>(Kandydat obywatelski / PiS)</small>
            </div>
        </div>
        
        <div class="bingo-container">
            <div class="instructions">
                <strong>Instrukcja:</strong> Zaznacz pole, gdy usłyszysz dany argument podczas debaty, wystąpienia lub kampanii. Celem jest uzbieranie 5 pól w rzędzie (poziomo, pionowo lub po przekątnej)!
            </div>
            
            <div class="bingo-grid" id="bingoGrid"></div>
            
            <div class="controls">
                <button class="btn" id="resetBtn">🔄 Resetuj planszę</button>
                <button class="btn" id="shuffleBtn">🎲 Wymieszaj</button>
            </div>
            
            <div class="winner-message" id="winnerMessage">
                🎉 BINGO! Gratulacje! 🎉
            </div>
        </div>
    </div>

    <script>
        const bingoItems = [
            "Obrona demokracji",
            "Wspomnienie o Tusku",
            "13. emerytura",
            "Bruksela nam każe",
            "Wracamy do Europy",
            "Polski Ład to porażka",
            "Ideologia LGBT",
            "Wolne media",
            "Sądy w rękach polityki",
            "500 plus dla dzieci",
            "Inflacja przez PiS",
            "Niemcy nas wyręczają",
            "Zielony Ład niszczy gospodarkę",
            "Waloryzacja emerytur",
            "Reprywatyzacja mieszkań",
            "Smoleńsk to był zamach",
            "Praworządność",
            "Mieszkanie plus",
            "Uchodźcy na granicy",
            "Tusk to agent Niemiec",
            "Kościół w polityce",
            "CPK to fuszerka",
            "Euro zamiast złotego",
            "Czyszczenie sądów",
            "Finansowanie z zagranicy",
            "Soros finansuje kampanię",
            "Nielegalne finansowanie",
            "Prostytutki w hotelu",
            "Kampania kłamstw",
            "Dezinformacja i fake news",
            "Ordo Iuris atakuje",
            "Prostytucja polityczna",
            "Obce pieniądze",
            "Stekiem kłamstw",
            "Pozew sądowy",
            "Prokuratura bada",
            "DARMOWE POLE"
        ];

        let markedCells = new Set();

        function createBingoGrid() {
            const grid = document.getElementById('bingoGrid');
            grid.innerHTML = '';
            
            for (let i = 0; i < 25; i++) {
                const cell = document.createElement('div');
                cell.className = 'bingo-cell';
                cell.dataset.index = i;
                
                if (i === 18) {
                    cell.textContent = bingoItems[36];
                    cell.classList.add('center-cell', 'marked');
                    markedCells.add(i);
                } else {
                    const itemIndex = i < 18 ? i : (i > 18 ? i - 1 : i);
                    cell.textContent = bingoItems[itemIndex];
                }
                
                cell.addEventListener('click', function() {
                    toggleCell(i);
                });
                
                grid.appendChild(cell);
            }
        }

        function toggleCell(index) {
            if (index === 18) return;
            
            const cell = document.querySelector('[data-index="' + index + '"]');
            
            if (markedCells.has(index)) {
                markedCells.delete(index);
                cell.classList.remove('marked');
            } else {
                markedCells.add(index);
                cell.classList.add('marked');
            }
            
            checkBingo();
        }

        function checkBingo() {
            const winConditions = [
                [0, 1, 2, 3, 4],
                [5, 6, 7, 8, 9],
                [10, 11, 12, 13, 14],
                [15, 16, 17, 18, 19],
                [20, 21, 22, 23, 24],
                [0, 5, 10, 15, 20],
                [1, 6, 11, 16, 21],
                [2, 7, 12, 17, 22],
                [3, 8, 13, 18, 23],
                [4, 9, 14, 19, 24],
                [0, 6, 12, 18, 24],
                [4, 8, 12, 16, 20]
            ];

            for (let condition of winConditions) {
                if (condition.every(index => markedCells.has(index))) {
                    showWinner();
                    return;
                }
            }
        }

        function showWinner() {
            document.getElementById('winnerMessage').style.display = 'block';
            setTimeout(function() {
                document.getElementById('winnerMessage').style.display = 'none';
            }, 5000);
        }

        function resetBingo() {
            markedCells.clear();
            markedCells.add(18);
            
            document.querySelectorAll('.bingo-cell').forEach(function(cell) {
                cell.classList.remove('marked');
            });
            
            document.querySelector('[data-index="18"]').classList.add('marked');
            document.getElementById('winnerMessage').style.display = 'none';
        }

        function shuffleBingo() {
            const itemsToShuffle = bingoItems.slice(0, 36);
            
            for (let i = itemsToShuffle.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [itemsToShuffle[i], itemsToShuffle[j]] = [itemsToShuffle[j], itemsToShuffle[i]];
            }
            
            document.querySelectorAll('.bingo-cell').forEach(function(cell, index) {
                if (index !== 18) {
                    const itemIndex = index < 18 ? index : (index > 18 ? index - 1 : index);
                    cell.textContent = itemsToShuffle[itemIndex];
                }
            });
            
            resetBingo();
        }

        // Inicjalizacja po załadowaniu strony
        document.addEventListener('DOMContentLoaded', function() {
            createBingoGrid();
            
            document.getElementById('resetBtn').addEventListener('click', resetBingo);
            document.getElementById('shuffleBtn').addEventListener('click', shuffleBingo);
        });
    </script>
</body>
</html>
