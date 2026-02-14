<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<title>เกมฝึกออกเสียงคำศัพท์</title>

<style>
/* พื้นหลังไล่สี */
body {
    font-family: 'Segoe UI', Tahoma, sans-serif;
    background: linear-gradient(135deg, #a18cd1, #6dd5ed, #fbc2eb);
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0;
}

/* กล่องเกม */
.container {
    background: white;
    padding: 50px 60px;
    border-radius: 25px;
    box-shadow: 0 20px 50px rgba(0,0,0,0.15);
    text-align: center;
    width: 420px;
    animation: fadeIn 0.6s ease;
}

@keyframes fadeIn {
    from {opacity:0; transform: translateY(30px);}
    to {opacity:1; transform: translateY(0);}
}

h1 {
    color: #6a5acd;
    margin-bottom: 25px;
}

/* คำศัพท์ */
#word {
    font-size: 54px;
    font-weight: bold;
    color: #009dff;
    margin: 20px 0;
}

/* สถานะ */
#status {
    font-size: 26px;
    font-weight: bold;
    min-height: 35px;
}

/* คะแนน */
#score {
    font-size: 22px;
    margin: 15px 0 25px;
    color: #444;
}

/* ปุ่ม */
button {
    padding: 12px 28px;
    font-size: 18px;
    border: none;
    border-radius: 12px;
    background: linear-gradient(45deg,#7f7fd5,#86a8e7,#91eae4);
    color: white;
    cursor: pointer;
    transition: 0.25s;
    box-shadow: 0 5px 15px rgba(0,0,0,0.15);
}

button:hover {
    transform: translateY(-2px) scale(1.05);
    box-shadow: 0 10px 25px rgba(0,0,0,0.25);
}
</style>
</head>

<body>

<div class="container">
    <h1>ตรวจจับเสียงพูดคำภาษาไทย</h1>

    <div id="word">กิ่ง</div>
    <div id="status"></div>
    <div id="score">คะแนน: 0</div>

    <button id="startButton">เริ่มฟังเสียง</button>
    <button id="nextButton" style="display:none;">คำถัดไป</button>
</div>

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
    recognition.lang = 'th-TH';

    startButton.addEventListener('click', () => {
        statusElement.style.color = "#333";
        statusElement.textContent = "กำลังฟังเสียง...";
        recognition.start();
    });

    recognition.onresult = (event) => {
        const spokenWord = event.results[0][0].transcript.trim();

        if (spokenWord === words[currentIndex]) {
            statusElement.textContent = "ถูกต้อง!";
            statusElement.style.color = "#28a745";
            score++;
            scoreElement.textContent = `คะแนน: ${score}`;
        } else {
            statusElement.textContent = "ผิด!";
            statusElement.style.color = "#e63946";
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
            wordElement.textContent = "จบเกม!";
            statusElement.textContent = `คะแนนรวม ${score} / ${words.length}`;
            statusElement.style.color = "#6a5acd";
            nextButton.style.display = "none";
        }
    });
}
</script>

</body>
</html>
