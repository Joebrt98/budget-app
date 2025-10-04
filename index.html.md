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
      margin: 0;  
      padding: 0;  
      overflow-x: hidden;  
      max-width: 100vw;  
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,  
        Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;  
      background: #f7f3ee;  
      color: #000;  
      -webkit-font-smoothing: antialiased;  
      -moz-osx-font-smoothing: grayscale;  
    }  
    .top-bar {  
      position: fixed;  
      top: 0;  
      left: 0;  
      right: 0;  
      background: #0058db;  
      border-radius: 0 0 12px 12px;  
      box-shadow: 0 2px 6px rgba(0,0,0,0.15);  
      z-index: 50;  
      padding: 6px 10px 14px 10px;  
      text-align: center;  
    }  
    #menu-toggle {  
      font-size: 28px;  
      padding: 6px 12px 8px 12px;  
      color: #fff;  
      background: transparent;  
      border: none;  
      border-radius: 8px;  
      cursor: pointer;  
      user-select: none;  
      margin: 0 auto 6px;  
      display: block;  
      transition: background-color 0.25s ease;  
      position: relative;  
      z-index: 60;  
    }  
    #menu-toggle:hover,  
    #menu-toggle:focus {  
      background-color: rgba(255,255,255,0.15);  
      outline: none;  
    }  
    .tab-bar {  
      display: flex;  
      justify-content: center;  
      gap: 12px; /* ridotto e usato gap */  
      padding: 4px 0;  
      user-select: none;  
      position: relative;  
      z-index: 55;  
      flex-wrap: nowrap;  
      overflow-x: visible; /* rimosso scroll */  
    }  
    .tab {  
      color: #fff;  
      font-weight: 600;  
      font-size: 16px;  
      border-radius: 20px;  
      cursor: pointer;  
      padding: 9px 14px; /* ridotti i padding */  
      min-width: 70px;  
      text-align: center;  
      transition: background-color 0.3s ease;  
      flex: 0 0 auto;  
      white-space: nowrap;  
    }  
    .tab.active {  
      background: #00cc55;  
      font-weight: 700;  
    }  
    .side-menu {  
      position: fixed;  
      top: 0;  
      left: -230px;  
      width: 230px;  
      height: 100vh;  
      background: #fff;  
      box-shadow: 2px 0 13px rgba(0,0,0,0.2);  
      transition: left 0.3s ease;  
      z-index: 10000;  
      padding: 72px 16px 24px 16px;  
      font-size: 18px;  
      font-weight: 500;  
      box-sizing: border-box;  
      overflow-y: auto;  
      -webkit-overflow-scrolling: touch;  
    }  
    .side-menu.open {  
      left: 0;  
    }  
    .side-menu ul {  
      list-style: none;  
      margin: 0;  
      padding: 0;  
      max-height: 80vh;  
      overflow-y: auto;  
    }  
    .side-menu li {  
      margin-bottom: 16px;  
      cursor: pointer;  
      border-radius: 8px;  
      padding: 8px 12px;  
      transition: background-color 0.25s, color 0.25s;  
      font-weight: 500;  
      color: #333;  
    }  
    .side-menu li:hover {  
      background-color: #e0f2e4;  
      color: #007a2b;  
    }  
    .side-menu li.selected {  
      background-color: #b2e6b9;  
      font-weight: 700;  
      color: #00561a;  
    }  
    .content {  
      padding: 130px 22px 40px;  
      max-width: 420px;  
      margin: 0 auto;  
      position: relative;  
      width: 100%;  
      box-sizing: border-box;  
    }  
    h2.section-title {  
      font-size: 22px;  
      font-weight: 700;  
      margin: 28px 0 18px 0;  
      text-align: center;  
      color: #000;  
    }  
    .field-list {  
      display: flex;  
      flex-direction: column;  
      gap: 14px;  
      margin-bottom: 10px;  
    }  
    .field-row {  
      display: flex;  
      gap: 12px;  
      width: 100%;  
      box-sizing: border-box;  
    }  
    .field-row input[type="text"] {  
      width: 60%;  
      flex: none;  
      padding: 12px 14px;  
      font-size: 18px;  
      border: 2px solid #b3dff0;  
      border-radius: 12px;  
      outline-offset: 2px;  
      transition: border-color 0.3s;  
      background: #fefefe;  
      box-sizing: border-box;  
    }  
    .field-row input[type="number"] {  
      width: 38%; /* aumentato */  
      flex: none;  
      padding: 12px 14px;  
      font-size: 18px;  
      border: 2px solid #b3dff0;  
      border-radius: 12px;  
      outline-offset: 2px;  
      transition: border-color 0.3s;  
      background: #fefefe;  
      box-sizing: border-box;  
      text-align: right;  
    }  
    .field-row input[type="text"]:focus,  
    .field-row input[type="number"]:focus {  
      border-color: #00cc55;  
    }  
    .add-btn {  
      background: #00cc55;  
      color: white;  
      border: none;  
      padding: 10px 20px;  
      border-radius: 30px;  
      font-weight: 700;  
      cursor: pointer;  
      align-self: flex-start;  
      margin-bottom: 36px;  
      user-select: none;  
      box-shadow: 0 3px 10px rgba(0, 204, 85, 0.5);  
      transition: background-color 0.3s, box-shadow 0.3s;  
    }  
    .add-btn:hover {  
      background: #009944;  
      box-shadow: 0 5px 15px rgba(0, 153, 68, 0.7);  
    }  
    .expenses-sections {  
      display: flex;  
      gap: 34px;  
      margin-bottom: 6px;  
      box-sizing: border-box;  
      justify-content: center;  
      flex-wrap: wrap;  
    }  
    .expenses-col {  
      flex: 1;  
      box-sizing: border-box;  
      min-width: 150px;  
      max-width: 200px;  
      background: #ffffffcc;  
      border-radius: 14px;  
      padding: 18px 16px;  
      box-shadow: 2px 0 12px rgba(0,0,0,0.05);  
      display: flex;  
      flex-direction: column;  
    }  
    .totals-row {  
      display: flex;  
      flex-direction: column;  
      gap: 16px;  
      margin-bottom: 36px;  
      max-width: 420px;  
      margin-left: auto;  
      margin-right: auto;  
    }  
    .totals-row label {  
      font-weight: 700;  
      margin-bottom: 8px;  
      font-size: 17px;  
    }  
    .totals-row input {  
      background: #f5f3ed;  
      font-weight: 700;  
      border-radius: 14px;  
      padding: 12px 16px;  
      font-size: 20px;  
      border: none;  
      text-align: right;  
      user-select: none;  
      max-width: 100%;  
      box-sizing: border-box;  
      box-shadow: inset 1px 1px 4px #ccc;  
    }  
    .totals-section {  
      margin-top: 12px;  
      padding-top: 16px;  
      border-top: 1.5px solid #ccc;  
      font-weight: 700;  
      font-size: 17px;  
      text-align: right;  
      user-select: none;  
      color: #00561a;  
    }  
    .saldo-section {  
      display: flex;  
      justify-content: space-between;  
      gap: 14px;  
      margin-top: 20px;  
      width: 100%;  
      box-sizing: border-box;  
    }  
    .saldo-box {  
      flex: 1;  
      background: rgba(0,204,85,0.18);  
      border-radius: 18px;  
      padding: 22px 24px;  
      font-weight: 700;  
      font-size: 20px;  
      text-align: center;  
      user-select: none;  
      box-sizing: border-box;  
      box-shadow: 0 0 10px rgba(0,0,0,0.05);  
      transition: background-color 0.3s;  
    }  
    .saldo-box:hover {  
      background-color: rgba(0,204,85,0.23);  
    }  
    .header-space {  
      height: 130px; /* spazio per top bar */  
    }  
    @media (max-width: 430px) {  
      .tab-bar {  
        padding: 4px 0;  
      }  
      .tab {  
        font-size: 16px;  
        padding: 10px 10px;  
        margin-left: 10px;  
        margin-right: 10px;  
      }  
      .expenses-sections {  
        gap: 24px;  
        justify-content: center;  
      }  
      .expenses-col {  
        width: 48%;  
        max-width: none;  
        min-width: 0;  
        margin: 0 auto 24px;  
        box-shadow: 0 0 7px rgba(0,0,0,0.12);  
        padding: 16px 14px;  
      }  
      .field-row input[type="text"] {  
        font-size: 16px;  
        padding: 10px 12px;  
      }  
      .field-row input[type="number"] {  
        font-size: 16px;  
        padding: 10px 12px;  
      }  
      .add-btn {  
        padding: 10px 20px;  
        font-weight: 700;  
      }  
      .saldo-box {  
        font-size: 18px;  
        padding: 20px 16px;  
      }  
      .content {  
        max-width: 420px;  
        padding-left: 18px;  
        padding-right: 18px;  
      }  
      .totals-row input {  
        padding: 10px 14px;  
        font-size: 18px;  
      }  
      .totals-section {  
        font-size: 15px;  
      }  
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
      <div class="expenses-sections" role="group" aria-label="Sezione spese fisse e variabili">  
        <div class="expenses-col">  
          <h3>Spese Fisse</h3>  
          <div class="field-list" id="spese-fisse-list">  
            <div class="field-row">  
              <input type="text" placeholder="Etichetta" class="spesa-fissa-label" />  
              <input type="number" placeholder="Importo" class="spesa-fissa-amount" />  
            </div>  
            <div class="field-row">  
              <input type="text" placeholder="Etichetta" class="spesa-fissa-label" />  
              <input type="number" placeholder="Importo" class="spesa-fissa-amount" />  
            </div>  
          </div>  
          <div class="totals-section" id="tot-spese-fisse">Totale spese fisse: € 0.00</div>  
          <button class="add-btn" id="add-spesa-fissa" aria-label="Aggiungi spesa fissa">+ Aggiungi spesa fissa</button>  
        </div>  
        <div class="expenses-col">  
          <h3>Spese Variabili</h3>  
          <div class="field-list" id="spese-variabili-list">  
            <div class="field-row">  
              <input type="text" placeholder="Etichetta" class="spesa-variabile-label" />  
              <input type="number" placeholder="Importo" class="spesa-variabile-amount" />  
            </div>  
            <div class="field-row">  
              <input type="text" placeholder="Etichetta" class="spesa-variabile-label" />  
              <input type="number" placeholder="Importo" class="spesa-variabile-amount" />  
            </div>  
          </div>  
          <div class="totals-section" id="tot-spese-variabili">Totale spese variabili: € 0.00</div>  
          <button class="add-btn" id="add-spesa-variabile" aria-label="Aggiungi spesa variabile">+ Aggiungi spesa variabile</button>  
        </div>  
      </div>  
    </section>  
  
    <section id="content-saldo" style="display:none">  
      <div class="saldo-section" role="region" aria-label="Sezione saldo">  
        <div class="saldo-box" id="saldo-effettivo" aria-live="polite">Saldo Effettivo<br><span id="saldo-effettivo-val">€ 0.00</span></div>  
        <div class="saldo-box" id="budget-mensile" aria-live="polite">Budget Mensile<br><span id="budget-mensile-val">€ 0.00</span></div>  
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
    const months = [  
      "Gennaio",  
      "Febbraio",  
      "Marzo",  
      "Aprile",  
      "Maggio",  
      "Giugno",  
      "Luglio",  
      "Agosto",  
      "Settembre",  
      "Ottobre",  
      "Novembre",  
      "Dicembre",  
    ];  
    let currentMonth = new Date().getMonth();  
  
    function renderMonths() {  
      const monthsList = document.getElementById('months-list');  
      monthsList.innerHTML = '';  
      months.forEach((month, index) => {  
        const li = document.createElement('li');  
        li.textContent = month;  
        if(index === currentMonth) li.classList.add('selected');  
        li.tabIndex = 0;  
        li.addEventListener('click', () => {  
          saveCurrentMonthData();  
          currentMonth = index;  
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
      if(!sideMenu.contains(e.target) && e.target !== menuToggle) {  
        sideMenu.classList.remove('open');  
      }  
    });  
  
    tabs.forEach(tab => {  
      tab.addEventListener('click', () => {  
        tabs.forEach((t) => {  
          t.classList.remove('active');  
          t.setAttribute('aria-selected', 'false');  
          t.tabIndex = -1;  
        });  
        tab.classList.add('active');  
        tab.setAttribute('aria-selected', 'true');  
        tab.tabIndex = 0;  
  
        if(tab.id === 'tab-entrate') {  
          sections.entrate.style.display = 'block';  
          sections.spese.style.display = 'none';  
          sections.saldo.style.display = 'none';  
        } else if(tab.id === 'tab-spese') {  
          sections.entrate.style.display = 'none';  
          sections.spese.style.display = 'flex';  
          sections.saldo.style.display = 'none';  
        } else if(tab.id === 'tab-saldo') {  
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
      const data = {  
        residuo: parseFloat(document.getElementById('residuo-mese').value) || 0,  
        entrate: [],  
        speseFisse: [],  
        speseVariabili: []  
      };  
      document.querySelectorAll('#entrate-list .field-row').forEach(row => {  
        const label = row.querySelector('.entrate-label').value.trim();  
        const amount = parseFloat(row.querySelector('.entrate-amount').value);  
        if(label !== '' && !isNaN(amount)) data.entrate.push({label, amount});  
      });  
      document.querySelectorAll('#spese-fisse-list .field-row').forEach(row => {  
        const label = row.querySelector('.spesa-fissa-label').value.trim();  
        const amount = parseFloat(row.querySelector('.spesa-fissa-amount').value);  
        if(label !== '' && !isNaN(amount)) data.speseFisse.push({label, amount});  
      });  
      document.querySelectorAll('#spese-variabili-list .field-row').forEach(row => {  
        const label = row.querySelector('.spesa-variabile-label').value.trim();  
        const amount = parseFloat(row.querySelector('.spesa-variabile-amount').value);  
        if(label !== '' && !isNaN(amount)) data.speseVariabili.push({label, amount});  
      });  
      localStorage.setItem(monthKey, JSON.stringify(data));  
    }  
    function loadMonthData(month) {  
      clearAllFields();  
      const monthKey = getStorageKey(month);  
      const data = JSON.parse(localStorage.getItem(monthKey));  
      if(data) {  
        document.getElementById('residuo-mese').value = data.residuo || 0;  
        const entrateList = document.getElementById('entrate-list');  
        data.entrate.forEach((entry, i) => {  
          if(i >= entrateList.children.length) addEntrataField();  
          entrateList.children[i].querySelector('.entrate-label').value = entry.label;  
          entrateList.children[i].querySelector('.entrate-amount').value = entry.amount;  
        });  
        const speseFisseList = document.getElementById('spese-fisse-list');  
        data.speseFisse.forEach((entry, i) => {  
          if(i >= speseFisseList.children.length) addSpesaFissaField();  
          speseFisseList.children[i].querySelector('.spesa-fissa-label').value = entry.label;  
          speseFisseList.children[i].querySelector('.spesa-fissa-amount').value = entry.amount;  
        });  
        const speseVariabiliList = document.getElementById('spese-variabili-list');  
        data.speseVariabili.forEach((entry, i) => {  
          if(i >= speseVariabiliList.children.length) addSpesaVariabileField();  
          speseVariabiliList.children[i].querySelector('.spesa-variabile-label').value = entry.label;  
          speseVariabiliList.children[i].querySelector('.spesa-variabile-amount').value = entry.amount;  
        });  
      } else {  
        document.getElementById('residuo-mese').value = 0;  
      }  
      updateAllTotals();  
      updateSaldo();  
    }  
    function clearAllFields() {  
      ['entrate-list', 'spese-fisse-list', 'spese-variabili-list'].forEach(id => {  
        const container = document.getElementById(id);  
        container.innerHTML = '';  
      });  
      for(let i=0; i<3; i++) addEntrataField();  
      for(let i=0; i<2; i++) addSpesaFissaField();  
      for(let i=0; i<2; i++) addSpesaVariabileField();  
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
    function addSpesaFissaField() {  
      const container = document.getElementById('spese-fisse-list');  
      const newRow = document.createElement('div');  
      newRow.className = 'field-row';  
      newRow.innerHTML = `<input type="text" placeholder="Etichetta" class="spesa-fissa-label" />  
                          <input type="number" placeholder="Importo" class="spesa-fissa-amount" />`;  
      container.appendChild(newRow);  
      addInputListeners(newRow);  
    }  
    function addSpesaVariabileField() {  
      const container = document.getElementById('spese-variabili-list');  
      const newRow = document.createElement('div');  
      newRow.className = 'field-row';  
      newRow.innerHTML = `<input type="text" placeholder="Etichetta" class="spesa-variabile-label" />  
                          <input type="number" placeholder="Importo" class="spesa-variabile-amount" />`;  
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
  
    function calculateTotaleEntrate() {  
      let sum = 0;  
      document.querySelectorAll('.entrate-amount').forEach(input => {  
        const val = parseFloat(input.value);  
        if(!isNaN(val)) sum += val;  
      });  
      const residuo = parseFloat(document.getElementById('residuo-mese').value);  
      if(!isNaN(residuo)) sum += residuo;  
      return sum;  
    }  
    function calculateTotaleSpeseFisse() {  
      let sum = 0;  
      document.querySelectorAll('.spesa-fissa-amount').forEach(input => {  
        const val = parseFloat(input.value);  
        if(!isNaN(val)) sum += val;  
      });  
      return sum;  
    }  
    function calculateTotaleSpeseVariabili() {  
      let sum = 0;  
      document.querySelectorAll('.spesa-variabile-amount').forEach(input => {  
        const val = parseFloat(input.value);  
        if(!isNaN(val)) sum += val;  
      });  
      return sum;  
    }  
    function updateAllTotals() {  
      document.getElementById('tot-spese-fisse').textContent = "Totale spese fisse: € " + calculateTotaleSpeseFisse().toFixed(2);  
      document.getElementById('tot-spese-variabili').textContent = "Totale spese variabili: € " + calculateTotaleSpeseVariabili().toFixed(2);  
      document.getElementById('totale-entrate').value = calculateTotaleEntrate().toFixed(2);  
    }  
    function updateSaldo() {  
      const totaleEntrate = calculateTotaleEntrate();  
      const totaleSpese = calculateTotaleSpeseFisse() + calculateTotaleSpeseVariabili();  
      const saldoEff = totaleEntrate - totaleSpese;  
      document.getElementById('saldo-effettivo-val').textContent = "€ " + saldoEff.toFixed(2);  
      document.getElementById('budget-mensile-val').textContent = "€ " + saldoEff.toFixed(2);  
    }  
    function updateMonthlyResidual() {  
      const prevMonth = (currentMonth === 0) ? 11 : currentMonth - 1;  
      const prevData = JSON.parse(localStorage.getItem(getStorageKey(prevMonth)));  
      let prevSaldo = 0;  
      if(prevData) {  
        const entrateSum = prevData.entrate.reduce((acc, e) => acc + e.amount, 0) + (prevData.residuo || 0);  
        const speseFisseSum = prevData.speseFisse.reduce((acc, s) => acc + s.amount, 0);  
        const speseVariabiliSum = prevData.speseVariabili.reduce((acc, s) => acc + s.amount, 0);  
        prevSaldo = entrateSum - (speseFisseSum + speseVariabiliSum);  
      }  
      document.getElementById('residuo-mese').value = prevSaldo.toFixed(2);  
      saveCurrentMonthData();  
    }  
  
    window.addEventListener('DOMContentLoaded', () => {  
      renderMonths();  
      loadMonthData(currentMonth);  
      updateMonthlyResidual();  
      addCalculationListeners();  
      updateSaldo();  
      setSelectedMonth(currentMonth);  
    });  
  </script>  
</body>  
</html>  
