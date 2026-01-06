<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>อะไรเอ่ยยยยย???</title>

  <!-- Tailwind -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- ฟอนต์ Mitr -->
  <link href="https://fonts.googleapis.com/css2?family=Mitr&display=swap" rel="stylesheet">

  <style>
    body {
      font-family: 'Mitr', sans-serif;
      background-image: url('https://i.pinimg.com/736x/37/a0/ee/37a0ee278c0896560ca1f44235272a85.jpg');
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      transition: background 0.5s;
      text-shadow: 0 1px 2px rgba(0, 0, 0, 0.25);
      position: relative;
    }

    body::before {
      content: "";
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.3);
      z-index: -1;
    }

    .fade-in {
      opacity: 0;
      transform: translateY(20px);
      animation: fadeInUp 0.8s ease forwards;
    }

    @keyframes fadeInUp {
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }
  </style>
</head>

<body class="flex items-center justify-center min-h-screen p-4 text-gray-800">

  <!-- หน้าใส่รหัสผ่าน -->
  <div id="passwordPage" class="max-w-md w-full bg-purple-200/80 backdrop-blur-md border border-purple-300 p-8 rounded-2xl shadow-xl text-center text-gray-800">
    <h2 class="text-2xl font-bold text-purple-700">ขอปล้นวันเกิดหน่อยจิ :3</h2>
    <input
      id="passwordInput"
      type="password"
      placeholder="กรอกวันเดือนปีเกิดหน่อยสิ..."
      class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-300"
    />
    <p id="errorText" class="text-red-500 hidden mt-2">รหัสไม่ถูกต้อง ลองอีกครั้งนะคะ</p>
    <button
      id="enterBtn"
      class="w-full bg-purple-500 hover:bg-purple-600 text-white py-2 mt-4 rounded-lg transition"
    >
      มาตรงนี้ๆ จิ้มๆ
    </button>
  </div>

  <!-- หน้าแสดงข้อความ -->
  <div id="messagePage" class="hidden max-w-2xl bg-purple-200/80 backdrop-blur-md border border-purple-300 shadow-xl rounded-3xl p-8 space-y-6 text-center text-gray-800">
    <h1 class="text-3xl font-bold text-purple-700">อะไรน้าาาาา?</h1>

    <div id="messageContainer" class="space-y-6 text-lg leading-relaxed text-left"></div>

    <button id="nextBtn" class="mt-4 px-6 py-2 bg-purple-500 text-white rounded-full hover:bg-purple-600 transition">
      จิ้มๆตรงนี้ :3
    </button>

    <button id="restartBtn" class="hidden px-6 py-2 bg-gray-500 text-white rounded-full hover:bg-gray-600 transition">
      อยากอ่านใหม่หรอ? กดตรงนี้สิ >///<
    </button>

    <p class="text-sm text-gray-600 mt-6 italic">ช่วยเป็นคนที่น่ารักแบบนี้ต่อไปด้วยนะคะ</p>

    <!-- container ของ YouTube -->
    <div id="ytPlayer"></div>
  </div>

  <!-- YouTube API -->
  <script src="https://www.youtube.com/iframe_api"></script>

  <script>
    const correctPassword = "13122000"; // รหัสผ่าน
    const passwordInput = document.getElementById("passwordInput");
    const enterBtn = document.getElementById("enterBtn");
    const errorText = document.getElementById("errorText");
    const passwordPage = document.getElementById("passwordPage");
    const messagePage = document.getElementById("messagePage");
    const container = document.getElementById("messageContainer");
    const nextBtn = document.getElementById("nextBtn");
    const restartBtn = document.getElementById("restartBtn");

    const messages = [
      "ᰔᩚ สวัสดีค้าบคนเก่ง",
      "ᰔᩚ ไหนวันนี้วันอะไรเอ่ยยย???",
      "ᰔᩚ ติ๊กต๊อก ติ๊กต๊อก!!",
      "ᰔᩚ ใช่แล้ววว วันนี้วันเกิดคนเก่งไงคะ",
      "ᰔᩚ HAPPY BIRTHDAY TO YOU ย้อนหลังนะคะ",
      "ᰔᩚ ถึงจะพึ่งรู้จักกันแต่ว่า ฮัจก็อยากทำอะไรสักอย่างให้นะ",
      "ᰔᩚ ช่วงนี้เป็นยังไงบ้างคะ มีความสุขบ้างรึเปล่าเอ่ย?",
      "ᰔᩚ ยังนึกถึงเรื่องในอดีตหรือเรื่องที่ทำให้รู้สึกเศร้าอยู่มั้ยน้าาา",
      "ᰔᩚ ถ้าไม่นึกถึงแล้วก็คงดีนะคะ คุณริรู้มั้ยคะว่าตัวเองเก่งมากเลยน้าา",
      "ᰔᩚ แต่ถ้ายังคงนึกถึงอยู่ก็ไม่เป็นไรเลยนะคะ ทุกอย่างมันต้องใช้เวลา",
      "ᰔᩚ ส่วนฮัจก็จะคอยเป็นกำลังใจให้ และฮัจก็ยินดีรับฟังคุณริเสมอเลย",
      "ᰔᩚ เพราะฮัจก็อยากเห็นคุณริยิ้มเยอะๆค่ะ",
      "ᰔᩚ แต่ก่อนมีคนเคยบอกฮัจว่าเวลาใครสักคนยิ้มจากใจจริง มันดูสวยแล้วก็น่ารักมากเลยน้า",
      "ᰔᩚ เพราะงั้นแล้ว เวลาที่คุณริมีความสุขหรืออยากจะยิ้ม ก็ช่วยยิ้มเยอะๆทีนะคะ",
      "ᰔᩚ สุดท้ายแล้ววววววว",
      "ᰔᩚ ฮัจขอให้คุณริมีความสุขมากๆเลยนะ ขอให้เป็นวันเกิดที่ดีมีแต่คนน่ารักแล้วก็ใจดีรายล้อมคนเก่งเสมอเลยนะคะ",
      "ᰔᩚ ทั้งหมดนี้ฮัจตั้งใจทำมากๆ ถือซะว่าเป็นของขวัญวันเกิดให้คนเก่งคนนี้นะคะ ^-^",
      "ᰔᩚ ถึงแม้ว่ามันจะไม่ได้มีค่าอะไร แต่ฮัจก็หวังว่าคุณริจะชอบนะคะ",
    ];

    let index = 0;
    const typingSpeed = 25;

    // typewriter
    function typeWriter(htmlString, callback) {
      const p = document.createElement("p");
      p.classList.add("fade-in");
      container.appendChild(p);

      let i = 0;
      function type() {
        if (i < htmlString.length) {
          p.innerHTML += htmlString.charAt(i);
          i++;
          setTimeout(type, typingSpeed);
        } else if (callback) {
          callback();
        }
      }
      type();
    }

    function showNextMessage() {
      if (index < messages.length) {
        nextBtn.disabled = true;
        typeWriter(messages[index], () => {
          index++;
          nextBtn.disabled = false;
          if (index === messages.length) {
            nextBtn.style.display = "none";
            restartBtn.classList.remove("hidden");
          }
        });
      }
    }

    // YouTube fade in
    let player;
    function onYouTubeIframeAPIReady() {
      // ต้องมี แต่ยังไม่สร้าง player
    }

    function playMusic() {
      player = new YT.Player('ytPlayer', {
        height: '0',
        width: '0',
        videoId: 'e9ZjFpXyoGA',
        playerVars: {
          start: 22,
          autoplay: 1,
          loop: 1,
          playlist: 'e9ZjFpXyoGA'
        },
        events: {
          onReady: (event) => {
            event.target.setVolume(0);
            event.target.playVideo();

            let volume = 0;
            const fadeInterval = setInterval(() => {
              if (volume < 50) {
                volume += 1;
                player.setVolume(volume);
              } else {
                clearInterval(fadeInterval);
              }
            }, 100);
          }
        }
      });
    }

    // รหัสผ่าน
    enterBtn.addEventListener("click", () => {
      const entered = passwordInput.value.trim();
      if (entered === correctPassword) {
        passwordPage.style.display = "none";
        messagePage.style.display = "block";

        playMusic(); // 🎶 เริ่มเพลง + fade in

        nextBtn.click();
      } else {
        errorText.classList.remove("hidden");
      }
    });

    passwordInput.addEventListener("keyup", (event) => {
      if (event.key === "Enter") enterBtn.click();
    });

    window.onload = () => passwordInput.focus();

    nextBtn.addEventListener("click", showNextMessage);

    restartBtn.addEventListener("click", () => {
      container.innerHTML = "";
      index = 0;
      nextBtn.style.display = "inline-block";
      restartBtn.classList.add("hidden");
      showNextMessage();
    });
  </script>
</body>
</html>
