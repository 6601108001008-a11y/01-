<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<title>เกมฝึกออกเสียงคำศัพท์</title>

<style>
body {
    font-family: sans-serif;
    background-color: #e9e9ef;
    text-align: center;
    padding-top: 80px;
}

h1 {
    font-size: 36px;
    margin-bottom: 30px;
}

#word {
    font-size: 48px;
    color: #0d6efd;
    margin: 20px 0;
}

#status {
    font-size: 24px;
    font-weight: bold;
    margin: 15px 0;
}

#score {
    font-size: 22px;
    margin: 10px 0 20px;
}

button {
    padding: 10px 25px;
    font-size: 18px;
    border: none;
    border-radius: 8px;
    background-color: #0d6efd;
    color: white;
    cursor: pointer;
}

button:hover {
    background-color: #0b5ed7;
}
</style>
</head>

<body>

<h1>ตรวจจับเสียงพูดคำภาษาไทย</h1>

<h2 id="word">กิ่ง</h2>
<p id="status"></p>
<p id="score">คะแนน: 0</p>

<button id="startButton">เริ่มฟังเสียง</button>
<button id="nextButton" style="display:none;">คำถัดไป</button>

<script>
const words = ["กิ่ง","ยอด","ลำต้น","ราก","ต้นกล้า","หน่อ","เนื้อเยื่อ","เมล็ด","ขยายพันธุ์","ตอนกิ่ง"];
let currentIndex = 0;
let score = 0;

const wordElement = document.getElementById('word');
const statusElement = document.getElementById('status');
const startButton = document.getElementById('startButton');
const nextButton = document.getElementById('nextButton');
const scoreElement = document.getElementById('score');

if (!('webkitSpeechRecognition' in window)) {
    statusElement.textContent = "เบราว์เซอร์ของคุณไม่รองรับ Web Speech API";
    startButton.disabled = true;
} else {

    const recognition = new webkitSpeechRecognition();
    recognition.continuous = false;
    recognition.interimResults = false;
    recognition.lang = 'th-TH';

    startButton.addEventListener('click', () => {
        statusElement.style.color = "black";
        statusElement.textContent = "กำลังฟังเสียง...";
        recognition.start();
    });

    recognition.onresult = (event) => {
        const spokenWord = event.results[0][0].transcript.trim();

        if (spokenWord === words[currentIndex]) {
            statusElement.textContent = "ถูกต้อง!";
            statusElement.style.color = "green";
            score++;
            scoreElement.textContent = `คะแนน: ${score}`;
        } else {
            statusElement.textContent = "ผิด!";
            statusElement.style.color = "red";
        }

        startButton.style.display = "none";
        nextButton.style.display = "inline-block";
    };

    nextButton.addEventListener('click', () => {
        currentIndex++;

        if (currentIndex < words.length) {
            wordElement.textContent = words[currentIndex];
            statusElement.textContent = "";
            startButton.style.display = "inline-block";
            nextButton.style.display = "none";
        } else {
            wordElement.textContent = "";
            statusElement.textContent = `เสร็จสิ้น! คะแนนรวมของคุณคือ: ${score} จาก ${words.length}`;
            nextButton.style.display = "none";
        }
    });
}
</script>

</body>
</html>
