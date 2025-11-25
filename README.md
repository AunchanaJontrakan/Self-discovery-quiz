<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>แบบทดสอบค้นพบตัวเอง</title>
  <style>
    body { font-family: 'Sarabun', sans-serif; background: #eef; margin: 0; padding: 0; }
    .container { max-width: 600px; margin: 40px auto; background: #fff; padding: 30px 20px; border-radius: 10px; box-shadow: 0 4px 12px #bbb;}
    h1 { text-align: center; color: #335;}
    .question { margin-bottom: 20px; }
    .answers label { display: block; margin-bottom: 8px; }
    .hide { display: none; }
    #result { font-size: 1.2em; text-align: center; margin-top: 20px;}
    button { padding: 8px 20px; font-size: 1em; background: #347; color: #fff; border: none; border-radius: 5px; cursor: pointer;}
  </style>
</head>
<body>
  <div class="container">
    <h1>แบบทดสอบค้นพบตัวเอง (Self-discovery Quiz)</h1>
    <form id="quizForm">
      <div id="quiz"></div>
      <button type="button" id="nextBtn">ถัดไป</button>
      <button type="submit" id="submitBtn" class="hide">ส่งคำตอบ</button>
    </form>
    <div id="result" class="hide"></div>
  </div>

  <script>
    const questions = [
      { q: "ข้อ 1: คุณชอบใช้เวลาว่างกับอะไร?", 
        options: ["อ่านหนังสือ", "เล่นกีฬา", "พบปะเพื่อน", "ทำงานศิลปะ"] },
      { q: "ข้อ 2: คุณคิดว่าคุณมีจุดแข็งอะไร?",
        options: ["ความคิดสร้างสรรค์", "ความขยัน", "ความกล้าแสดงออก", "ความรอบคอบ"] },
      { q: "ข้อ 3: ถ้าให้เลือกงานที่อยากทำ จะเลือกอะไร?",
        options: ["นักเขียน", "นักกีฬา", "นักธุรกิจ", "ศิลปิน"] },
      { q: "ข้อ 4: คุณรับมือกับความเครียดยังไง?",
        options: ["พักผ่อน", "ออกกำลังกาย", "พูดคุยกับเพื่อน", "ระบายผ่านงานศิลปะ"] },
      { q: "ข้อ 5: คุณอยากพัฒนาทักษะด้านใดมากที่สุด?",
        options: ["การคิดวิเคราะห์", "ความเป็นผู้นำ", "การสร้างมนุษยสัมพันธ์", "ความคิดสร้างสรรค์"] },
      { q: "ข้อ 6: คุณชื่นชอบกิจกรรมรูปแบบไหน?",
        options: ["กิจกรรมเงียบๆ", "การแข่งขัน", "การพบปะสังคม", "งานศิลปะ"] },
      { q: "ข้อ 7: คุณให้ความสำคัญกับอะไรในชีวิต?",
        options: ["ความรู้", "สุขภาพ", "มิตรภาพ", "ความสุข"] },
      { q: "ข้อ 8: คุณอยากประสบความสำเร็จในเรื่องใด?",
        options: ["การเรียน", "กีฬา", "การงาน", "ศิลปะ/ดนตรี"] },
      { q: "ข้อ 9: ถ้าต้องแก้ปัญหา คุณจะ...",
        options: ["คิดแบบมีเหตุมีผล", "ลงมือทันที", "ขอคำปรึกษา", "หาวิธีสร้างสรรค์"] },
      { q: "ข้อ 10: คุณพบความสุขตอนใดมากที่สุด?",
        options: ["ตอนอ่าน-เรียนรู้", "ตอนแข่งขัน", "ตอนอยู่กับเพื่อน", "ตอนทำสิ่งใหม่ๆ"] },
    ];

    let current = 0;
    let answers = [];

    function showQuestion(idx) {
      const q = questions[idx];
      let html = `<div class="question"><b>${q.q}</b></div><div class="answers">`;
      q.options.forEach((opt, i) => {
        html += `<label><input type="radio" name="answer" value="${i}"> ${opt}</label>`;
      });
      html += `</div>`;
      document.getElementById('quiz').innerHTML = html;
      // Reset button visibility
      document.getElementById('nextBtn').classList.remove('hide');
      document.getElementById('submitBtn').classList.add('hide');
    }

    showQuestion(0);

    document.getElementById('nextBtn').onclick = function() {
      const selected = document.querySelector('input[name="answer"]:checked');
      if (!selected) {
        alert("กรุณาเลือกคำตอบก่อนนะคะ");
        return;
      }
      answers[current] = Number(selected.value);
      current++;
      if (current < questions.length) {
        showQuestion(current);
      } else {
        document.getElementById('quiz').innerHTML = "";
        document.getElementById('nextBtn').classList.add('hide');
        document.getElementById('submitBtn').classList.remove('hide');
      }
    };

    document.getElementById('quizForm').onsubmit = function(e) {
      e.preventDefault();
      // คำนวณผลลัพธ์ตัวอย่างแบบง่ายๆ
      let score = [0, 0, 0, 0];
      answers.forEach(ans => score[ans]++);
      let max = Math.max(...score);
      let type = score.indexOf(max);
      let descs = [
        "คุณรักการเรียนรู้และคิดวิเคราะห์ ชอบทำกิจกรรมเงียบๆ",
        "คุณชื่นชอบกีฬาและการแข่งขัน รักสุขภาพ",
        "คุณเป็นคนรักสังคมและมิตรภาพ ชอบอยู่กับผู้คน",
        "คุณมีความสร้างสรรค์สูง มีศิลปะและจินตนาการ"
      ];
      document.getElementById('result').innerHTML = 
        `<b>ผลลัพธ์:</b> <br>${descs[type]}<br><br>
         <a href="" onclick="location.reload(); return false;">เริ่มใหม่อีกครั้ง</a>`;
      document.getElementById('result').classList.remove('hide');
      document.getElementById('submitBtn').classList.add('hide');
    };
  </script>
</body>
</html>
