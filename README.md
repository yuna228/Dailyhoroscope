# Dailyhoroscope
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>오늘의 운세 뽑기 ✨</title>

  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: "Malgun Gothic", "Apple SD Gothic Neo", sans-serif;
      color: white;
      min-height: 100vh;
      overflow-x: hidden;

      /* 신비한 은하수 배경 */
      background:
        radial-gradient(circle at 20% 20%, rgba(120, 90, 255, 0.35), transparent 25%),
        radial-gradient(circle at 80% 30%, rgba(255, 100, 200, 0.25), transparent 25%),
        radial-gradient(circle at 50% 90%, rgba(50, 150, 255, 0.25), transparent 30%),
        linear-gradient(135deg, #08001c, #160044 45%, #05001a);
    }

    /* 별 */
    body::before {
      content: "✦   ·     ✧       ·   ✦      ·        ✧    ·      ✦";
      position: fixed;
      top: 10px;
      left: 0;
      width: 100%;
      color: rgba(255,255,255,0.7);
      font-size: 24px;
      line-height: 90px;
      word-spacing: 25px;
      pointer-events: none;
    }

    .container {
      width: 92%;
      max-width: 900px;
      margin: 0 auto;
      padding: 55px 0;
      text-align: center;
    }

    h1 {
      font-size: clamp(34px, 7vw, 64px);
      margin: 15px 0 10px;
      text-shadow: 0 0 20px rgba(255,255,255,0.6);
    }

    .subtitle {
      font-size: clamp(17px, 3vw, 23px);
      margin-bottom: 40px;
      opacity: 0.9;
    }

    .zodiac-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 18px;
    }

    .zodiac-card {
      border: 1px solid rgba(255,255,255,0.25);
      border-radius: 22px;
      padding: 22px 10px;
      background: rgba(255,255,255,0.1);
      backdrop-filter: blur(8px);
      color: white;
      cursor: pointer;
      transition: 0.25s;
      min-height: 150px;
    }

    .zodiac-card:hover {
      transform: translateY(-7px) scale(1.03);
      background: rgba(255,255,255,0.18);
      box-shadow: 0 10px 35px rgba(160,100,255,0.35);
    }

    .emoji {
      font-size: 55px;
      display: block;
      margin-bottom: 10px;
    }

    .zodiac-name {
      font-size: 21px;
      font-weight: bold;
    }

    .date {
      font-size: 13px;
      opacity: 0.7;
      margin-top: 7px;
    }

    /* 운세 결과 */
    #result {
      display: none;
      margin-top: 20px;
      animation: appear 0.5s ease;
    }

    @keyframes appear {
      from {
        opacity: 0;
        transform: translateY(20px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .result-card {
      padding: 40px 30px;
      border-radius: 30px;
      background: rgba(255,255,255,0.13);
      border: 1px solid rgba(255,255,255,0.25);
      backdrop-filter: blur(12px);
      box-shadow: 0 15px 50px rgba(0,0,0,0.25);
    }

    .result-emoji {
      font-size: 90px;
    }

    .result-title {
      font-size: clamp(32px, 6vw, 50px);
      margin: 10px 0 25px;
    }

    .fortune-box {
      text-align: left;
      background: rgba(0,0,0,0.2);
      border-radius: 20px;
      padding: 25px;
      margin: 20px 0;
    }

    .fortune-box h3 {
      font-size: 23px;
      margin-top: 0;
    }

    .fortune-box p {
      font-size: 19px;
      line-height: 1.7;
      margin-bottom: 0;
    }

    .compatibility {
      font-size: 21px;
      padding: 18px;
      border-radius: 18px;
      background: rgba(255,215,100,0.12);
    }

    .again {
      border: none;
      border-radius: 15px;
      padding: 16px 30px;
      font-size: 19px;
      font-weight: bold;
      cursor: pointer;
      color: #241047;
      background: #fff;
      transition: 0.2s;
      margin-top: 15px;
    }

    .again:hover {
      transform: scale(1.05);
      box-shadow: 0 5px 25px rgba(255,255,255,0.3);
    }

    @media (max-width: 700px) {
      .zodiac-grid {
        grid-template-columns: repeat(2, 1fr);
        gap: 12px;
      }

      .zodiac-card {
        min-height: 135px;
        padding: 17px 8px;
      }

      .emoji {
        font-size: 45px;
      }

      .zodiac-name {
        font-size: 18px;
      }

      .result-card {
        padding: 30px 18px;
      }
    }

    @media (max-width: 380px) {
      .zodiac-grid {
        grid-template-columns: 1fr 1fr;
      }
    }
  </style>
</head>

<body>

  <div class="container">

    <h1>🔮 오늘의 운세</h1>
    <div class="subtitle">
      당신의 별자리를 선택해 오늘의 운세를 확인해보세요 ✨
    </div>

    <!-- 별자리 선택 화면 -->
    <div id="selection">
      <div class="zodiac-grid" id="zodiacGrid"></div>
    </div>

    <!-- 결과 화면 -->
    <div id="result">
      <div class="result-card">

        <div class="result-emoji" id="resultEmoji"></div>

        <div class="result-title" id="resultTitle"></div>

        <div class="fortune-box">
          <h3>🌙 오늘의 운세</h3>
          <p id="fortuneText"></p>
        </div>

        <div class="compatibility">
          💫 오늘의 궁합 : <strong id="compatible"></strong>
        </div>

        <button class="again" onclick="resetPage()">
          🔄 다시 고르기
        </button>

      </div>
    </div>

  </div>

  <script>
    // 12가지 별자리 데이터
    const zodiacData = [
      {
        name: "양자리",
        emoji: "♈",
        date: "3/21 ~ 4/19",
        fortune: "새로운 일을 시작하기 좋은 날이에요. 자신감을 가지고 먼저 움직인다면 좋은 기회가 찾아올 수 있어요. 주변 사람의 조언에도 귀를 기울여 보세요.",
        compatibility: "사자자리 ♌"
      },
      {
        name: "황소자리",
        emoji: "♉",
        date: "4/20 ~ 5/20",
        fortune: "차분하게 생각할수록 좋은 결과가 생기는 날이에요. 급하게 결정하기보다는 천천히 하나씩 해결해 보세요. 작은 행운이 찾아올 수 있어요.",
        compatibility: "처녀자리 ♍"
      },
      {
        name: "쌍둥이자리",
        emoji: "♊",
        date: "5/21 ~ 6/21",
        fortune: "대화와 소통에서 행운이 들어오는 날이에요. 친구나 가족과 이야기를 나누다 보면 재미있는 아이디어를 얻을 수 있어요.",
        compatibility: "천칭자리 ♎"
      },
      {
        name: "게자리",
        emoji: "♋",
        date: "6/22 ~ 7/22",
        fortune: "따뜻한 사람들과 함께하면 기분이 좋아지는 날이에요. 가까운 사람에게 먼저 다정한 말을 건네보세요. 예상하지 못한 행복을 느낄 수 있어요.",
        compatibility: "물고기자리 ♓"
      },
      {
        name: "사자자리",
        emoji: "♌",
        date: "7/23 ~ 8/22",
        fortune: "당신의 매력이 빛나는 날이에요. 자신 있게 행동하면 주변에서 좋은 평가를 받을 수 있어요. 하고 싶었던 일에 도전해 보세요.",
        compatibility: "양자리 ♈"
      },
      {
        name: "처녀자리",
        emoji: "♍",
        date: "8/23 ~ 9/22",
        fortune: "꼼꼼함이 행운을 가져오는 날이에요. 작은 실수도 미리 확인하면 좋은 결과를 얻을 수 있어요. 오늘은 계획을 세워 움직여 보세요.",
        compatibility: "황소자리 ♉"
      },
      {
        name: "천칭자리",
        emoji: "♎",
        date: "9/23 ~ 10/22",
        fortune: "사람들과의 관계에서 좋은 기운이 가득해요. 혼자 고민하기보다는 믿을 만한 사람과 이야기를 나누면 해결책을 찾을 수 있어요.",
        compatibility: "쌍둥이자리 ♊"
      },
      {
        name: "전갈자리",
        emoji: "♏",
        date: "10/23 ~ 11/21",
        fortune: "집중력이 높아지는 날이에요. 미뤄두었던 일을 하나씩 해결해 보세요. 끝까지 포기하지 않는다면 만족스러운 결과를 얻을 수 있어요.",
        compatibility: "염소자리 ♑"
      },
      {
        name: "사수자리",
        emoji: "♐",
        date: "11/22 ~ 12/21",
        fortune: "새로운 경험이 행운을 가져오는 날이에요. 평소와 다른 길을 선택해 보거나 새로운 사람을 만나보세요. 재미있는 일이 생길 수 있어요.",
        compatibility: "물병자리 ♒"
      },
      {
        name: "염소자리",
        emoji: "♑",
        date: "12/22 ~ 1/19",
        fortune: "꾸준히 노력한 일이 빛을 보는 날이에요. 당장 결과가 보이지 않더라도 포기하지 마세요. 작은 성취가 큰 자신감으로 이어질 거예요.",
        compatibility: "전갈자리 ♏"
      },
      {
        name: "물병자리",
        emoji: "♒",
        date: "1/20 ~ 2/18",
        fortune: "독특한 아이디어가 떠오르는 날이에요. 다른 사람과 조금 다른 생각이라도 자신 있게 표현해 보세요. 뜻밖의 칭찬을 받을 수 있어요.",
        compatibility: "사수자리 ♐"
      },
      {
        name: "물고기자리",
        emoji: "♓",
        date: "2/19 ~ 3/20",
        fortune: "감성이 풍부해지는 날이에요. 좋아하는 음악을 듣거나 편안한 시간을 보내면 새로운 활력을 얻을 수 있어요. 직감을 믿어보세요.",
        compatibility: "게자리 ♋"
      }
    ];

    // 별자리 카드 만들기
    const zodiacGrid = document.getElementById("zodiacGrid");

    zodiacData.forEach((zodiac, index) => {
      const card = document.createElement("div");

      card.className = "zodiac-card";

      card.innerHTML = `
        <span class="emoji">${zodiac.emoji}</span>
        <div class="zodiac-name">${zodiac.name}</div>
        <div class="date">${zodiac.date}</div>
      `;

      card.addEventListener("click", function() {
        showFortune(index);
      });

      zodiacGrid.appendChild(card);
    });

    // 운세 보여주기
    function showFortune(index) {
      const zodiac = zodiacData[index];

      document.getElementById("selection").style.display = "none";
      document.getElementById("result").style.display = "block";

      document.getElementById("resultEmoji").textContent = zodiac.emoji;
      document.getElementById("resultTitle").textContent = zodiac.name;
      document.getElementById("fortuneText").textContent = zodiac.fortune;
      document.getElementById("compatible").textContent = zodiac.compatibility;

      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });
    }

    // 다시 고르기
    function resetPage() {
      document.getElementById("result").style.display = "none";
      document.getElementById("selection").style.display = "block";

      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });
    }
  </script>

</body>
</html>
