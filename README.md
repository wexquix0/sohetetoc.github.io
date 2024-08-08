<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>sohbet</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: url('https://r.resimlink.com/KvMW9waY.jpg') no-repeat center center fixed;
            background-size: cover;
            margin: 0;
            padding: 0;
            color: #fff;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .chat-container {
            width: 100%;
            max-width: 600px;
            height: 700px;
            display: flex;
            flex-direction: column;
            border-radius: 15px;
            overflow: hidden;
        }
        .chat-box {
            flex: 1;
            background-color: rgba(0, 0, 0, 0.7);
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
        }
        .message {
            margin: 10px 0;
            padding: 15px;
            border-radius: 15px;
            max-width: 80%;
            word-wrap: break-word;
        }
        .user-message {
            background-color: #007bff;
            color: #fff;
            align-self: flex-end;
        }
        .bot-message {
            background-color: #28a745;
            color: #fff;
            align-self: flex-start;
        }
        .input-container {
            display: flex;
            padding: 10px;
            background: rgba(0, 0, 0, 0.8);
        }
        input[type="text"] {
            flex: 1;
            padding: 15px;
            border: none;
            border-radius: 15px;
            margin-right: 10px;
            box-sizing: border-box;
            background-color: #fff;
            color: #333;
        }
        button {
            padding: 15px 25px;
            border: none;
            border-radius: 15px;
            background-color: #007bff;
            color: #fff;
            cursor: pointer;
            font-size: 16px;
        }
        button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <div class="chat-box" id="chat">
            <!-- Mesajlar buraya eklenecek -->
        </div>
        <div class="input-container">
            <input type="text" id="userInput" placeholder="Mesajınızı yazın...">
            <button onclick="sendMessage()">Gönder</button>
        </div>
    </div>
    <script>
        async function sendMessage() {
            const userInput = document.getElementById('userInput').value;
            if (!userInput.trim()) return;
            document.getElementById('chat').innerHTML += `
                <div class="message user-message">${userInput}</div>
            `;            
            try {
                const response = await fetch(`https://tilki.dev/api/hercai?soru=${encodeURIComponent(userInput)}`);
                const data = await response.json();               
                console.log('API Yanıtı:', data);              
                const botResponse = data.cevap || "Üzgünüm, yanıt bulamadım.")    
                document.getElementById('chat').innerHTML += `
                    <div class="message bot-message">${botResponse}</div>
                `;
            } catch (error) {
                console.error('API isteğinde bir hata oluştu:', error);
                document.getElementById('chat').innerHTML += `
                    <div class="message bot-message">Bir hata oluştu. Lütfen tekrar deneyin.</div>
                `;
            }  
            document.getElementById('userInput').value = '';
            document.getElementById('chat').scrollTop = document.getElementById('chat').scrollHeight;
        }
    </script>
</body>
</html>
