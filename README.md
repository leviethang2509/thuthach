<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Split Image with Grid and Excel Answer</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 0;
      max-width: fit-content;
      margin-top: 20px;
    }

    .grid img {
      width: 100%;
      height: auto;
      display: block;
      object-fit: cover;
      margin: 0;
      padding: 0;
      border: none;
      visibility: hidden;
      image-rendering: pixelated;
    }

    .grid .visible {
      visibility: visible;
    }

    .container {
      display: flex;
      justify-content: center;
      align-items: flex-start;
      min-height: 100vh;
    }

    #defaultImage {
      display: none;
      width: 40%;
      height: auto;
      object-fit: contain;
    }

    .flying-object {
      position: fixed;
      top: 20%;
      left: -150px;
      width: 120px;
      height: auto;
      z-index: 9999;
      pointer-events: none;
      animation: flyAcross 12s linear infinite;
    }

    @keyframes flyAcross {
      0% { transform: translateX(0); }
      100% { transform: translateX(120vw); }
    }

    .flying-tank {
      position: fixed;
      bottom: 0;
      right: -200px;
      width: 150px;
      height: auto;
      z-index: 9999;
      pointer-events: none;
      animation: tankMoveLeft 15s linear infinite;
    }
    
    @keyframes tankMoveLeft {
      0% { transform: translateX(0); }
      100% { transform: translateX(-120vw); }
    }
    

    .tank-wrapper {
      position: relative; /* Đảm bảo tank-wrapper có position: relative */
      bottom: 50px;
      left: 100px;
      width: 150px; /* Kích thước mới của tank-wrapper */
      height: auto;
      z-index: 1000;
      display: flex;
      align-items: center; /* Căn chỉnh phần tử con theo chiều dọc */
    }
    
    .fire {
      position: absolute;
      width: 20px;
      height: 20px;
      background: radial-gradient(circle, orange, red, transparent);
      border-radius: 50%;
      left: -10px;  /* Đặt lửa cách xe tăng 10px */
      top: 50%;
      transform: translateY(-50%);
      animation: flame 0.2s infinite alternate;
      opacity: 0.8;
      pointer-events: none;
      z-index: 999;  /* Đảm bảo lửa luôn nằm trên xe tăng */
    }
    
    
    @keyframes flame {
      0% {
        transform: translateY(-50%) scale(1);
        opacity: 0.8;
      }
      100% {
        transform: translateY(-50%) scale(1.4);
        opacity: 0.4;
      }
    }
    

    .flying-doclap {
      position: fixed;
      bottom: 20px;
      left: 20px;
      width: 150px;
      height: auto;
      z-index: 1000;
      pointer-events: none;
    }
    .doclap-wrapper {
      position: fixed;
      bottom: 20px;
      left: 20px;
      width: 150px;
      height: auto;
      z-index: 1000;
      pointer-events: none;
    }
    
    .flying-doclap-img {
      width: 100%;
      height: auto;
      display: none;
    }
    
    #victoryMessage {
      position: absolute;
      bottom: 100%;
      left: 50%;
      transform: translateX(-50%) scale(0.5);
      font-size: 22px;
      font-weight: bold;
      color: red;
      text-shadow: 2px 2px 5px yellow;
      opacity: 0;
      transition: all 0.5s ease;
      white-space: nowrap;
    }
    
    #victoryMessage.show {
      opacity: 1;
      transform: translateX(-50%) scale(1.2);
    }
    .flying-object {
      animation: flyAcross 10s linear forwards; /* bay 1 lần rồi dừng */
    }
    
    .flying-tank {
      animation: tankMoveLeft 15s linear forwards;
    }
    .modal {
      display: none;
      position: fixed;
      z-index: 999;
      padding-top: 100px;
      left: 0; top: 0;
      width: 100%; height: 100%;
      background-color: rgba(0,0,0,0.5);
    }
    
    .modal-content {
      margin: auto;
      background-color: #fff;
      padding: 20px;
      width: 60%;
      border-radius: 8px;
      position: relative;
      box-shadow: 0 5px 10px rgba(0,0,0,0.3);
    }
    
    .close {
      color: #aaa;
      font-size: 28px;
      font-weight: bold;
      position: absolute;
      top: 10px; right: 20px;
      cursor: pointer;
    }
    
    button#continueUpload {
      margin-top: 15px;
      padding: 10px 20px;
      background-color: #4caf50;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    
  </style>
</head>
<body>

<img id="defaultImage" src="./44.jpg" crossOrigin="anonymous">
<h3>Upload Excel chứa đáp án (ví dụ: "1A 10C 25B"):</h3>
<input type="file" id="uploadExcel" accept=".xlsx"><br><br>
<!-- Modal lưu ý -->
<div id="noteModal" class="modal">
  <div class="modal-content">
    <span class="close">&times;</span>
    <h3>📌 Lưu ý khi nhập file Excel</h3>
    <ul>
      <li>📎 Đáp án theo định dạng: <strong>1A, 2B, 3C..mỗi đáp án nên xuống dòng</strong></li>
      <li>📁 Chỉ dùng <strong>cột đầu tiên</strong> để nhập</li>
      <li>⛔ Khi âm thanh bị lỗi hãy loard lại trang</li>

    </ul>
  </div>
</div>
<button id="openNoteModal" style="
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 1000;
  background-color: #28a745;
  color: white;
  padding: 10px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
">
  📌 Hướng dẫn nhập Excel
</button>

<div class="container">
  <div class="grid" id="result"></div>
</div>

<!-- SheetJS -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<!-- Fireworks -->
<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    const answerMap = new Map();
    const soSanh = [
 "1D", "2B", "3B", "4A", "5A", "6B", "7B", "8A", "9A", "10A",
"11B", "12B", "13C", "14B", "15A", "16B", "17A", "18B", "19C", "20D",
"21B", "22C", "23B", "24B", "25A", "26B", "27C", "28B", "29D", "30D",
"31B", "32B", "33D"
    ];
    const uploadInput = document.getElementById('uploadExcel');
    const noteModal = document.getElementById('noteModal');
    const closeModal = document.querySelector('.close');
    const continueBtn = document.getElementById('continueUpload');
    
    
    // Đóng modal
    closeModal.onclick = () => {
      noteModal.style.display = 'none';
      uploadInput.value = ''; // Reset input nếu đóng
    };
    openNoteModal.onclick = () => {
      noteModal.style.display = 'block';
    };
  
    // Đóng modal khi nhấn dấu X
    closeModal.onclick = () => {
      noteModal.style.display = 'none';
    };
  
    // Đóng modal khi nhấn ra ngoài vùng modal-content
    window.onclick = (event) => {
      if (event.target === noteModal) {
        noteModal.style.display = 'none';
      }
    };
    // Tiếp tục xử lý sau khi nhấn "Tôi đã hiểu"
 
    
    let isTankPassing = false;

    document.getElementById('uploadExcel').addEventListener('change', function (e) {
      const file = e.target.files[0];
      const reader = new FileReader();

      reader.onload = function (event) {
        const data = new Uint8Array(event.target.result);
        const workbook = XLSX.read(data, { type: 'array' });
        const sheetName = workbook.SheetNames[0];
        const worksheet = workbook.Sheets[sheetName];
        const json = XLSX.utils.sheet_to_json(worksheet, { header: 1 });

        const answerString = json.map(row => row[0])
          .filter(cell => !!cell)
          .join(' ')
          .replace(/\s+/g, '')
          .toUpperCase();

        const regex = /(\d{1,2}|100)[A-D]/g;
        const matches = answerString.match(regex);

        answerMap.clear();
        function animate() {
          checkTankCrossing();
          requestAnimationFrame(animate);
        }
      
        animate();
        if (matches) {
          matches.forEach(ans => {
            const number = parseInt(ans.match(/\d+/)[0]);
            const choice = ans.match(/[A-D]/)[0];
            answerMap.set(number, choice);
          });

          const img = document.getElementById('defaultImage');
          if (img.complete) {
            processImage(img);
          } else {
            img.onload = () => processImage(img);
          }
        } else {
          document.getElementById('result').innerHTML = 'Không tìm thấy đáp án phù hợp.';
        }
      };

      reader.readAsArrayBuffer(file);
    });



   // Biến toàn cục để lưu số câu trả lời đúng
   let correctAnswers = 0;

   function processImage(img) {
     const rows = 11;
     const cols = 3;
     const tileWidth = img.naturalWidth / cols;
     const tileHeight = img.naturalHeight / rows;
   
     const result = document.getElementById('result');
     result.innerHTML = '';
   
     let count = 1;
     let wrongAnswers = [];
   
     correctAnswers = 0; // Reset mỗi lần xử lý
   
     for (let y = 0; y < rows; y++) {
       for (let x = 0; x < cols; x++) {
         const canvas = document.createElement('canvas');
         canvas.width = tileWidth;
         canvas.height = tileHeight;
         const ctx = canvas.getContext('2d');
   
         ctx.drawImage(img, x * tileWidth, y * tileHeight, tileWidth, tileHeight, 0, 0, tileWidth, tileHeight);
   
         const imgElement = document.createElement('img');
         imgElement.src = canvas.toDataURL();
   
         const container = document.createElement('div');
         container.appendChild(imgElement);
   
         const currentAnswer = answerMap.get(count);
         const fullAnswer = `${count}${currentAnswer}`;
   
         if (currentAnswer && soSanh.includes(fullAnswer)) {
           imgElement.classList.add('visible');
           correctAnswers++;
         } else {
           wrongAnswers.push(count);
         }
   
         result.appendChild(container);
         count++;
       }
     }
   
     // Kiểm tra kết quả
     if (correctAnswers === 33) {
       alert('🎉 Chúc mừng! Bạn đã hoàn thành thử thách!');
       playVictoryMusic();
       hienDocLap();
     } else if (correctAnswers > 0 && correctAnswers < 33) {
      anDocLap();
    
      if (wrongAnswers.length > 0) {
        alert(`❌ Bạn đã sai các câu: ${wrongAnswers.join(', ')}`);
      }
    }
   
     // Hàm bổ sung cho kiểm tra từng câu
     function checkAnswer(isCorrect) {
       if (isCorrect && correctAnswers < 33) {
         stopAllEffects();
       }
     }
   
   
function hienDocLap() {
  const doclapElement = document.getElementById("doclap");
  const flyingImg = document.querySelector(".flying-doclap-img");

  if (doclapElement) {
    doclapElement.style.display = "block";
  }

  if (flyingImg) {
    flyingImg.style.display = "block";
  }
}
function anDocLap() {
  const doclapElement = document.getElementById("doclap");
  const flyingImg = document.querySelector(".flying-doclap-img");

  if (doclapElement) {
    doclapElement.style.display = "none";
  }

  if (flyingImg) {
    flyingImg.style.display = "none";
  }
}


// Hàm phát nhạc khi đạt đủ 33 câu đúng
function playVictoryMusic() {
const audio = new Audio('./HaoKhiVietNam.mp3'); // Thay 'path_to_victory_music.mp3' bằng đường dẫn đến file nhạc của bạn
audio.play();
}
function stopAllEffects() {
  // Dừng pháo hoa
  clearInterval(fireworkInterval); 
  cancelAnimationFrame(fireworkAnimationFrame);

  // Ẩn máy bay
  const airplanes = document.querySelectorAll('.flying-object');
  airplanes.forEach(airplane => airplane.style.display = 'none');

  // Ẩn xe tăng
  const tanks = document.querySelectorAll('.flying-tank');
  tanks.forEach(tank => tank.style.display = 'none');
  
  // Ẩn các hiệu ứng khác nếu có
  const fires = document.querySelectorAll('.fire');
  fires.forEach(fire => fire.style.display = 'none');
  
  // Dừng tất cả các animation (nếu có)
  const allAnimations = document.querySelectorAll('*');
  allAnimations.forEach(element => {
    element.style.animation = 'none'; // Hủy bỏ animation
  });

  // Dừng mọi hoạt động liên quan đến confetti
  confetti.reset();
}



}

    


    

    function startConfetti(originX, originY) {
      confetti({
        particleCount: 50,
        angle: 90,
        spread: 50,
        startVelocity: 50,
        origin: { x: originX, y: originY },
        colors: ['#ff0000', '#ffaa00', '#ffff00']
      });
    }
    let fireworkInterval;
    let fireworkAnimationFrame;
    let fireworkFired = false;

    function checkTankCrossing() {
      const tanks = document.querySelectorAll('.flying-tank');
      const doclap = document.getElementById('doclap');
    
      tanks.forEach(tank => {
        const tankRect = tank.getBoundingClientRect();
        const doclapRect = doclap.getBoundingClientRect();
    
        if (
          tankRect.right >= doclapRect.left &&
          tankRect.left <= doclapRect.right &&
          !fireworkFired
        ) {
          fireworkFired = true;
          launchFireworksFromDoclap();
    
          setTimeout(() => {
            fireworkFired = false;
          }, 1000);
        }
      });
    }
    
    function launchFireworksFromDoclap() {
      const doclap = document.getElementById('doclap');
      const rect = doclap.getBoundingClientRect();
    
      // 🎆 Bắn pháo hoa
      confetti({
        particleCount: 20,
        angle: 90,
        spread: 60,
        startVelocity: 50,
        origin: {
          x: (rect.left + rect.width / 2) / window.innerWidth,
          y: (rect.top + rect.height / 2) / window.innerHeight,
        },
        colors: ['#ff0000', '#ffff00', '#00ff00', '#00ccff'],
      });
    
      // ✅ Hiển thị chữ "TOÀN THẮNG!"
      const message = document.getElementById("victoryMessage");
      message.classList.add("show");
    
      // ⏱️ Sau 5 giây thì ẩn đi
      setTimeout(() => {
        message.classList.remove("show");
      }, 5000);
    }
    
// Hàm khởi tạo máy bay mới và thêm hiệu ứng
function createAirplane(id) {
  // Kiểm tra xem số câu trả lời đúng có bằng 33 hay không
  if (correctAnswers === 33) {
    const airplane = document.createElement("img");
    airplane.src = "./—Pngtree—jet fighter illustration_8476956.png";
    airplane.className = "flying-object";
    airplane.id = id;

    // Thiết lập animation ngay khi máy bay được tạo
    airplane.style.animation = 'flyAcross 10s linear forwards'; // Thêm hiệu ứng bay ngay khi tạo

    document.body.appendChild(airplane);

    // Gọi hàm để bắt đầu hiệu ứng pháo hoa
    launchFireworks(airplane);

    return airplane;
  }
  // Nếu không đạt 33 câu trả lời đúng, không làm gì
  return null;
}

// Hàm tạo hiệu ứng pháo hoa




function launchFireworks(airplane) {
  // Nếu đã có hiệu ứng pháo hoa đang chạy, dừng lại
  if (fireworkInterval) clearInterval(fireworkInterval);  // Xóa interval cũ nếu có
  if (fireworkAnimationFrame) cancelAnimationFrame(fireworkAnimationFrame);  // Xóa animation frame cũ nếu có

  // Tạo pháo hoa liên tục với số lượng ít
  fireworkInterval = setInterval(() => {
    const rect = airplane.getBoundingClientRect();
    confetti({
      particleCount: 200, // Số lượng hạt pháo hoa ít
      angle: 180,
      spread: 30, // Giảm độ lan tỏa để ít pháo hoa hơn
      startVelocity: 20, // Giảm tốc độ pháo hoa
      origin: {
        x: (rect.left + rect.width / 2) / window.innerWidth,
        y: (rect.top + rect.height / 2) / window.innerHeight,
      }
    });

    // Tạo vòng lặp pháo hoa chạy liên tục
    confetti({
      particleCount: 3, // Số lượng pháo hoa ít
      angle: 60,
      spread: 45, // Giảm độ lan tỏa để ít pháo hoa hơn
      origin: { x: 0 },
    });
    confetti({
      particleCount: 3, // Số lượng pháo hoa ít
      angle: 120,
      spread: 45, // Giảm độ lan tỏa
      origin: { x: 1 },
    });
  }, 1000); // Điều chỉnh khoảng thời gian giữa các lần bắn (mỗi giây)
}


// Gọi hàm tạo máy bay liên tục
setInterval(() => createAirplane('airplane_' + Date.now()), 7000);  // Mỗi 7 giây tạo 1 máy bay mới



// 🚓 Tạo xe tăng liên tục
// Hàm khởi tạo xe tăng mới và thêm hiệu ứng
function createTank(id) {
  // Kiểm tra nếu số câu trả lời đúng là 33
  if (correctAnswers === 33) {
    const wrapper = document.createElement("div");
    wrapper.className = "tank-wrapper";
    wrapper.id = id;

    const tankImg = document.createElement("img");
    tankImg.src = "./xetang.png";
    tankImg.className = "flying-tank";

    const fire = document.createElement("div");
    fire.className = "fire";

    wrapper.appendChild(tankImg);
    wrapper.appendChild(fire);

    document.body.appendChild(wrapper);

    return wrapper;
  }
  // Nếu không đủ điều kiện, không làm gì
  return null;
}


// 🎬 Bắt đầu tạo máy bay & xe tăng sau mỗi vài giây
setInterval(createAirplane, 7000); // Mỗi 7 giây có 1 máy bay
setInterval(createTank, 9000);     // Mỗi 9 giây có 1 xe tăng

    function stopFireworks() {
      const airplane = document.getElementById('airplane');
      airplane.style.display = 'none';
      confetti.reset();
    }

  });
</script>

<img src="./—Pngtree—jet fighter illustration_8476956.png" class="flying-object" id="airplane" style="display: none;" />
<div class="tank-wrapper" id="tank">
  <img src="" class="flying-tank" />
  <div class="fire"></div> <!-- Lửa đã được di chuyển vào đây -->
</div>

<div class="doclap-wrapper" id="doclap">
  <img src="./đinhoclap.png" class="flying-doclap-img" />
  <div id="victoryMessage">TOÀN THẮNG!</div>
</div>

</body>
</html>
