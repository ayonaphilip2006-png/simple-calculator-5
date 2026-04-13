<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScript Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        .calculator {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            width: 320px;
            padding: 20px;
            overflow: hidden;
        }

        .display {
            background: #2d3436;
            color: #00ff88;
            font-size: 2.5em;
            padding: 20px;
            border-radius: 10px;
            text-align: right;
            margin-bottom: 20px;
            min-height: 80px;
            word-wrap: break-word;
            word-break: break-all;
            font-weight: 500;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        button {
            padding: 20px;
            font-size: 1.5em;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
            background: #f0f0f0;
            color: #333;
        }

        button:hover {
            background: #e0e0e0;
            transform: translateY(-2px);
        }

        button:active {
            transform: translateY(0);
        }

        .operator {
            background: #667eea;
            color: white;
        }

        .operator:hover {
            background: #5568d3;
        }

        .equals {
            background: #00d084;
            color: white;
            grid-column: span 2;
        }

        .equals:hover {
            background: #00b871;
        }

        .clear {
            background: #ff7675;
            color: white;
            grid-column: span 2;
        }

        .clear:hover {
            background: #d63031;
        }

        .delete {
            background: #fdcb6e;
            color: #333;
        }

        .delete:hover {
            background: #e8b923;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display" id="display">0</div>
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">AC</button>
            <button class="delete" onclick="deleteLast()">DEL</button>
            <button class="operator" onclick="appendOperator('/')">÷</button>
            <button class="operator" onclick="appendOperator('*')">×</button>
            
            <button onclick="appendNumber('7')">7</button>
            <button onclick="appendNumber('8')">8</button>
            <button onclick="appendNumber('9')">9</button>
            <button class="operator" onclick="appendOperator('-')">−</button>
            
            <button onclick="appendNumber('4')">4</button>
            <button onclick="appendNumber('5')">5</button>
            <button onclick="appendNumber('6')">6</button>
            <button class="operator" onclick="appendOperator('+')">+</button>
            
            <button onclick="appendNumber('1')">1</button>
            <button onclick="appendNumber('2')">2</button>
            <button onclick="appendNumber('3')">3</button>
            <button class="operator" onclick="appendDecimal()">.</button>
            
            <button onclick="appendNumber('0')" style="grid-column: span 2;">0</button>
            <button class="equals" onclick="calculate()">=</button>
        </div>
    </div>

    <script>
        let display = document.getElementById('display');
        let currentInput = '0';
        let previousInput = '';
        let operation = null;
        let shouldResetDisplay = false;

        function updateDisplay() {
            display.textContent = currentInput;
        }

        function appendNumber(num) {
            if (shouldResetDisplay) {
                currentInput = num;
                shouldResetDisplay = false;
            } else {
                if (currentInput === '0' && num !== '.') {
                    currentInput = num;
                } else {
                    currentInput += num;
                }
            }
            updateDisplay();
        }

        function appendOperator(op) {
            if (operation !== null && !shouldResetDisplay) {
                calculate();
            }
            previousInput = currentInput;
            operation = op;
            shouldResetDisplay = true;
        }

        function appendDecimal() {
            if (shouldResetDisplay) {
                currentInput = '0.';
                shouldResetDisplay = false;
            } else if (!currentInput.includes('.')) {
                currentInput += '.';
            }
            updateDisplay();
        }

        function calculate() {
            if (operation === null || shouldResetDisplay) {
                return;
            }

            let result;
            const prev = parseFloat(previousInput);
            const current = parseFloat(currentInput);

            switch (operation) {
                case '+':
                    result = prev + current;
                    break;
                case '-':
                    result = prev - current;
                    break;
                case '*':
                    result = prev * current;
                    break;
                case '/':
                    result = prev / current;
                    break;
                default:
                    return;
            }

            currentInput = result.toString();
            operation = null;
            shouldResetDisplay = true;
            updateDisplay();
        }

        function clearDisplay() {
            currentInput = '0';
            previousInput = '';
            operation = null;
            shouldResetDisplay = false;
            updateDisplay();
        }

        function deleteLast() {
            if (currentInput.length > 1) {
                currentInput = currentInput.slice(0, -1);
            } else {
                currentInput = '0';
            }
            updateDisplay();
        }
    </script>
</body>
</html>
