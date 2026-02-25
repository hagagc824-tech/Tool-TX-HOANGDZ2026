<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI PRO 2026 | HOANGDZ SYSTEM</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;900&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.1.1/crypto-js.min.js"></script>
    <style>
        :root { --neon: #00f2ff; --bg: #050505; }
        body { margin:0; background:var(--bg); color:#fff; font-family:'Orbitron', sans-serif; overflow-x:hidden; }
        .snow-container { position:fixed; inset:0; pointer-events:none; z-index:999; }
        .snowflake { position:absolute; background:white; border-radius:50%; opacity:0.8; animation: fall linear infinite; }
        @keyframes fall { to { transform: translateY(100vh); } }

        .wrapper { max-width:450px; margin:20px auto; padding:15px; position:relative; z-index:1000; }
        .glass { background:rgba(0,0,0,0.85); border:1px solid var(--neon); border-radius:20px; padding:20px; margin-bottom:20px; text-align:center; }
        input, button { width:100%; padding:14px; margin-bottom:12px; border-radius:10px; border:1px solid #444; background:#111; color:#fff; font-size:16px; box-sizing:border-box; }
        .btn { background:linear-gradient(90deg, var(--neon), #7000ff); font-weight:bold; cursor:pointer; }
        .res-box { font-size:2.8rem; font-weight:900; color:var(--neon); margin:15px 0; }
        .timer-box { font-size:14px; color:yellow; margin-bottom:10px; }
        .login-overlay { position:fixed; inset:0; background:#000; z-index:9999; display:flex; align-items:center; justify-content:center; padding:20px; }
    </style>
</head>
<body>
    <div class="snow-container" id="snow"></div>

    <div class="login-overlay" id="loginScreen">
        <div class="glass" style="width:100%; max-width:320px;">
            <h2 style="color:var(--neon);">🔑 KÍCH HOẠT VIP</h2>
            <input type="text" id="keyInput" placeholder="Nhập Key...">
            <button class="btn" onclick="checkKey()">KÍCH HOẠT NGAY</button>
            <div id="checkStatus" style="font-size:12px; color:red; margin-top:10px;"></div>
        </div>
    </div>

    <div class="wrapper" id="mainScreen" style="display:none;">
        <div class="glass">
            <h3 style="color:var(--neon); margin:0;">GIỚI THIỆU TOOL</h3>
            <p style="font-size:12px;">Hệ thống phân tích MD5 2026. Bản quyền HOANGDZ. Độ ổn định cao.</p>
            <div class="timer-box" id="timerDisplay">Thời gian còn lại: --:--</div>
            <button class="btn" style="background:#ff3b3b; font-size:12px;" onclick="logout()">ĐĂNG XUẤT</button>
        </div>

        <div class="glass">
            <input type="text" id="code" placeholder="Mã phiên MD5...">
            <button class="btn" id="analyzeBtn" onclick="runAnalysis()">PHÂN TÍCH KẾT QUẢ</button>
            <div id="res" class="res-box">--</div>
        </div>
    </div>

    <script>
        // Tạo tuyết
        function createSnow() {
            const snow = document.getElementById('snow');
            for(let i=0; i<60; i++) {
                let d = document.createElement('div');
                d.className = 'snowflake';
                d.style.width = d.style.height = Math.random()*5 + 'px';
                d.style.left = Math.random()*100 + '%';
                d.style.animationDuration = Math.random()*5 + 5 + 's';
                snow.appendChild(d);
            }
        }
        createSnow();

        function checkKey() {
            let val = document.getElementById('keyInput').value.trim();
            if (val.startsWith("KEY-HOANGDZ-")) {
                localStorage.setItem('expiry', Date.now() + 3600000); // Key 1 giờ
                location.reload();
            } else {
                document.getElementById('checkStatus').innerText = "❌ Sai Key!";
            }
        }

        function runAnalysis() {
            let btn = document.getElementById('analyzeBtn');
            let input = document.getElementById('code').value.trim();
            if(input.length < 32) return alert("Nhập đủ 32 ký tự!");

            // Cơ chế ổn định: Khóa nút 30 giây (bỏ 2-3 tay)
            btn.disabled = true;
            btn.innerText = "ĐANG ỔN ĐỊNH HỆ THỐNG (30s)...";
            
            setTimeout(() => {
                let hash = CryptoJS.MD5(input).toString();
                document.getElementById('res').innerText = (parseInt(hash.slice(-1), 16) % 2 === 0) ? "🔴 TÀI" : "⚪ XỈU";
                btn.disabled = false;
                btn.innerText = "PHÂN TÍCH KẾT QUẢ";
            }, 30000);
        }

        function updateTimer() {
            let expiry = localStorage.getItem('expiry');
            if (expiry) {
                let left = expiry - Date.now();
                if (left <= 0) {
                    localStorage.removeItem('expiry');
                    location.reload();
                } else {
                    let min = Math.floor(left/60000);
                    let sec = Math.floor((left%60000)/1000);
                    document.getElementById('timerDisplay').innerText = `Thời gian còn lại: ${min}p ${sec}s`;
                }
            }
        }

        function logout() { localStorage.removeItem('expiry'); location.reload(); }

        window.onload = () => {
            if (localStorage.getItem('expiry') > Date.now()) {
                document.getElementById('loginScreen').style.display = 'none';
                document.getElementById('mainScreen').style.display = 'block';
                setInterval(updateTimer, 1000);
            }
        };
    </script>
</body>
</html>
