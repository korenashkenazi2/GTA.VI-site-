<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>GTA VI - ספירה לאחור</title>
  <style>
    body {
      margin: 0;
      font-family: 'Arial', sans-serif;
      background: linear-gradient(to bottom, #d664de, #f36baf, #f39c53);
      color: white;
      text-align: center;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
    }

    h1 {
      font-size: 2.5em;
    }

    .countdown {
      font-size: 2em;
      display: flex;
      gap: 80px;
      margin-bottom: 30px;
      flex-wrap: wrap;
      justify-content: center;
    }

    .unit {
      display: flex;
      flex-direction: column;
      align-items: center;
      font-weight: bold;
    }

    .unit span:first-child {
      font-size: 1.5em;
    }

    .progress-wrapper {
      position: relative;
      width: 80%;
      max-width: 600px;
      height: 50px;
      margin-bottom: 30px;
    }

    .progress-bar-container {
      width: 100%;
      height: 25px;
      background: #000;
      border-radius: 20px;
      overflow: hidden;
      box-shadow: 0 0 10px #ff00cc;
    }

    .progress-bar-fill {
      height: 100%;
      width: 0%;
      background: linear-gradient(to left, #5e2750, #ff9966, #ff00cc, #3333ff);
      border-radius: 20px;
      transition: width 2.5s ease;
    }

    .progress-percent {
      position: absolute;
      top: -30px;
      left: 50%;
      transform: translateX(-50%);
      font-weight: bold;
      color: #fff;
    }

    .quote {
      max-width: 80%;
      font-size: 1.5em;
      font-weight: bold;
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <h1>הזמן שנשאר</h1>

  <div class="countdown">
    <div class="unit"><span id="days">?</span><span>ימים</span></div>
    <div class="unit"><span id="hours">?</span><span>שעות</span></div>
    <div class="unit"><span id="minutes">?</span><span>דקות</span></div>
    <div class="unit"><span id="seconds">?</span><span>שניות</span></div>
  </div>

  <div class="progress-wrapper">
    <div class="progress-percent" id="progressPercent"></div>
    <div class="progress-bar-container">
      <div class="progress-bar-fill" id="progressBar"></div>
    </div>
  </div>

  <div class="quote" id="dailyQuote"></div>

  <script>
    const targetDate = new Date("2026-05-26T00:00:00").getTime();
    const startDate = new Date("2014-07-01T00:00:00").getTime();

    function updateCountdown() {
      const now = new Date().getTime();
      const diff = targetDate - now;
      if (diff <= 0) {
        document.querySelector('.countdown').innerHTML = "המשחק שוחרר!";
        return;
      }
      const days = Math.floor(diff / (1000 * 60 * 60 * 24));
      const hours = Math.floor((diff / (1000 * 60 * 60)) % 24);
      const minutes = Math.floor((diff / (1000 * 60)) % 60);
      const seconds = Math.floor((diff / 1000) % 60);
      document.getElementById("days").innerText = days;
      document.getElementById("hours").innerText = hours;
      document.getElementById("minutes").innerText = minutes;
      document.getElementById("seconds").innerText = seconds;
    }

    function animateProgressBarToCurrent() {
      const now = new Date().getTime();
      const total = targetDate - startDate;
      const elapsed = now - startDate;
      const actualPercent = Math.min(100, Math.max(0, (elapsed / total) * 100));
      const overshoot = Math.min(100, actualPercent + 5);

      const progressBar = document.getElementById("progressBar");
      const progressText = document.getElementById("progressPercent");

      progressBar.style.width = "0%";
      progressText.innerText = "0%";

      setTimeout(() => {
        progressBar.style.width = overshoot + "%";
        progressText.innerText = overshoot.toFixed(2) + "%";

        setTimeout(() => {
          progressBar.style.width = actualPercent.toFixed(2) + "%";
          progressText.innerText = actualPercent.toFixed(2) + "%";
        }, 2000);
      }, 100);
    }

    const quotes = [
      "כוח ✦ כל צעד מקרב אותך ל-GTA VI. היה מוכן!",
      "המטרה קרובה – אל תעצור עכשיו!",
      "גם הסבלנות היא כוח במשחק הגדול.",
      "הציפייה בונה את ההתרגשות.",
      "כל שנייה מקרבת אותך לעולם חדש."
    ];

    function showDailyQuote() {
      const today = new Date();
      const start = new Date(today.getFullYear(), 0, 0);
      const diff = today - start;
      const day = Math.floor(diff / (1000 * 60 * 60 * 24));
      const quote = quotes[day % quotes.length];
      document.getElementById("dailyQuote").innerText = quote;
    }

    setInterval(updateCountdown, 1000);
    updateCountdown();
    animateProgressBarToCurrent();
    showDailyQuote();
  </script>
</body>
</html>
