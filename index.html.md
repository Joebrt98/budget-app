#   
<!DOCTYPE html>  
<html lang="it">  
<head>  
  <meta charset="UTF-8" />  
  <meta name="viewport" content="width=device-width, initial-scale=1" />  
  <title>Budget App</title>  
  <link rel="apple-touch-icon" href="apple-touch-icon.png" />  
  <meta name="apple-mobile-web-app-capable" content="yes" />  
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />  
  <meta name="apple-mobile-web-app-title" content="BudgetApp" />  
  <style>  
    body {  
      margin: 0; padding: 0; overflow-x: hidden; max-width: 100vw;  
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,  
        Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;  
      background: #f7f3ee; color: #000;  
      -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale;  
    }  
    .top-bar { position: fixed; top: 0; left: 0; right: 0;  
      background: #0058db; border-radius: 0 0 12px 12px;  
      box-shadow: 0 2px 6px rgba(0,0,0,0.15); z-index: 50;  
      padding: 6px 10px 14px 10px; text-align: center;  
    }  
    #menu-toggle { font-size: 28px; padding: 6px 12px 8px 12px; color: #fff;  
      background: transparent; border: none; border-radius: 8px; cursor: pointer;  
      user-select: none; margin: 0 auto 6px; display: block;  
      transition: background-color 0.25s ease; position: relative; z-index: 60;  
    }  
    .tab-bar {  
      display: flex; justify-content: center; gap: 12px; padding: 4px 0;  
      user-select: none; position: relative; z-index: 55; flex-wrap: nowrap;  
      overflow-x: visible;  
    }  
    .tab {  
      color: #fff; font-weight: 600; font-size: 16px; border-radius: 20px; cursor: pointer;  
      padding: 9px 14px; min-width: 70px; text-align: center; transition: background-color 0.3s ease;  
      flex: 0 0 auto; white-space: nowrap;  
    }  
    .tab.active { background: #00cc55; font-weight: 700; }  
    .side-menu { position: fixed; top: 0; left: -230px; width: 230px; height: 100vh; background: #fff;  
      box-shadow: 2px 0 13px rgba(0,0,0,0.2); transition: left 0.3s ease; z-index: 10000;  
      padding: 72px 16px 24px 16px; font-size: 18px; font-weight: 500; box-sizing: border-box;  
      overflow-y: auto; -webkit-overflow-scrolling: touch;  
    }  
    .side-menu.open { left: 0; }  
    .side-menu ul {  
      list-style: none; margin: 0; padding: 0; max-height: 80vh; overflow-y: auto;  
    }  
    .side-menu li { margin-bottom: 16px; cursor: pointer; border-radius: 8px; padding: 8px 12px;  
      transition: background-color 0.25s, color 0.25s; font-weight: 500; color: #333;  
    }  
    .side-menu li:hover { background-color: #e0f2e4; color: #007a2b; }  
    .side-menu li.selected { background-color: #b2e6b9; font-weight: 700; color: #00561a; }  
    .content { padding: 130px 22px 40px; max-width: 420px;  
      margin: 0 auto; position: relative; width: 100%; box-sizing: border-box;}  
    h2.section-title { font-size: 22px; font-weight: 700; margin: 28px 0 18px 0; text-align: center; color: #000; }  
    .field-list { display: flex; flex-direction: column; gap: 14px; margin-bottom: 10px; }  
    .field-row { display: flex; gap: 12px; width: 100%; box-sizing: border-box; }  
    .field-row input[type="text"] { width: 55%; flex: none; padding: 12px 14px; font-size: 18px;  
      border: 2px solid #b3dff0; border-radius: 12px; outline-offset: 2px; transition: border-color 0.3s;  
      background: #fefefe; box-sizing: border-box;  
    }  
    .field-row input[type="number"] { width: 43%; flex: none; padding: 12px 14px; font-size: 18px;  
      border: 2px solid #b3dff0; border-radius: 12px; outline-offset: 2px; transition: border-color 0.3s;  
      background: #fefefe; box-sizing: border-box; text-align: right;  
    }  
    .field-row input[type="text"]:focus, .field-row input[type="number"]:focus { border-color: #00cc55; }  
    .add-btn {  
      background: #00cc55; color: white; border: none; padding: 10px 20px; border-radius: 30px; font-weight: 700;  
      cursor: pointer; align-self: flex-start; margin-bottom: 36px; user-select: none;  
      box-shadow: 0 3px 10px rgba(0, 204, 85, 0.5);  
      transition: background-color 0.3s, box-shadow 0.3s;  
    }  
    .add-btn:hover { background: #009944; box-shadow: 0 5px 15px rgba(0, 153, 68, 0.7);}  
    .expenses-col { box-sizing: border-box; background: #ffffffcc; border-radius: 14px; padding: 18px 16px;  
      box-shadow: 2px 0 12px rgba(0,0,0,0.05); display: flex; flex-direction: column;  
      max-width: 420px; margin: 0 auto 36px auto;  
    }  
    .totals-section { margin-top: 12px; padding-top: 16px; border-top: 1.5px solid #ccc; font-weight: 700;  
      font-size: 17px; text-align: right; user-select: none; color: #00561a;  
    }  
    .totals-section.total-spese { margin-top: 6px; font-size: 19px; font-weight: 800; color: #007a2b; }  
    .saldo-section {display: flex; justify-content: space-between; gap: 14px; margin-top: 20px;  
      width: 100%; box-sizing: border-box;  
    }  
    .saldo-box {  
      flex: 1; background: rgba(0,204,85,0.18); border-radius: 18px; padding: 22px 24px;  
      font-weight: 700; font-size: 20px; text-align: center; user-select: none; box-sizing: border-box;  
      box-shadow: 0 0 10px rgba(0,0,0,0.05); transition: background-color 0.3s;  
    }  
    .saldo-box:hover { background-color: rgba(0,204,85,0.23);}  
    .header-space { height: 130px; }  
    .dropdown-container { max-width: 420px; margin: 0 auto 16px auto; display: flex; justify-content: center; gap: 15px;}  
    select {  
      padding: 10px 16px; font-size: 18px; border-radius: 14px; border: 2px solid #b3dff0;  
      outline-offset: 2px; cursor: pointer; background: white;  
    }  
    @media (max-width: 430px) {  
      .tab-bar { padding: 4px 0;}  
      .tab { font-size: 16px; padding: 10px 10px; margin-left: 10px; margin-right: 10px;}  
      .field-row input[type="text"] { font-size: 16px; padding: 10px 12px;}  
      .field-row input[type="number"] { font-size: 16px; padding: 10px 12px;}  
      .add-btn { padding: 10px 20px; font-weight: 700;}  
      .saldo-box { font-size: 18px; padding: 20px 16px;}  
      .content { max-width: 420px; padding-left: 18px; padding-right: 18px;}  
      .totals-section { font-size: 15px;}  
      .dropdown-container { gap: 10px;}  
    }  
  </style>  
</head>  
<body>  
  <div class="top-bar">  
    <button id="menu-toggle" aria-label="Apri menu">&#9776;</button>  
    <div class="tab-bar" role="tablist">  
      <div class="tab active" role="tab" tabindex="0" aria-selected="true" id="tab-entrate">Entrate</div>  
      <div class="tab" role="tab" tabindex="-1" aria-selected="false" id="tab-spese">Spese</div>  
      <div class="tab" role="tab" tabindex="-1" aria-selected="false" id="tab-saldo">Saldo</div>  
    </div>  
  </div>  
  
  <nav id="side-menu" class="side-menu" role="navigation" aria-label="Seleziona mese">  
    <ul id="months-list" tabindex="0"></ul>  
  </nav>  
  
  <div class="header-space"></div>  
  
  <main class="content">  
    <section id="content-entrate">  
      <h2 class="section-title">Entrate</h2>  
      <div class="field-list" id="entrate-list">  
        <div class="field-row">  
          <input type="text" placeholder="Etichetta" class="entrate-label" />  
          <input type="number" placeholder="Importo" class="entrate-amount" />  
        </div>  
        <div class="field-row">  
          <input type="text" placeholder="Etichetta" class="entrate-label" />  
          <input type="number" placeholder="Importo" class="entrate-amount" />  
        </div>  
        <div class="field-row">  
          <input type="text" placeholder="Etichetta" class="entrate-label" />  
          <input type="number" placeholder="Importo" class="entrate-amount" />  
        </div>  
      </div>  
      <button class="add-btn" id="add-entrata">+ Aggiungi entrata</button>  
      <div class="totals-row">  
        <label for="residuo-mese">Residuo mese precedente</label>  
        <input type="number" id="residuo-mese" />  
      </div>  
      <div class="totals-row">  
        <label>Totale Entrate</label>  
        <input type="number" id="totale-entrate" value="0" readonly />  
      </div>  
    </section>  
  
    <section id="content-spese" style="display:none">  
      <h2 class="section-title">Spese</h2>  
      <div class="dropdown-container">  
        <label for="tipo-spesa">Tipo spesa:</label>  
        <select id="tipo-spesa" aria-label="Seleziona tipo di spesa">  
          <option value="fisse">Spese Fisse</option>  
          <option value="variabili">Spese Variabili</option>  
        </select>  
      </div>  
      <div class="expenses-col">  
        <div class="field-list" id="spese-list"></div>  
        <div class="totals-section" id="tot-spese-categoria">Totale spese categoria: € 0.00</div>  
        <div class="totals-section total-spese" id="tot-spese-totale">Totale spese complessivo: € 0.00</div>  
        <button class="add-btn" id="add-spesa" aria-label="Aggiungi spesa">+ Aggiungi spesa</button>  
      </div>  
    </section>  
  
    <section id="content-saldo" style="display:none">  
      <div class="saldo-section" role="region" aria-label="Sezione saldo">  
        <div class="saldo-box" id="saldo-effettivo" aria-live="polite">  
          Saldo Effettivo<br><span id="saldo-effettivo-val">€ 0.00</span>  
        </div>  
        <div class="saldo-box" id="budget-mensile" aria-live="polite">  
          Budget Mensile<br><span id="budget-mensile-val">€ 0.00</span>  
        </div>  
      </div>  
    </section>  
  </main>  
  <script>  
    const tabs = document.querySelectorAll('.tab');  
    const sections = {  
      entrate: document.getElementById('content-entrate'),  
      spese: document.getElementById('content-spese'),  
      saldo: document.getElementById('content-saldo'),  
    };  
    const months = ["Gennaio","Febbraio","Marzo","Aprile","Maggio","Giugno","Luglio","Agosto","Settembre","Ottobre","Novembre","Dicembre"];  
    let currentMonth = new Date().getMonth();  
    let currentSpesaType = 'fisse';  
  
    // Per propagazione spese fisse  
    function getLatestFixedExpenses(month) {  
      for(let i=month-1; i>=0; i--) {  
        const data = JSON.parse(localStorage.getItem(getStorageKey(i)));  
        if(data && data.speseFisse && data.speseFisse.length > 0)  
          return JSON.parse(JSON.stringify(data.speseFisse));  
      }  
      return [];  
    }  
  
    function renderMonths() {  
      const monthsList = document.getElementById('months-list');  
      monthsList.innerHTML = '';  
      months.forEach((month, index) => {  
        const li = document.createElement('li');  
        li.textContent = month;  
        if (index === currentMonth) li.classList.add('selected');  
        li.tabIndex = 0;  
        li.addEventListener('click', () => {  
          saveCurrentMonthData();  
          currentMonth = index;  
          propagateFisseIfEmpty(); // propagazione spese fisse  
          loadMonthData(currentMonth);  
          updateMonthlyResidual();  
          setSelectedMonth(currentMonth);  
          sideMenu.classList.remove('open');  
        });  
        monthsList.appendChild(li);  
      });  
    }  
    function setSelectedMonth(index) {  
      const monthsList = document.getElementById('months-list');  
      [...monthsList.children].forEach((li, i) => {  
        li.classList.toggle('selected', i === index);  
      });  
    }  
    const menuToggle = document.getElementById('menu-toggle');  
    const sideMenu = document.getElementById('side-menu');  
    menuToggle.addEventListener('click', () => {  
      sideMenu.classList.toggle('open');  
    });  
    document.addEventListener('click', (e) => {  
      if (!sideMenu.contains(e.target) && e.target !== menuToggle) {  
        sideMenu.classList.remove('open');  
      }  
    });  
  
    tabs.forEach(tab => {  
      tab.addEventListener('click', () => {  
        tabs.forEach(t => {  
          t.classList.remove('active');  
          t.setAttribute('aria-selected', 'false');  
          t.tabIndex = -1;  
        });  
        tab.classList.add('active');  
        tab.setAttribute('aria-selected', 'true');  
        tab.tabIndex = 0;  
        if (tab.id === 'tab-entrate') {  
          sections.entrate.style.display = 'block';  
          sections.spese.style.display = 'none';  
          sections.saldo.style.display = 'none';  
        } else if (tab.id === 'tab-spese') {  
          sections.entrate.style.display = 'none';  
          sections.spese.style.display = 'block';  
          sections.saldo.style.display = 'none';  
        } else if (tab.id === 'tab-saldo') {  
          sections.entrate.style.display = 'none';  
          sections.spese.style.display = 'none';  
          sections.saldo.style.display = 'flex';  
        }  
      });  
    });  
  
    function getStorageKey(month) {  
      return `budgetAppData_month_${month}`;  
    }  
  
    function saveCurrentMonthData() {  
      const monthKey = getStorageKey(currentMonth);  
      let data = JSON.parse(localStorage.getItem(monthKey)) || { residuo: 0, entrate: [], speseFisse: [], speseVariabili: [] };  
      data.residuo = parseFloat(document.getElementById('residuo-mese').value) || 0;  
      data.entrate = [];  
      document.querySelectorAll('#entrate-list .field-row').forEach(row => {  
        const label = row.querySelector('.entrate-label').value.trim();  
        const amount = parseFloat(row.querySelector('.entrate-amount').value);  
        if (label !== '' && !isNaN(amount)) data.entrate.push({ label, amount });  
      });  
      const speseCorrenti = [];  
      document.querySelectorAll('#spese-list .field-row').forEach(row => {  
        const label = row.querySelector('.spesa-label').value.trim();  
        const amount = parseFloat(row.querySelector('.spesa-amount').value);  
        if (label !== '' && !isNaN(amount)) speseCorrenti.push({ label, amount });  
      });  
      if (currentSpesaType === 'fisse') {  
        data.speseFisse = speseCorrenti;  
      } else {  
        data.speseVariabili = speseCorrenti;  
      }  
      localStorage.setItem(monthKey, JSON.stringify(data));  
    }  
  
    function propagateFisseIfEmpty() {  
      const monthKey = getStorageKey(currentMonth);  
      const data = JSON.parse(localStorage.getItem(monthKey)) || { residuo: 0, entrate: [], speseFisse: [], speseVariabili: [] };  
      if ((!data.speseFisse || data.speseFisse.length === 0) && currentMonth > 0) {  
        const latestFisse = getLatestFixedExpenses(currentMonth);  
        if (latestFisse.length > 0) {  
          data.speseFisse = JSON.parse(JSON.stringify(latestFisse));  
          localStorage.setItem(monthKey, JSON.stringify(data));  
        }  
      }  
    }  
  
    function loadMonthData(month) {  
      clearAllFields();  
      const monthKey = getStorageKey(month);  
      const data = JSON.parse(localStorage.getItem(monthKey)) || { residuo: 0, entrate: [], speseFisse: [], speseVariabili: [] };  
      document.getElementById('residuo-mese').value = data.residuo || 0;  
      const entrateList = document.getElementById('entrate-list');  
      data.entrate.forEach((entry, i) => {  
        if (i >= entrateList.children.length) addEntrataField();  
        entrateList.children[i].querySelector('.entrate-label').value = entry.label;  
        entrateList.children[i].querySelector('.entrate-amount').value = entry.amount;  
      });  
      const speseMostrate = (currentSpesaType === 'fisse') ? data.speseFisse : data.speseVariabili;  
      populateSpeseList(speseMostrate);  
      updateAllTotals();  
      updateSaldo();  
    }  
    function clearAllFields() {  
      ['entrate-list', 'spese-list'].forEach(id => {  
        const container = document.getElementById(id);  
        container.innerHTML = '';  
      });  
      for (let i = 0; i < 3; i++) addEntrataField();  
      for (let i = 0; i < 2; i++) addSpesaField();  
    }  
    function addEntrataField() {  
      const container = document.getElementById('entrate-list');  
      const newRow = document.createElement('div');  
      newRow.className = 'field-row';  
      newRow.innerHTML = `<input type="text" placeholder="Etichetta" class="entrate-label" />  
                          <input type="number" placeholder="Importo" class="entrate-amount" />`;  
      container.appendChild(newRow);  
      addInputListeners(newRow);  
    }  
    function addSpesaField() {  
      const container = document.getElementById('spese-list');  
      const newRow = document.createElement('div');  
      newRow.className = 'field-row';  
      newRow.innerHTML = `<input type="text" placeholder="Etichetta" class="spesa-label" />  
                          <input type="number" placeholder="Importo" class="spesa-amount" />`;  
      container.appendChild(newRow);  
      addInputListeners(newRow);  
    }  
    function addInputListeners(container) {  
      container.querySelectorAll('input').forEach(input => {  
        input.addEventListener('input', () => {  
          saveCurrentMonthData();  
          updateAllTotals();  
          updateSaldo();  
        });  
      });  
    }  
    function populateSpeseList(spese) {  
      const container = document.getElementById('spese-list');  
      container.innerHTML = '';  
      spese.forEach(entry => {  
        const newRow = document.createElement('div');  
        newRow.className = 'field-row';  
        newRow.innerHTML = `<input type="text" placeholder="Etichetta" class="spesa-label" />  
                            <input type="number" placeholder="Importo" class="spesa-amount" />`;  
        container.appendChild(newRow);  
        const inputs = newRow.querySelectorAll('input');  
        inputs[0].value = entry.label || '';  
        inputs[1].value = entry.amount || '';  
        addInputListeners(newRow);  
      });  
      if (spese.length === 0) {  
        for (let i = 0; i < 2; i++) addSpesaField();  
      }  
    }  
    function calculateTotaleEntrate() {  
      let sum = 0;  
      document.querySelectorAll('.entrate-amount').forEach(input => {  
        const val = parseFloat(input.value);  
        if (!isNaN(val)) sum += val;  
      });  
      const residuo = parseFloat(document.getElementById('residuo-mese').value);  
      if (!isNaN(residuo)) sum += residuo;  
      return sum;  
    }  
    function calculateTotaleEntrateSenzaResiduo() {  
      let sum = 0;  
      document.querySelectorAll('.entrate-amount').forEach(input => {  
        const val = parseFloat(input.value);  
        if (!isNaN(val)) sum += val;  
      });  
      return sum;  
    }  
    function calculateTotaleSpeseCategoria() {  
      let sum = 0;  
      document.querySelectorAll('.spesa-amount').forEach(input => {  
        const val = parseFloat(input.value);  
        if (!isNaN(val)) sum += val;  
      });  
      return sum;  
    }  
    function calculateTotaleSpeseTotale() {  
      const monthKey = getStorageKey(currentMonth);  
      const data = JSON.parse(localStorage.getItem(monthKey)) || { speseFisse: [], speseVariabili: [] };  
      let sumFisse = data.speseFisse.reduce((acc, e) => acc + e.amount, 0);  
      let sumVariabili = data.speseVariabili.reduce((acc, e) => acc + e.amount, 0);  
      return sumFisse + sumVariabili;  
    }  
    function updateAllTotals() {  
      document.getElementById('tot-spese-categoria').textContent =  
        `Totale spese categoria: € ${(calculateTotaleSpeseCategoria()).toFixed(2)}`;  
      document.getElementById('tot-spese-totale').textContent =  
        `Totale spese complessivo: € ${(calculateTotaleSpeseTotale()).toFixed(2)}`;  
      document.getElementById('totale-entrate').value = calculateTotaleEntrate().toFixed(2);  
    }  
    function updateSaldo() {  
      // Saldo effettivo (= tutto incluso residuo)  
      const totaleEntrate = calculateTotaleEntrate();  
      const totaleSpese = calculateTotaleSpeseTotale();  
      const saldoEff = totaleEntrate - totaleSpese;  
      document.getElementById('saldo-effettivo-val').textContent = `€ ${saldoEff.toFixed(2)}`;  
      // Budget mensile solo entrate mese (NO residuo!)  
      const entrateSenzaResiduo = calculateTotaleEntrateSenzaResiduo();  
      const budgetMensile = entrateSenzaResiduo - totaleSpese;  
      document.getElementById('budget-mensile-val').textContent = `€ ${budgetMensile.toFixed(2)}`;  
    }  
    function updateMonthlyResidual() {  
      const prevMonth = (currentMonth === 0) ? 11 : currentMonth - 1;  
      const prevData = JSON.parse(localStorage.getItem(getStorageKey(prevMonth))) || { entrate: [], speseFisse: [], speseVariabili: [], residuo: 0 };  
      const entrateSum = prevData.entrate.reduce((acc, e) => acc + e.amount, 0) + (prevData.residuo || 0);  
      const speseSum = [...(prevData.speseFisse || []), ...(prevData.speseVariabili || [])].reduce((acc, s) => acc + s.amount, 0);  
      const prevSaldo = entrateSum - speseSum;  
      document.getElementById('residuo-mese').value = prevSaldo.toFixed(2);  
      saveCurrentMonthData();  
    }  
    document.getElementById('tipo-spesa').addEventListener('change', (e) => {  
      saveCurrentMonthData();  
      currentSpesaType = e.target.value;  
      loadMonthData(currentMonth);  
    });  
    document.getElementById('add-entrata').addEventListener('click', () => {  
      addEntrataField();  
      saveCurrentMonthData();  
    });  
    document.getElementById('add-spesa').addEventListener('click', () => {  
      addSpesaField();  
      saveCurrentMonthData();  
    });  
    document.getElementById('residuo-mese').addEventListener('input', () => {  
      saveCurrentMonthData();  
      updateAllTotals();  
      updateSaldo();  
    });  
    window.addEventListener('DOMContentLoaded', () => {  
      renderMonths();  
      propagateFisseIfEmpty();  
      loadMonthData(currentMonth);  
      updateMonthlyResidual();  
      updateAllTotals();  
      updateSaldo();  
      setSelectedMonth(currentMonth);  
    });  
  </script>  
</body>  
</html>  
