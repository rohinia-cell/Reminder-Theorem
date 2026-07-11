# Reminder-Theorem
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Remainder Theorem — Polynomial Division</title>
<style>
  * { box-sizing: border-box; }
  body {
    margin: 0;
    min-height: 100vh;
    font-family: 'Segoe UI', Arial, sans-serif;
    background: linear-gradient(135deg, #6a3093, #a044ff, #2c0d54);
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 30px 12px;
  }
  .card {
    background: #fff;
    width: 600px;
    max-width: 95vw;
    border-radius: 16px;
    padding: 30px 30px 26px;
    box-shadow: 0 25px 50px rgba(0,0,0,0.4);
  }
  h1 {
    font-size: 21px;
    margin: 0 0 6px;
    color: #2c0d54;
    text-align: center;
  }
  p.sub {
    text-align: center;
    color: #666;
    font-size: 13px;
    margin: 0 0 18px;
  }
  .rule {
    background: #f4ecfc;
    border-left: 4px solid #a044ff;
    padding: 12px 14px;
    border-radius: 8px;
    font-size: 13.5px;
    color: #2c0d54;
    margin-bottom: 20px;
    line-height: 1.5;
  }
  .rule b { color: #180533; }

  label {
    display: block;
    font-size: 13px;
    font-weight: 600;
    color: #333;
    margin-bottom: 6px;
  }
  input {
    width: 100%;
    padding: 10px 12px;
    border: 1px solid #ccc;
    border-radius: 8px;
    font-size: 14px;
    outline: none;
  }
  input:focus { border-color: #a044ff; }

  .hint {
    font-size: 12px;
    color: #888;
    margin-top: 4px;
  }

  .row2 { display: flex; gap: 12px; margin-top: 14px; }
  .row2 > div { flex: 1; }

  button#calcBtn {
    width: 100%;
    margin-top: 18px;
    padding: 12px;
    border: none;
    border-radius: 8px;
    background: #6a3093;
    color: #fff;
    font-size: 15px;
    font-weight: 600;
    cursor: pointer;
  }
  button#calcBtn:hover { background: #7d3ead; }

  .error {
    color: #c0392b;
    font-size: 13px;
    margin-top: 10px;
    text-align: center;
    display: none;
  }

  .results { margin-top: 20px; display: none; }
  .results.show { display: block; }

  .summary {
    background: #2c0d54;
    color: #fff;
    padding: 16px;
    border-radius: 10px;
    text-align: center;
    margin-bottom: 16px;
  }
  .summary .big { font-size: 22px; font-weight: 700; }
  .summary .small { font-size: 13px; opacity: 0.85; margin-top: 4px; }

  .section-title {
    font-size: 13px;
    font-weight: 700;
    color: #2c0d54;
    margin: 14px 0 8px;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
    margin-bottom: 6px;
  }
  th, td {
    padding: 7px 6px;
    text-align: center;
    border-bottom: 1px solid #eee;
  }
  th { color: #a044ff; }
  td.carry { color: #a044ff; font-size: 11px; }

  .app-select-row {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    margin-bottom: 14px;
  }
  .app-select-row button {
    flex: 1;
    min-width: 110px;
    padding: 8px 10px;
    font-size: 12px;
    border: 1px solid #6a3093;
    background: #fff;
    color: #6a3093;
    border-radius: 8px;
    cursor: pointer;
  }
  .app-select-row button.active { background: #6a3093; color: #fff; }

  .app-box {
    background: #f8f4fc;
    border-radius: 10px;
    padding: 14px;
    font-size: 13.5px;
    color: #333;
    line-height: 1.6;
  }
  .app-box b { color: #2c0d54; }
</style>
</head>
<body>

<div class="card">
  <h1>The Remainder Theorem</h1>
  <p class="sub">If a polynomial p(x) is divided by (x − a), the remainder equals p(a).</p>

  <div class="rule">
    <b>Statement:</b> For a polynomial p(x) divided by the linear factor <b>(x − a)</b>,
    the remainder r is simply <b>r = p(a)</b> — no long division needed, just substitute x = a.<br>
    <b>Corollary (Factor Theorem):</b> if p(a) = 0, then (x − a) is an exact factor of p(x).
  </div>

  <div class="app-select-row">
    <button class="active" onclick="setPreset('engineering')">🏗️ Engineering Stress</button>
    <button onclick="setPreset('economics')">📈 Cost/Profit Model</button>
    <button onclick="setPreset('coding')">💾 Error-Correcting Codes</button>
    <button onclick="setPreset('custom')">🔢 Custom</button>
  </div>

  <label for="poly">Polynomial p(x) — coefficients from highest to lowest power, comma-separated</label>
  <input type="text" id="poly" value="2,3,-5,7" placeholder="e.g. 2,3,-5,7 means 2x^3+3x^2-5x+7">
  <div class="hint">Example: "2,3,-5,7" represents 2x³ + 3x² − 5x + 7</div>

  <div class="row2">
    <div>
      <label for="aVal">Divide by (x − a): value of a</label>
      <input type="number" id="aVal" value="2">
    </div>
  </div>

  <button id="calcBtn" onclick="calculate()">Find the Remainder</button>

  <p class="error" id="errorMsg">Please enter valid comma-separated coefficients and a valid value for a.</p>

  <div class="results" id="results">
    <div class="summary">
      <div class="big" id="polyExpr">-</div>
      <div class="small" id="remainderText">-</div>
    </div>

    <div class="section-title">Method: Synthetic Division</div>
    <table id="synthTable"><tbody></tbody></table>

    <div class="section-title" id="appTitle">Real-World Application</div>
    <div class="app-box" id="appBox"></div>
  </div>
</div>

<script>
let currentMode = 'engineering';

function setPreset(mode) {
  currentMode = mode;
  document.querySelectorAll('.app-select-row button').forEach(b => b.classList.remove('active'));
  event.target.classList.add('active');

  if (mode === 'engineering') {
    document.getElementById('poly').value = '1,-6,11,-6';
    document.getElementById('aVal').value = 3;
  } else if (mode === 'economics') {
    document.getElementById('poly').value = '-2,40,-150,300';
    document.getElementById('aVal').value = 5;
  } else if (mode === 'coding') {
    document.getElementById('poly').value = '1,0,1,1,0,1';
    document.getElementById('aVal').value = 1;
  } else {
    document.getElementById('poly').value = '2,3,-5,7';
    document.getElementById('aVal').value = 2;
  }
  calculate();
}

function parsePoly(str) {
  return str.split(',').map(s => parseFloat(s.trim())).filter(n => !isNaN(n));
}

function polyToString(coeffs) {
  const deg = coeffs.length - 1;
  let terms = [];
  coeffs.forEach((c, i) => {
    const power = deg - i;
    if (c === 0) return;
    let term = '';
    const sign = c < 0 ? ' − ' : (terms.length ? ' + ' : '');
    const absC = Math.abs(c);
    const coefStr = (absC === 1 && power !== 0) ? '' : absC.toString();
    if (power === 0) term = `${coefStr || absC}`;
    else if (power === 1) term = `${coefStr}x`;
    else term = `${coefStr}x^${power}`;
    terms.push(sign + term);
  });
  return 'p(x) = ' + terms.join('').replace(/^\+ /, '');
}

function syntheticDivision(coeffs, a) {
  const result = [coeffs[0]];
  const carries = [];
  for (let i = 1; i < coeffs.length; i++) {
    const carry = result[i - 1] * a;
    carries.push(carry);
    result.push(coeffs[i] + carry);
  }
  const remainder = result[result.length - 1];
  const quotient = result.slice(0, -1);
  return { quotient, remainder, carries };
}

function calculate() {
  const coeffs = parsePoly(document.getElementById('poly').value);
  const a = parseFloat(document.getElementById('aVal').value);
  const errorMsg = document.getElementById('errorMsg');
  const results = document.getElementById('results');

  if (coeffs.length < 2 || isNaN(a)) {
    errorMsg.style.display = 'block';
    results.classList.remove('show');
    return;
  }
  errorMsg.style.display = 'none';

  const { quotient, remainder, carries } = syntheticDivision(coeffs, a);

  document.getElementById('polyExpr').textContent = polyToString(coeffs);
  document.getElementById('remainderText').textContent =
    `Divided by (x − ${a}):  p(${a}) = remainder = ${remainder}`;

  // Synthetic division table
  const tbody = document.querySelector('#synthTable tbody');
  tbody.innerHTML = '';

  let rowCoeffs = `<tr><th>a = ${a}</th>`;
  coeffs.forEach(c => rowCoeffs += `<td>${c}</td>`);
  rowCoeffs += `</tr>`;

  let rowCarry = `<tr><td></td><td></td>`;
  carries.forEach(c => rowCarry += `<td class="carry">+ ${c}</td>`);
  rowCarry += `</tr>`;

  let rowResult = `<tr><th>Result</th>`;
  const fullResult = [...quotient, remainder];
  fullResult.forEach((r, i) => {
    rowResult += `<td style="${i === fullResult.length - 1 ? 'font-weight:700;color:#a044ff;' : ''}">${r}</td>`;
  });
  rowResult += `</tr>`;

  tbody.innerHTML = rowCoeffs + rowCarry + rowResult;

  // Application text
  const appTitle = document.getElementById('appTitle');
  const appBox = document.getElementById('appBox');

  if (currentMode === 'engineering') {
    appTitle.textContent = 'Real-World Application: Structural Engineering';
    appBox.innerHTML = `Engineers model beam deflection or stress along a beam as a polynomial p(x).
      To quickly check the stress value at a specific support point x = <b>${a}</b> (without re-solving
      the whole equation), they use the Remainder Theorem: <b>p(${a}) = ${remainder}</b>. If the
      remainder is 0, that point is a root — meaning zero stress/deflection there, often marking a
      support or node.`;
  } else if (currentMode === 'economics') {
    appTitle.textContent = 'Real-World Application: Business Cost & Profit Models';
    appBox.innerHTML = `A company's profit is modeled as a polynomial p(x) in terms of units sold, x.
      To instantly find the profit at a specific production level x = <b>${a}</b> (e.g. ${a} thousand
      units), instead of expanding and simplifying the whole polynomial, they substitute directly:
      <b>p(${a}) = ${remainder}</b>. This is exactly what the Remainder Theorem guarantees.`;
  } else if (currentMode === 'coding') {
    appTitle.textContent = 'Real-World Application: Error-Correcting Codes (CRC)';
    appBox.innerHTML = `Data transmission (Wi-Fi, USB, hard drives) uses Cyclic Redundancy Check (CRC),
      where data bits are treated as polynomial coefficients. The transmitted message is divided by a
      fixed generator polynomial, and the <b>remainder</b> becomes a checksum. Here, dividing by
      (x − ${a}) gives remainder <b>${remainder}</b> — if it doesn't match the expected value at the
      receiver, the data is flagged as corrupted.`;
  } else {
    appTitle.textContent = 'Why This Matters';
    appBox.innerHTML = `Instead of performing full polynomial long division, the Remainder Theorem lets
      you find the remainder of p(x) ÷ (x − a) instantly, just by evaluating p(${a}) = <b>${remainder}</b>.
      This saves significant time in calculus, engineering, computer science, and economics whenever
      you need to test many values quickly.`;
  }

  results.classList.add('show');
}

calculate();
</script>

</body>
</html>
