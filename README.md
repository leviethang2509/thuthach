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
    .visible {
      opacity: 0;
      transform: scale(0.8);
      animation: popIn 0.4s ease-out forwards;
    }
    
    @keyframes popIn {
      to {
        opacity: 1;
        transform: scale(1);
      }
    }
    #congrats {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 3em;
      background: rgba(255, 255, 255, 0.9);
      padding: 30px 50px;
      border-radius: 20px;
      z-index: 9999;
      animation: pulse 1s infinite;
    }
    
    @keyframes pulse {
      0% { transform: translate(-50%, -50%) scale(1); }
      50% { transform: translate(-50%, -50%) scale(1.05); }
      100% { transform: translate(-50%, -50%) scale(1); }
    }
    
    .hidden {
      display: none;
    }
    @keyframes flyZigzag {
      0% {
        transform: translate(0, 0);
      }
      20% {
        transform: translate(20vw, -20px);
      }
      40% {
        transform: translate(40vw, 20px);
      }
      60% {
        transform: translate(60vw, -20px);
      }
      80% {
        transform: translate(80vw, 20px);
      }
      100% {
        transform: translate(100vw, 0);
      }
    }
    
    .flying-object {
      position: fixed;
      width: 120px;
      z-index: 999;
      pointer-events: none;
    }
    @keyframes flyAcross {
      0% {
        left: -10%; /* Vị trí ban đầu bên ngoài màn hình trái */
      }
      100% {
        left: 110%; /* Vị trí cuối cùng ngoài màn hình phải */
      }
    }
    
    .flying-object {
      position: fixed; /* Đảm bảo máy bay cố định trong viewport */
      top: 20%; /* Đặt vị trí bắt đầu của máy bay */
      width: 50px; /* Đặt kích thước của máy bay */
      height: auto;
      z-index: 10; /* Đảm bảo máy bay xuất hiện phía trên các phần tử khác */
    }
    
    .form-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 50px;
    }
    input[type="text"] {
      padding: 10px;
      font-size: 16px;
      margin-bottom: 10px;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <img id="defaultImage" src="./44.jpg" crossOrigin="anonymous">
  <div id="notification" style="display: none; color: green; font-size: 16px; margin-top: 10px;"></div>
  
<h3>Upload Excel chứa đáp án (ví dụ: "1A 10C 25B"):</h3>
<!-- Form nhập tên -->
<!-- Form nhập tên -->
<!-- Form nhập tên -->
<!-- Form nhập tên -->
<form id="nameForm">
  <input type="text" id="username" placeholder="Nhập tên của bạn" required>
  <button type="submit">Lưu tên</button>
</form>

<!-- Modal lưu ý -->
<div id="noteModal" class="modal">
  <div class="modal-content">
    <span class="close">&times;</span>
    <h3>📌 Lưu ý khi nhập file Excel</h3>
    <ul>
      <li>📎 Đáp án theo định dạng: <strong>1A, 2B, 3C..mỗi đáp án nên xuống dòng</strong></li>
      <li>📁 Chỉ dùng <strong>cột đầu tiên</strong> để nhập</li>
      <li>⛔ Khi âm thanh bị lỗi hãy loard lại trang</li>
     <li>🖼️Có dưới 100 bức ảnh cần được giải mã</li>
      
    </ul>
  </div>
</div>





<!-- Input tải lên file Excel -->
<input type="file" id="uploadExcel" accept=".xlsx" style="display:block"><br><br>

<!-- Thông báo yêu cầu nhập tên -->
<script>
  // Hàm kiểm tra nếu tên đã được lưu trong sessionStorage
  function checkStoredData() {
    const storedUsername = sessionStorage.getItem('username');
    const uploadExcelElement = document.getElementById('uploadExcel');
    const nameFormElement = document.getElementById('nameForm');
    const notification = document.getElementById('notification');
    
    // Kiểm tra xem tên đã được lưu trong sessionStorage
    if (storedUsername) {
      // Nếu đã có tên, ẩn form nhập tên và hiển thị input tải lên file
      nameFormElement.style.display = 'none'; // Ẩn form nhập tên
      uploadExcelElement.style.display = 'block'; // Hiển thị input tải lên file
      notification.style.display = 'none'; // Ẩn thông báo yêu cầu nhập tên
    } else {
      // Nếu chưa có tên, chỉ hiển thị form nhập tên và ẩn input tải lên file
      uploadExcelElement.style.display = 'none'; // Ẩn input tải lên file
      notification.style.display = 'block'; // Hiển thị thông báo yêu cầu nhập tên
      nameFormElement.style.display = 'block'; // Hiển thị form nhập tên
    }
  }

  // Đảm bảo kiểm tra dữ liệu khi DOM đã sẵn sàng
  document.addEventListener('DOMContentLoaded', function() {
    checkStoredData(); // Kiểm tra và cập nhật giao diện sau khi trang tải
  });

  // Xử lý form submit
  document.getElementById('nameForm').addEventListener('submit', function(event) {
    event.preventDefault(); // Ngừng hành động mặc định của form (reload trang)

    const username = document.getElementById('username').value.trim();
    
    if (username) {
      // Lưu tên vào sessionStorage
      sessionStorage.setItem('username', username); // Lưu tên vào sessionStorage
      
      // Thêm thông báo thành công
      const notificationElement = document.getElementById('notification');
      notificationElement.innerHTML = 'Chào mừng bạn, ' + username; // Thêm tên vào thông báo
      notificationElement.style.color = 'green'; // Đổi màu thông báo thành xanh
      
      // Kiểm tra lại và cập nhật giao diện
      checkStoredData(); // Gọi lại để cập nhật giao diện
    } else {
      // Nếu chưa nhập tên, hiển thị thông báo yêu cầu nhập tên
      const notificationElement = document.getElementById('notification');
      notificationElement.style.display = 'block'; // Hiển thị thông báo
    }
  });
</script>





<img id="defaultImage" src="./44.jpg" crossOrigin="anonymous">

<div id="notification" style="display: none; color: green; font-size: 16px; margin-top: 10px;"></div>


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
<div id="congrats" class="hidden">🎉 Chúc mừng bạn đã hoàn thành! 🎉</div>

<!-- SheetJS -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<!-- Fireworks -->
<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

<script>
  function randomizeImage() {
    const imageNames = ['44.jpg', '55.jpg', '66.jpg', '77.jpg', '88.jpg', '99.jpg','10.jpg'];
    const randomIndex = Math.floor(Math.random() * imageNames.length);
    const imgElement = document.getElementById('defaultImage');

    imgElement.src = `./${imageNames[randomIndex]}`;

    // Lưu ảnh vào sessionStorage
    sessionStorage.setItem('lastImage', imageNames[randomIndex]);

    // Hiển thị thông báo
    const notification = document.getElementById('notification');
    //notification.textContent = `Đã thay đổi ảnh thành: ${imageNames[randomIndex]}`; -->
    notification.style.display = 'block';
  }

  // Hàm kiểm tra nếu có ảnh và tên đã lưu trong sessionStorage
  function checkStoredData() {
    const storedUsername = sessionStorage.getItem('username');
    const storedImage = sessionStorage.getItem('lastImage');

    // Kiểm tra tên đã lưu
    if (storedUsername) {
      // Nếu đã có tên, ẩn form và hiển thị thông báo
      const form = document.getElementById('nameForm');
      form.style.display = 'none'; // Ẩn form

      // Hiển thị thông báo về tên đã lưu
      const notification = document.getElementById('notification');
      notification.textContent = `Chào ${storedUsername}!`;
      notification.style.display = 'block'; // Hiển thị thông báo
    }

    // Nếu ảnh đã lưu, hiển thị lại ảnh đó
    if (storedImage) {
      const imgElement = document.getElementById('defaultImage');
      imgElement.src = `./${storedImage}`; // Set lại ảnh đã lưu
    } else {
      randomizeImage(); // Nếu chưa có ảnh, random ảnh mới
    }
  }

  // Gọi hàm checkStoredData khi trang tải
  window.onload = checkStoredData;

  // Xử lý form submit
  document.getElementById('nameForm').addEventListener('submit', function(event) {
    event.preventDefault(); // Ngừng hành động mặc định của form (reload trang)

    const username = document.getElementById('username').value.trim();
    
    if (username) {
      sessionStorage.setItem('username', username); // Lưu tên vào sessionStorage
      alert(`Tên của bạn (${username}) đã được lưu!`);
      // Ẩn form và thông báo sau khi nhập xong
      const form = document.getElementById('nameForm');
      const notification = document.getElementById('notification');
      form.style.display = 'none'; // Ẩn form
      notification.textContent = `Tên của bạn ${username} đã được lưu!`;
      notification.style.display = 'block'; // Hiển thị thông báo thành công
    }
  });
  document.addEventListener("DOMContentLoaded", function () {
    const answerMap = new Map();
    const soSanh = [
    "1A", "2C", "3A", "4C", "5C", "6C", "7B", "8B", "9C", "10B",
    "11B", "12B", "13B", "14C", "15A", "16C", "17C", "18C", "19C", "20C",
    "21C", "22A", "23C", "24B", "25C", "26B", "27C", "28B", "29C", "30A",
    "31C", "32B", "33C"
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
       startEffects();
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


     function startEffects() {
      // Hiện bảng chúc mừng
      const congrats = document.getElementById('congrats');
      congrats.classList.remove('hidden');
    
      // Confetti đơn giản
      confetti({
        particleCount: 150,
        spread: 80,
        origin: { y: 0.6 },
      });
    
      // Phát âm thanh
      const audio = new Audio('success.mp3');
      audio.play();
    
      // Ẩn sau 2 giây
      setTimeout(() => {
        congrats.classList.add('hidden');
      }, 2000);
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
    
    let isCreatingAirplanes = false; // Biến theo dõi việc tạo máy bay

    function createAirplane(id = "airplane", count = 1) {
      if (correctAnswers !== 33 || isCreatingAirplanes) return;
    
      isCreatingAirplanes = true; // Đánh dấu bắt đầu quá trình tạo máy bay
      
      // Tạo ngẫu nhiên số lượng máy bay (3 hoặc 4 máy bay)
      const randomCount = Math.floor(Math.random() * 2) + 3; // Số máy bay ngẫu nhiên từ 3 đến 4
    
      for (let i = 0; i < randomCount; i++) {
        const airplane = document.createElement("img");
        airplane.src = "./—Pngtree—jet fighter illustration_8476956.png";
        airplane.className = "flying-object";
        
        // Nếu count > 1, thêm số thứ tự vào id, còn lại giữ nguyên
        airplane.id = count > 1 ? `${id}-${i}` : id;
        
        // Máy bay sát nhau nếu count > 1, còn lại giữ nguyên top
        const topOffset = 20 - i * 1.2;
        airplane.style.top = `${topOffset}%`;
    
        // Vị trí ngẫu nhiên trên trục x (giới hạn trong phạm vi cửa sổ)
        const randomLeft = Math.floor(Math.random() * 100) - 50; // Tạo vị trí ngẫu nhiên từ -50% đến 50%
        airplane.style.left = `${randomLeft}%`;
        
        // Hiệu ứng bay từ trái sang phải (kéo dài qua màn hình)
        const randomAnimationDuration = Math.floor(Math.random() * 5) + 5; // Từ 5s đến 10s
        airplane.style.animation = `flyAcross ${randomAnimationDuration}s linear forwards`;
        airplane.style.animationDelay = `${i * 0.15}s`; // Delay nhỏ để các máy bay bay gần nhau
    
        document.body.appendChild(airplane);
    
        // Bắt đầu pháo hoa cho từng chiếc nếu muốn
        setTimeout(() => launchFireworks(airplane), i * 150);
    
        // Sau khi máy bay hoàn thành animation, xóa máy bay và tạo lại nhóm mới
        airplane.addEventListener("animationend", () => {
          airplane.remove(); // Xóa máy bay đã bay hết
        });
      }
    
      // Tạo nhóm mới mỗi 3 giây sau khi tất cả máy bay đã bay hết
      setTimeout(() => {
        isCreatingAirplanes = false; // Đánh dấu kết thúc quá trình tạo máy bay
        createAirplane(id, count); // Tạo lại nhóm máy bay mới
      }, 3000); // Thời gian tạo nhóm mới (sau khi máy bay đã bay hết)
    }
    
    // CSS cho hiệu ứng `flyAcross`
   
    
  

  
  
  

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
