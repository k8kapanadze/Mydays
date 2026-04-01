<!DOCTYPE html>
<html lang="ka">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Carpe Diem - მართვის პანელი</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --glass: rgba(255, 255, 255, 0.9);
            --glass-border: rgba(255, 255, 255, 0.5);
            --accent: #3b82f6;
            --success: #10b981;
            --danger: #ef4444;
            --bg-gradient: linear-gradient(135deg, #f1f5f9 0%, #e2e8f0 100%);
            --shadow: 0 10px 30px rgba(0, 0, 0, 0.04);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Inter', sans-serif; }
        
        body { 
            background: var(--bg-gradient);
            background-attachment: fixed;
            min-height: 100vh;
            padding: 15px 0;
            color: #1e293b;
        }

        .container { width: 100%; max-width: 600px; margin: 0 auto; padding: 0 10px; }

        .glass-card {
            background: var(--glass);
            backdrop-filter: blur(15px);
            border: 1px solid var(--glass-border);
            border-radius: 24px;
            box-shadow: var(--shadow);
            padding: 20px;
            margin-bottom: 20px;
        }

        .section-title {
            font-size: 0.7rem;
            font-weight: 800;
            color: #94a3b8;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            margin-bottom: 15px;
            display: block;
        }

        .input-group { display: flex; flex-direction: column; gap: 5px; margin-bottom: 15px; }
        label { font-size: 0.8rem; font-weight: 700; color: #64748b; margin-left: 5px; }

        input, select {
            background: white;
            border: 1px solid #e2e8f0;
            border-radius: 12px;
            padding: 12px;
            font-size: 1rem;
            outline: none;
            color: #334155;
            width: 100%;
            transition: 0.2s;
        }
        input:focus { border-color: var(--accent); }

        .control-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }

        .btn {
            border-radius: 14px;
            border: none;
            padding: 14px;
            font-weight: 700;
            cursor: pointer;
            transition: 0.2s;
            font-size: 0.9rem;
            width: 100%;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        .btn:active { transform: scale(0.98); }
        
        .btn-primary { background: #0f172a; color: white; }
        .btn-secondary { background: white; color: #1e293b; border: 1px solid #e2e8f0; }
        .btn-danger { background: #fee2e2; color: var(--danger); }

        /* Table Styles */
        .table-card { padding: 0; overflow: hidden; }
        table { width: 100%; border-collapse: collapse; table-layout: fixed; }
        th { background: #1e293b; color: white; padding: 14px 5px; font-size: 0.7rem; text-transform: uppercase; }
        td { padding: 12px 2px; text-align: center; border-bottom: 1px solid rgba(0,0,0,0.03); }

        .day-cell { font-weight: 800; font-size: 1rem; color: #475569; }
        .day-cell.is-auto { color: var(--danger); } /* ავტომატურად შევსებული რიცხვები */

        .plan-select {
            width: 85%; padding: 8px; border-radius: 10px; border: 1px solid #e2e8f0;
            font-weight: 800; text-align: center; font-size: 0.9rem;
        }

        .real-display {
            background: rgba(59, 130, 246, 0.08);
            color: var(--accent);
            font-weight: 800;
            border-radius: 10px;
            padding: 8px 0;
            width: 85%;
            display: inline-block;
        }

        .journal-btn {
            background: #f8fafc;
            color: #94a3b8;
            font-size: 0.75rem;
            font-weight: 700;
            padding: 8px 0;
            border-radius: 10px;
            border: 1px solid #e2e8f0;
            cursor: pointer;
            width: 85%;
            display: inline-block;
        }
        .journal-btn.has-note {
            background: rgba(239, 68, 68, 0.1);
            color: var(--danger);
            border-color: rgba(239, 68, 68, 0.2);
        }

        .totals-footer {
            display: flex; justify-content: space-around; padding: 20px;
            background: #f8fafc; border-top: 1px solid #e2e8f0;
        }
        .total-item { text-align: center; }
        .total-label { font-size: 0.65rem; color: #94a3b8; font-weight: 800; text-transform: uppercase; }
        .total-val { display: block; font-size: 1.3rem; font-weight: 900; color: #0f172a; }

        /* Modal */
        .modal { display: none; position: fixed; inset: 0; background: rgba(15, 23, 42, 0.6); backdrop-filter: blur(8px); z-index: 1000; padding: 20px; }
        .modal-content { background: white; max-width: 450px; margin: 50px auto; padding: 25px; border-radius: 28px; }
        textarea { width: 100%; border-radius: 16px; padding: 15px; background: #f8fafc; border: 2px solid #e2e8f0; margin: 15px 0; font-size: 1rem; outline: none; transition: 0.3s; resize: none; }
        textarea:focus { border-color: var(--accent); background: white; }

        /* Archive Section (Hidden by default) */
        #archiveSection { display: none; margin-top: 30px; background: white; padding: 30px; border-radius: 0; }
        .archive-item { margin-bottom: 20px; padding-bottom: 15px; border-bottom: 1px dashed #cbd5e1; }
        .archive-date { font-weight: 800; color: #0f172a; margin-bottom: 5px; display: block; }
        .archive-text { color: #475569; line-height: 1.5; font-style: italic; }

        @media print {
            .no-print { display: none !important; }
            body { background: white; padding: 0; }
            .glass-card { box-shadow: none; border: none; width: 100%; }
            #archiveSection { display: block !important; }
            .container { max-width: 100%; }
        }
    </style>
</head>
<body>

<div class="container">
    <div class="glass-card no-print">
        <span class="section-title">პარამეტრები</span>
        <div class="input-group">
            <label>ექთნის სახელი</label>
            <input type="text" id="nurseName" placeholder="სახელი და გვარი..." oninput="save()">
        </div>

        <div class="control-grid">
            <div class="input-group">
                <label>თვე</label>
                <select id="mSelect" onchange="render()"></select>
            </div>
            <div class="input-group">
                <label>წელი</label>
                <select id="ySelect" onchange="render()"></select>
            </div>
        </div>

        <span class="section-title" style="margin-top: 10px;">საათების ფიქსაცია</span>
        <div class="control-grid">
            <div class="input-group">
                <label>რიცხვი</label>
                <input type="number" id="realDay" placeholder="1-31">
            </div>
            <div class="input-group">
                <label>საათი</label>
                <select id="realHours">
                    <option value="0">0</option>
                    <option value="8">8 სთ</option>
                    <option value="16">16 სთ</option>
                    <option value="24">24 სთ</option>
                </select>
            </div>
        </div>

        <button class="btn btn-primary" onclick="saveReal()">დაფიქსირება</button>
        
        <div class="control-grid">
            <button class="btn btn-secondary" onclick="fillFuture()">წლის შევსება</button>
            <button class="btn btn-danger" style="background:#fff1f2" onclick="clearFromNow()">გეგმის წაშლა</button>
        </div>
    </div>

    <div class="glass-card table-card">
        <table>
            <thead>
                <tr>
                    <th style="width: 15%;">#</th>
                    <th style="width: 25%;">გეგმა</th>
                    <th style="width: 25%;">რეალ.</th>
                    <th style="width: 35%;">დღიური</th>
                </tr>
            </thead>
            <tbody id="bTable"></tbody>
        </table>
        
        <div class="totals-footer">
            <div class="total-item">
                <span class="total-label">გეგმა</span>
                <span id="totalPlan" class="total-val">0</span>
            </div>
            <div class="total-item">
                <span class="total-label">ნამუშევარი</span>
                <span id="totalReal" class="total-val" style="color: var(--accent);">0</span>
            </div>
        </div>
        
        <button class="btn btn-secondary no-print" style="border-radius:0; border:none; margin:0; border-top:1px solid #e2e8f0" onclick="prepareAndPrint()">
            PDF არქივი / ბეჭდვა
        </button>
    </div>

    <div id="archiveSection">
        <h2 id="printHeader" style="margin-bottom: 20px; border-bottom: 2px solid #0f172a; padding-bottom: 10px;"></h2>
        <div id="archiveContent"></div>
    </div>
</div>

<div id="jModal" class="modal">
    <div class="modal-content">
        <h3 id="jTitle" style="font-weight: 800; color: #0f172a;"></h3>
        <p style="font-size: 0.85rem; color: #64748b; margin-top: 10px;">რა ისწავლეთ? / როგორ გაერთეთ?</p>
        <textarea id="jText" rows="6" placeholder="აღწერეთ დღე..."></textarea>
        <div style="display: flex; gap: 10px;">
            <button class="btn btn-secondary" style="flex:1; margin-bottom:0;" onclick="closeM()">გაუქმება</button>
            <button class="btn btn-primary" style="flex:1; margin-bottom:0;" onclick="saveJ()">შენახვა</button>
        </div>
    </div>
</div>

<script>
    let state = JSON.parse(localStorage.getItem('nurse_pro_v14')) || { name: "", shifts: {} };
    const months = ["იანვარი", "თებერვალი", "მარტი", "აპრილი", "მაისი", "ივნისი", "ივლისი", "აგვისტო", "სექტემბერი", "ოქტომბერი", "ნოემბერი", "დეკემბერი"];
    let activeKey = null;

    function init() {
        const ms = document.getElementById('mSelect');
        const ys = document.getElementById('ySelect');
        months.forEach((m, i) => ms.add(new Option(m, i)));
        for(let y=2025; y<=2030; y++) ys.add(new Option(y, y));
        
        ms.value = new Date().getMonth();
        ys.value = new Date().getFullYear();
        document.getElementById('nurseName').value = state.name || "";
        render();
    }

    function render() {
        const m = parseInt(document.getElementById('mSelect').value);
        const y = parseInt(document.getElementById('ySelect').value);
        const daysInMonth = new Date(y, m + 1, 0).getDate();
        
        let bHTML = "";
        let tPlan = 0, tReal = 0;

        for(let d=1; d<=daysInMonth; d++) {
            const k = `d_${y}_${m}_${d}`;
            const s = state.shifts[k] || {};
            const pVal = s.plan || "";
            const rVal = s.real || "—";
            const isAuto = s.isAuto ? "is-auto" : "";
            const btnLabel = s.note ? "ნახვა" : "ჩაწერა";
            const btnClass = s.note ? "has-note" : "";

            tPlan += parseInt(pVal) || 0;
            tReal += parseInt(rVal) || 0;

            bHTML += `
                <tr>
                    <td class="day-cell ${isAuto}">${d}</td>
                    <td>
                        <select class="plan-select" onchange="upd('${k}','plan',this.value, false)">
                            <option value=""></option>
                            <option value="8" ${pVal=='8'?'selected':''}>8</option>
                            <option value="16" ${pVal=='16'?'selected':''}>16</option>
                            <option value="24" ${pVal=='24'?'selected':''}>24</option>
                        </select>
                    </td>
                    <td><div class="real-display">${rVal}</div></td>
                    <td>
                        <div class="journal-btn ${btnClass}" onclick="openM('${k}', ${d})">
                            ${btnLabel}
                        </div>
                    </td>
                </tr>`;
        }
        document.getElementById('bTable').innerHTML = bHTML;
        document.getElementById('totalPlan').innerText = tPlan;
        document.getElementById('totalReal').innerText = tReal;
    }

    function saveReal() {
        const d = document.getElementById('realDay').value;
        const h = document.getElementById('realHours').value;
        const m = document.getElementById('mSelect').value;
        const y = document.getElementById('ySelect').value;
        if(!d || d < 1 || d > 31) return alert("მიუთითეთ სწორი რიცხვი");
        const k = `d_${y}_${m}_${d}`;
        if(!state.shifts[k]) state.shifts[k] = {};
        state.shifts[k].real = h;
        save(); render();
    }

    function fillFuture() {
        const m = parseInt(document.getElementById('mSelect').value);
        const y = parseInt(document.getElementById('ySelect').value);
        let startD = null, hrs = 24;

        // ვეძებთ პირველ მონიშნულ გეგმას ამ თვეში
        for(let d=1; d<=31; d++) {
            if(state.shifts[`d_${y}_${m}_${d}`]?.plan) {
                startD = new Date(y, m, d);
                hrs = state.shifts[`d_${y}_${m}_${d}`].plan;
                break;
            }
        }

        if(startD) {
            let c = new Date(startD);
            // ვასუფთავებთ "ავტო" ფლაგს ყველგან ამ წლისთვის რომ თავიდან გადავანაწილოთ
            Object.keys(state.shifts).forEach(key => {
                if(key.startsWith(`d_${y}`)) state.shifts[key].isAuto = false;
            });

            while(c.getFullYear() == y) {
                const k = `d_${c.getFullYear()}_${c.getMonth()}_${c.getDate()}`;
                if(!state.shifts[k]) state.shifts[k] = {};
                state.shifts[k].plan = hrs;
                state.shifts[k].isAuto = true; // მოინიშნოს როგორც ავტომატური
                c.setDate(c.getDate() + 4);
            }
            save(); render();
        } else {
            alert("ჯერ მონიშნეთ ერთ-ერთი დღის გეგმა ცხრილში!");
        }
    }

    function clearFromNow() {
        const m = parseInt(document.getElementById('mSelect').value);
        const y = document.getElementById('ySelect').value;
        if(confirm(`${months[m]} თვიდან წლის ბოლომდე გეგმა წაიშლება?`)) {
            for(let currM = m; currM <= 11; currM++) {
                for(let d = 1; d <= 31; d++) {
                    const k = `d_${y}_${currM}_${d}`;
                    if(state.shifts[k]) {
                        state.shifts[k].plan = "";
                        state.shifts[k].isAuto = false;
                    }
                }
            }
            save(); render();
        }
    }

    function prepareAndPrint() {
        const m = document.getElementById('mSelect').value;
        const y = document.getElementById('ySelect').value;
        const name = state.name || "პერსონალური არქივი";
        
        document.getElementById('printHeader').innerText = `${name} - ${months[m]}, ${y}`;
        
        let archiveHTML = "";
        let hasNotes = false;

        // ვაგროვებთ მხოლოდ იმ დღეებს რომლებსაც ჩანაწერი აქვთ
        for(let d=1; d<=31; d++) {
            const k = `d_${y}_${m}_${d}`;
            if(state.shifts[k]?.note) {
                hasNotes = true;
                archiveHTML += `
                    <div class="archive-item">
                        <span class="archive-date">${d} ${months[m]}, ${y}</span>
                        <div class="archive-text">${state.shifts[k].note.replace(/\n/g, '<br>')}</div>
                    </div>`;
            }
        }

        document.getElementById('archiveContent').innerHTML = hasNotes ? archiveHTML : "<p>ამ თვეში ჩანაწერები არ იძებნება.</p>";
        window.print();
    }

    function upd(k, f, v, auto) { 
        if(!state.shifts[k]) state.shifts[k] = {}; 
        state.shifts[k][f] = v; 
        state.shifts[k].isAuto = auto;
        save(); render(); 
    }
    
    function openM(k, d) { 
        activeKey = k; 
        const m = document.getElementById('mSelect').value;
        const y = document.getElementById('ySelect').value;
        document.getElementById('jTitle').innerText = `${d} ${months[m]}, ${y}`; 
        document.getElementById('jText').value = state.shifts[k]?.note || ""; 
        document.getElementById('jModal').style.display='block'; 
    }

    function saveJ() { 
        if(!state.shifts[activeKey]) state.shifts[activeKey] = {}; 
        state.shifts[activeKey].note = document.getElementById('jText').value; 
        save(); render(); closeM(); 
    }

    function closeM() { document.getElementById('jModal').style.display='none'; }
    function save() { state.name = document.getElementById('nurseName').value; localStorage.setItem('nurse_pro_v14', JSON.stringify(state)); }

    init();
</script>
</body>
</html>
