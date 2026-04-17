<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>M-Tech Maintenance</title>
    <style>
        :root {
            --primary-color: #4A5D73;
            --bg-color: #DCE3E3;
            --white: #ffffff;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--primary-color);
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            direction: ltr;
        }

        .container {
            width: 90%;
            max-width: 500px;
            background: var(--white);
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            text-align: center;
            min-height: 400px;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .page { display: none; }
        .active { display: block; }

        h1 { font-size: 3rem; margin-bottom: 0.5rem; color: var(--primary-color); }
        h2 { margin-bottom: 1.5rem; }

        .btn {
            display: block;
            width: 100%;
            padding: 15px;
            margin: 10px 0;
            border: 2px solid var(--primary-color);
            background: transparent;
            color: var(--primary-color);
            border-radius: 8px;
            cursor: pointer;
            font-size: 1.1rem;
            font-weight: bold;
            transition: 0.3s;
        }

        .btn:hover {
            background: var(--primary-color);
            color: var(--white);
        }

        .back-btn {
            background: #ccc;
            border-color: #ccc;
            margin-top: 20px;
            color: #333;
        }

        input, select, textarea {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
        }

        .rtl { direction: rtl; }
    </style>
</head>
<body>

<div class="container" id="main-container">
    
    <div id="page1" class="page active">
        <h1>M-Tech</h1>
        <p id="welcome-text">Welcome</p>
        <button class="btn" onclick="setLanguage('en')">English</button>
        <button class="btn" onclick="setLanguage('ar')">عربي</button>
    </div>

    <div id="page2" class="page">
        <h2 id="device-title">Device Type</h2>
        <button class="btn" onclick="selectDevice('mobile')">Mobile</button>
        <button class="btn" onclick="selectDevice('computer')">Computer</button>
        <button class="btn" onclick="selectDevice('laptop')">Laptop</button>
        <button class="btn" onclick="selectDevice('playstation')">Playstation</button>
        <button class="btn back-btn" onclick="goBack(1)" id="back-p2">Back</button>
    </div>

    <div id="page3" class="page">
        <h2 id="selection-title">Select Brand</h2>
        <div id="brand-options"></div>
        <button class="btn back-btn" onclick="goBack(2)" id="back-p3">Back</button>
    </div>

    <div id="page4" class="page">
        <h2 id="service-title">Select Service</h2>
        <div id="service-options"></div>
        <button class="btn back-btn" onclick="goBack(3)" id="back-p4">Back</button>
    </div>

    <div id="page5" class="page">
        <h2 id="method-title">Repair Method</h2>
        <button class="btn" onclick="selectMethod('At Shop')">I will come to shop</button>
        <button class="btn" onclick="selectMethod('Home Service')">Come to my location</button>
        <button class="btn back-btn" onclick="goBack(4)" id="back-p5">Back</button>
    </div>

    <div id="page6" class="page">
        <h2 id="details-title">Enter Details</h2>
        <input type="text" id="cust-name" placeholder="Full Name" required>
        <input type="text" id="cust-phone" placeholder="Phone Number" required>
        <input type="text" id="cust-address" placeholder="Address" required>
        <input type="text" id="dev-model" placeholder="Device Model (e.g. iPhone 13)" required>
        <textarea id="issue-desc" placeholder="Describe the problem in detail" rows="4" required></textarea>
        <button class="btn" style="background:#25D366; color:white; border:none;" onclick="sendWhatsApp()">Submit & Open WhatsApp</button>
        <button class="btn back-btn" onclick="goBack(5)" id="back-p6">Back</button>
    </div>

</div>

<script>
    let lang = 'en';
    let orderData = {};

    const texts = {
        en: {
            welcome: "Welcome", back: "Back", deviceTitle: "Device Type", 
            selectionTitle: "Select Brand", serviceTitle: "Select Service",
            methodTitle: "How to repair?", atShop: "I come to shop", homeSrv: "Come to my house",
            detailsTitle: "Final Details", nameP: "Name", phoneP: "Phone", addrP: "Address",
            modelP: "Device Model", issueP: "Problem details",
            brands: ["Samsung", "Iphone", "Infinix", "Honor", "Realme", "Itel", "Tecno", "Xiaomi", "Huawei", "Blackview"]
        },
        ar: {
            welcome: "أهلاً بك", back: "رجوع", deviceTitle: "نوع الجهاز", 
            selectionTitle: "اختر النوع", serviceTitle: "اختر الخدمة",
            methodTitle: "طريقة الإصلاح", atShop: "سأذهب للمحل", homeSrv: "تعال إلى عنواني",
            detailsTitle: "التفاصيل الأخيرة", nameP: "الاسم", phoneP: "رقم الهاتف", addrP: "العنوان",
            modelP: "موديل الجهاز", issueP: "تفاصيل المشكلة",
            brands: ["سامسونج", "ايفون", "انفينيكس", "هونر", "ريلمي", "ايتل", "تكنو", "شاومي", "هواوي", "بلاك فيو"]
        }
    };

    function setLanguage(l) {
        lang = l;
        orderData.language = l;
        document.getElementById('main-container').className = (l === 'ar') ? 'container rtl' : 'container';
        updateText();
        showPage(2);
    }

    function updateText() {
        document.getElementById('welcome-text').innerText = texts[lang].welcome;
        document.querySelectorAll('.back-btn').forEach(b => b.innerText = texts[lang].back);
        document.getElementById('device-title').innerText = texts[lang].deviceTitle;
        document.getElementById('details-title').innerText = texts[lang].detailsTitle;
        document.getElementById('cust-name').placeholder = texts[lang].nameP;
        document.getElementById('cust-phone').placeholder = texts[lang].phoneP;
        document.getElementById('cust-address').placeholder = texts[lang].addrP;
        document.getElementById('dev-model').placeholder = texts[lang].modelP;
        document.getElementById('issue-desc').placeholder = texts[lang].issueP;
    }

    function showPage(p) {
        document.querySelectorAll('.page').forEach(page => page.classList.remove('active'));
        document.getElementById('page' + p).classList.add('active');
    }

    function goBack(p) { showPage(p); }

    function selectDevice(type) {
        orderData.device = type;
        const brandDiv = document.getElementById('brand-options');
        brandDiv.innerHTML = '';
        
        if(type === 'mobile') {
            texts[lang].brands.forEach(b => {
                let btn = document.createElement('button');
                btn.className = 'btn';
                btn.innerText = b;
                btn.onclick = () => selectBrand(b);
                brandDiv.appendChild(btn);
            });
            showPage(3);
        } else if(type === 'laptop') {
            const brands = ["Samsung", "HP", "Dell", "Toshiba", "Microsoft"];
            brands.forEach(b => {
                let btn = document.createElement('button');
                btn.className = 'btn';
                btn.innerText = b;
                btn.onclick = () => selectBrand(b);
                brandDiv.appendChild(btn);
            });
            showPage(3);
        } else if(type === 'playstation') {
            const models = ["PS2", "PS3", "PS4", "PS5"];
            models.forEach(m => {
                let btn = document.createElement('button');
                btn.className = 'btn';
                btn.innerText = m;
                btn.onclick = () => selectBrand(m);
                brandDiv.appendChild(btn);
            });
            showPage(3);
        } else {
            selectBrand('PC'); // Computer
        }
    }

    function selectBrand(brand) {
        orderData.brand = brand;
        const serviceDiv = document.getElementById('service-options');
        serviceDiv.innerHTML = '';
        let services = [];

        if(orderData.device === 'mobile') {
            if(brand.toLowerCase().includes('iphone') || brand.includes('ايفون')) {
                services = lang === 'en' ? ["Screen", "Camera", "Charging Port", "Locked iCloud", "Board Repair"] : ["شاشة", "كاميرا", "مدخل شحن", "ايكلاود", "تصليح بورد"];
            } else {
                services = lang === 'en' ? ["FRP Bypass", "Screen", "Charging Port", "Clean", "Water Damage", "Software", "Skip PIN"] : ["تخطي حساب", "شاشة", "مدخل شحن", "تنظيف", "ضرر مياه", "سوفوير", "تخطي رمز"];
            }
        } else if(orderData.device === 'laptop') {
            services = lang === 'en' ? ["Screen", "Keyboard", "Mouse", "Format", "Clean"] : ["شاشة", "كيبورد", "ماوس", "فورمات", "تنظيف"];
        } else if(orderData.device === 'computer') {
            services = lang === 'en' ? ["Overheating", "Screen", "Mouse", "Keyboard", "Fan"] : ["حرارة زائدة", "شاشة", "ماوس", "كيبورد", "مروحة"];
        } else if(orderData.device === 'playstation') {
            services = lang === 'en' ? ["Overheating", "Fan", "Repair Body"] : ["حرارة زائدة", "مروحة", "تصليح جسم الجهاز"];
        }

        services.forEach(s => {
            let btn = document.createElement('button');
            btn.className = 'btn';
            btn.innerText = s;
            btn.onclick = () => { orderData.service = s; showPage(5); };
            serviceDiv.appendChild(btn);
        });
        showPage(4);
    }

    function selectMethod(m) {
        orderData.method = m;
        showPage(6);
    }

    function sendWhatsApp() {
        const name = document.getElementById('cust-name').value;
        const phone = document.getElementById('cust-phone').value;
        const addr = document.getElementById('cust-address').value;
        const model = document.getElementById('dev-model').value;
        const issue = document.getElementById('issue-desc').value;

        if(!name || !phone || !addr || !model || !issue) {
            alert(lang === 'en' ? "Please fill all fields!" : "الرجاء تعبئة جميع الخانات!");
            return;
        }

        const message = `*M-Tech Order*%0A
*Customer:* ${name}%0A
*Phone:* ${phone}%0A
*Address:* ${addr}%0A
*Device:* ${orderData.device} (${orderData.brand})%0A
*Specific Model:* ${model}%0A
*Service:* ${orderData.service}%0A
*Method:* ${orderData.method}%0A
*Problem:* ${issue}`;

        window.open(`https://wa.me/96181855930?text=${message}`, '_blank');
    }
</script>

</body>
</html>