# Calculator
A calculator anyone can use for free without viewing ads


https://freecalculatoropensource.tiiny.site/


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NEXUS • CALC</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Space+Grotesk:wght@500;600&display=swap');

        :root {
            --primary: #00f5ff;
            --accent: #ff00aa;
            --bg: #0a0a0f;
            --card: rgba(20, 20, 35, 0.95);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #0a0a0f 0%, #1a0033 100%);
            color: white;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
            position: relative;
        }

        body::before {
            content: '';
            position: absolute;
            inset: 0;
            background: radial-gradient(circle at 50% 20%, rgba(0, 245, 255, 0.15), transparent 50%);
            pointer-events: none;
        }

        .calculator {
            width: 380px;
            background: var(--card);
            border-radius: 32px;
            overflow: hidden;
            box-shadow: 
                0 0 80px rgba(0, 245, 255, 0.4),
                0 25px 50px -12px rgb(0 0 0 / 0.5);
            border: 1px solid rgba(0, 245, 255, 0.3);
            backdrop-filter: blur(20px);
            position: relative;
        }

        .header {
            padding: 20px 24px 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(10, 10, 15, 0.8);
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }

        .title {
            font-family: 'Space Grotesk', sans-serif;
            font-size: 22px;
            font-weight: 600;
            background: linear-gradient(90deg, #00f5ff, #ff00aa);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            letter-spacing: -1px;
        }

        .mode-toggle {
            padding: 6px 14px;
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(0,245,255,0.4);
            border-radius: 9999px;
            font-size: 12px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .mode-toggle.active {
            background: var(--primary);
            color: black;
            box-shadow: 0 0 15px rgba(0, 245, 255, 0.6);
        }

        .display {
            padding: 30px 24px 20px;
            background: rgba(10, 10, 20, 0.9);
            position: relative;
        }

        .history {
            font-size: 14px;
            color: #888;
            text-align: right;
            height: 22px;
            overflow: hidden;
        }

        .main-display {
            font-size: 52px;
            font-weight: 600;
            text-align: right;
            min-height: 72px;
            word-break: break-all;
            padding: 8px 0;
            font-family: 'Space Grotesk', sans-serif;
            background: linear-gradient(90deg, #fff, #ddd);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .buttons {
            padding: 20px;
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
        }

        button {
            height: 68px;
            border-radius: 20px;
            border: none;
            font-size: 22px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
        }

        button::after {
            content: '';
            position: absolute;
            inset: 0;
            background: linear-gradient(white, white);
            opacity: 0;
            transition: opacity 0.2s;
        }

        button:active::after {
            opacity: 0.2;
        }

        .btn-number {
            background: #1f1f2e;
            color: white;
        }

        .btn-operator {
            background: #2a2a3f;
            color: var(--primary);
            font-size: 24px;
        }

        .btn-function {
            background: #1a1a2a;
            color: #ff00aa;
            font-size: 18px;
        }

        .btn-equal {
            background: linear-gradient(135deg, var(--primary), #00aaff);
            color: black;
            font-size: 28px;
            font-weight: 700;
        }

        .btn-clear {
            background: #3a1a1a;
            color: #ff5555;
        }

        .advanced-panel {
            display: none;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
            padding: 0 20px 20px;
            border-top: 1px solid rgba(255,255,255,0.1);
        }

        .footer {
            padding: 12px 24px;
            display: flex;
            justify-content: space-between;
            font-size: 11px;
            color: #666;
            background: rgba(10,10,15,0.6);
        }

        .glow {
            box-shadow: 0 0 25px currentColor;
        }

        .result {
            font-size: 18px;
            color: #00ffaa;
            text-align: right;
            margin-top: 8px;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <!-- Header -->
        <div class="header">
            <div class="title">NEXUS</div>
            <div onclick="toggleAdvanced()" id="adv-btn" class="mode-toggle">ADVANCED</div>
        </div>

        <!-- Display -->
        <div class="display">
            <div class="history" id="history"></div>
            <div class="main-display" id="display">0</div>
            <div class="result" id="result"></div>
        </div>

        <!-- Basic Buttons -->
        <div class="buttons" id="basic-buttons">
            <button class="btn-clear" onclick="clearAll()">AC</button>
            <button class="btn-function" onclick="toggleSign()">±</button>
            <button class="btn-function" onclick="percent()">%</button>
            <button class="btn-operator" onclick="appendOperator('/')">÷</button>

            <button class="btn-number" onclick="appendNumber('7')">7</button>
            <button class="btn-number" onclick="appendNumber('8')">8</button>
            <button class="btn-number" onclick="appendNumber('9')">9</button>
            <button class="btn-operator" onclick="appendOperator('*')">×</button>

            <button class="btn-number" onclick="appendNumber('4')">4</button>
            <button class="btn-number" onclick="appendNumber('5')">5</button>
            <button class="btn-number" onclick="appendNumber('6')">6</button>
            <button class="btn-operator" onclick="appendOperator('-')">−</button>

            <button class="btn-number" onclick="appendNumber('1')">1</button>
            <button class="btn-number" onclick="appendNumber('2')">2</button>
            <button class="btn-number" onclick="appendNumber('3')">3</button>
            <button class="btn-operator" onclick="appendOperator('+')">+</button>

            <button class="btn-number" onclick="appendNumber('0')" style="grid-column: span 2;">0</button>
            <button class="btn-number" onclick="appendNumber('.')">.</button>
            <button class="btn-equal" onclick="calculate()">=</button>
        </div>

        <!-- Advanced Panel -->
        <div class="advanced-panel" id="advanced-panel">
            <button class="btn-function" onclick="appendFunction('sqrt')">√</button>
            <button class="btn-function" onclick="appendFunction('cbrt')">∛</button>
            <button class="btn-function" onclick="appendFunction('log')">log</button>
            <button class="btn-function" onclick="appendFunction('ln')">ln</button>

            <button class="btn-function" onclick="appendFunction('pow')">xʸ</button>
            <button class="btn-function" onclick="modulo()">MOD</button>
            <button class="btn-function" onclick="appendFunction('sin')">sin</button>
            <button class="btn-function" onclick="appendFunction('cos')">cos</button>

            <button class="btn-function" onclick="appendFunction('tan')">tan</button>
            <button class="btn-function" onclick="pi()">π</button>
            <button class="btn-function" onclick="eConst()">e</button>
            <button class="btn-function" onclick="factorial()">n!</button>
        </div>

        <div class="footer">
            <div>⌨️ Keyboard supported</div>
            <div onclick="showHistory()" style="cursor:pointer;">HISTORY</div>
        </div>
    </div>

    <!-- History Modal -->
    <div id="history-modal" style="display:none; position:fixed; inset:0; background:rgba(0,0,0,0.9); align-items:center; justify-content:center; z-index:100;">
        <div style="background:#1a1a2a; width:90%; max-width:400px; border-radius:24px; padding:24px; max-height:80vh; overflow:auto;">
            <h3 style="margin-bottom:16px; color:var(--primary);">Calculation History</h3>
            <div id="history-list" style="font-family:monospace; line-height:1.6;"></div>
            <button onclick="hideHistory()" style="margin-top:20px; width:100%; padding:12px; background:#333; border:none; border-radius:12px; color:white;">Close</button>
        </div>
    </div>

    <script>
        let display = document.getElementById('display');
        let historyEl = document.getElementById('history');
        let resultEl = document.getElementById('result');
        let currentInput = '0';
        let previousInput = '';
        let operator = '';
        let shouldResetDisplay = false;
        let history = [];

        function updateDisplay() {
            display.textContent = currentInput;
        }

        function appendNumber(number) {
            if (shouldResetDisplay) {
                currentInput = '';
                shouldResetDisplay = false;
            }
            if (currentInput === '0' && number !== '.') {
                currentInput = number;
            } else {
                currentInput += number;
            }
            updateDisplay();
        }

        function appendOperator(op) {
            if (operator !== '' && !shouldResetDisplay) {
                calculate();
            }
            previousInput = currentInput;
            operator = op;
            shouldResetDisplay = true;
            historyEl.textContent = `${previousInput} ${op}`;
        }

        function calculate() {
            if (operator === '' || shouldResetDisplay) return;
            
            let result;
            const prev = parseFloat(previousInput);
            const current = parseFloat(currentInput);

            switch(operator) {
                case '+': result = prev + current; break;
                case '-': result = prev - current; break;
                case '*': result = prev * current; break;
                case '/': result = prev / current; break;
            }

            const expression = `${previousInput} ${operator} ${currentInput} =`;
            addToHistory(expression, result);

            currentInput = parseFloat(result.toFixed(8)).toString();
            operator = '';
            shouldResetDisplay = true;
            historyEl.textContent = '';
            resultEl.textContent = '';
            updateDisplay();
        }

        function clearAll() {
            currentInput = '0';
            previousInput = '';
            operator = '';
            shouldResetDisplay = false;
            historyEl.textContent = '';
            resultEl.textContent = '';
            updateDisplay();
        }

        function toggleSign() {
            if (currentInput === '0') return;
            if (currentInput.startsWith('-')) {
                currentInput = currentInput.slice(1);
            } else {
                currentInput = '-' + currentInput;
            }
            updateDisplay();
        }

        function percent() {
            currentInput = (parseFloat(currentInput) / 100).toString();
            updateDisplay();
        }

        // Advanced Functions
        function appendFunction(func) {
            let value = parseFloat(currentInput);
            let result;

            switch(func) {
                case 'sqrt':
                    result = Math.sqrt(value);
                    break;
                case 'cbrt':
                    result = Math.cbrt(value);
                    break;
                case 'log':
                    result = Math.log10(value);
                    break;
                case 'ln':
                    result = Math.log(value);
                    break;
                case 'sin':
                    result = Math.sin(value * Math.PI / 180);
                    break;
                case 'cos':
                    result = Math.cos(value * Math.PI / 180);
                    break;
                case 'tan':
                    result = Math.tan(value * Math.PI / 180);
                    break;
            }

            addToHistory(`${func}(${currentInput})`, result);
            currentInput = parseFloat(result.toFixed(8)).toString();
            updateDisplay();
        }

        function appendFunctionPow() {
            previousInput = currentInput;
            operator = '^';
            shouldResetDisplay = true;
            historyEl.textContent = `${previousInput} ^`;
        }

        function modulo() {
            previousInput = currentInput;
            operator = '%';
            shouldResetDisplay = true;
            historyEl.textContent = `${previousInput} mod`;
        }

        function pi() {
            currentInput = Math.PI.toFixed(8);
            updateDisplay();
        }

        function eConst() {
            currentInput = Math.E.toFixed(8);
            updateDisplay();
        }

        function factorial() {
            let n = parseInt(currentInput);
            if (n < 0 || isNaN(n)) return;
            let res = 1;
            for(let i = 2; i <= n; i++) res *= i;
            addToHistory(`${n}!`, res);
            currentInput = res.toString();
            updateDisplay();
        }

        function addToHistory(expr, res) {
            history.unshift({expr: expr, result: res});
            if (history.length > 10) history.pop();
        }

        function showHistory() {
            const modal = document.getElementById('history-modal');
            const list = document.getElementById('history-list');
            list.innerHTML = '';
            
            history.forEach(item => {
                const div = document.createElement('div');
                div.style.marginBottom = '12px';
                div.style.padding = '10px';
                div.style.background = 'rgba(255,255,255,0.05)';
                div.style.borderRadius = '12px';
                div.innerHTML = `
                    <div style="color:#888; font-size:13px;">${item.expr}</div>
                    <div style="color:#00ffaa; font-size:18px; font-weight:600;">${item.result}</div>
                `;
                list.appendChild(div);
            });
            
            modal.style.display = 'flex';
        }

        function hideHistory() {
            document.getElementById('history-modal').style.display = 'none';
        }

        function toggleAdvanced() {
            const panel = document.getElementById('advanced-panel');
            const btn = document.getElementById('adv-btn');
            
            if (panel.style.display === 'grid') {
                panel.style.display = 'none';
                btn.classList.remove('active');
            } else {
                panel.style.display = 'grid';
                btn.classList.add('active');
            }
        }

        // Keyboard Support
        document.addEventListener('keydown', (e) => {
            if (e.key >= '0' && e.key <= '9') appendNumber(e.key);
            if (e.key === '.') appendNumber('.');
            if (e.key === '+') appendOperator('+');
            if (e.key === '-') appendOperator('-');
            if (e.key === '*') appendOperator('*');
            if (e.key === '/') appendOperator('/');
            if (e.key === 'Enter' || e.key === '=') calculate();
            if (e.key === 'Backspace') {
                if (currentInput.length > 1) {
                    currentInput = currentInput.slice(0, -1);
                } else {
                    currentInput = '0';
                }
                updateDisplay();
            }
            if (e.key === 'Escape') clearAll();
        });

        // Initialize
        updateDisplay();

        // Bonus: Add pow support to operator
        // Override one button or add listener
        console.log('%cNEXUS CALC loaded 🔥', 'color:#00f5ff; font-size:14px; font-weight:bold;');
    </script>
</body>
</html>
