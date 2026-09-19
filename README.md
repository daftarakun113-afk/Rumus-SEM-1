# Rumus-SEM-1
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kalkulator Konsumsi Bensin & Energi Listrik</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    body {
      background-color: #0f172a;
      color: #f8fafc;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
    }

    .container {
      background-color: #1e293b;
      padding: 28px;
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
      width: 100%;
      max-width: 520px;
      border: 1px solid #334155;
    }

    h2 {
      font-size: 1.4rem;
      margin-bottom: 20px;
      text-align: center;
      color: #38bdf8;
    }

    .form-group {
      margin-bottom: 16px;
    }

    label {
      display: block;
      font-size: 0.88rem;
      margin-bottom: 6px;
      color: #cbd5e1;
    }

    .input-wrapper {
      display: flex;
      gap: 8px;
    }

    input[type="number"], select {
      width: 100%;
      padding: 12px;
      background-color: #0f172a;
      border: 1px solid #475569;
      border-radius: 8px;
      color: #ffffff;
      font-size: 1rem;
      outline: none;
      transition: border-color 0.2s;
    }

    input[type="number"]:focus, select:focus {
      border-color: #38bdf8;
    }

    select {
      width: auto;
      cursor: pointer;
    }

    button {
      width: 100%;
      padding: 14px;
      margin-top: 10px;
      background-color: #0284c7;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 1rem;
      font-weight: bold;
      cursor: pointer;
      transition: background-color 0.2s;
    }

    button:hover {
      background-color: #0369a1;
    }

    .result-container {
      margin-top: 24px;
      padding: 16px;
      background-color: #0f172a;
      border-radius: 8px;
      border-left: 4px solid #38bdf8;
      display: none;
    }

    .result-item {
      margin-bottom: 12px;
    }

    .result-item:last-child {
      margin-bottom: 0;
    }

    .result-label {
      font-size: 0.85rem;
      color: #94a3b8;
    }

    .result-value {
      font-size: 1.2rem;
      font-weight: bold;
      color: #4ade80;
    }

    .spec-info {
      margin-top: 16px;
      padding: 12px;
      background-color: #182232;
      border-radius: 6px;
      font-size: 0.75rem;
      color: #94a3b8;
      line-height: 1.5;
    }
  </style>
</head>
<body>

  <div class="container">
    <h2>Kalkulator Konsumsi Bensin & Listrik</h2>

    <div class="form-group">
      <label for="bensin">1. Volume Bensin Utama (ml)</label>
      <input type="number" id="bensin" step="any" placeholder="Masukkan volume bensin (ml)" required>
    </div>

    <div class="form-group">
      <label for="energi">2. Konsumsi Listrik Tambahan</label>
      <div class="input-wrapper">
        <input type="number" id="energi" step="any" placeholder="Masukkan nilai energi" required>
        <select id="satuanEnergi">
          <option value="Wh">Wh</option>
          <option value="Joule">Joule</option>
        </select>
      </div>
    </div>

    <div class="form-group">
      <label for="jarak">3. Jarak Tempuh (km)</label>
      <input type="number" id="jarak" step="any" placeholder="Masukkan jarak (km)" required>
    </div>

    <button onclick="hitung()">Hitung Konsumsi</button>

    <div id="hasil" class="result-container">
      <div class="result-item">
        <div class="result-label">Volume Bensin Utama:</div>
        <div id="bensinUtamaVal" class="result-value">0 ml</div>
      </div>
      <div class="result-item">
        <div class="result-label">Energi Listrik Terkonversi:</div>
        <div id="jouleVal" class="result-value">0 Joule</div>
      </div>
      <div class="result-item">
        <div class="result-label">Konsumsi Listrik ke Setara Bensin:</div>
        <div id="setaraBensinVal" class="result-value">0 ml</div>
      </div>
      <div class="result-item">
        <div class="result-label">Total Volume Bensin Konsumsi:</div>
        <div id="totalBensinVal" class="result-value">0 ml</div>
      </div>
      <div class="result-item">
        <div class="result-label">Hasil Akhir Konsumsi:</div>
        <div id="efisiensiVal" class="result-value">0 km/liter</div>
      </div>
    </div>

    <div class="spec-info">
      <strong>Parameter Acuan:</strong><br>
      • NCV Bensin = 42.900 kJ/kg (42.900 J/g)<br>
      • Densitas Pertamax Turbo = 0,7425 kg/l (0,7425 g/ml)<br>
      • Efisiensi Mesin = 25% (0,25)<br>
      • Efisiensi Alternator = 75% (0,75)<br>
      • Konversi Energi: 1 Wh = 3.600 Joule<br>
      • Rumus Bensin dari Listrik = <code>(Joule / (0,25 × 0,75)) / (42.900 × 0,7425)</code><br>
      • Rumus Hasil Akhir = <code>Jarak (km) / (Total ml / 1000)</code>
    </div>
  </div>

  <script>
    function hitung() {
      const bensinUtamaInput = parseFloat(document.getElementById('bensin').value);
      const energiInput = parseFloat(document.getElementById('energi').value);
      const satuan = document.getElementById('satuanEnergi').value;
      const jarakInput = parseFloat(document.getElementById('jarak').value);

      if (isNaN(bensinUtamaInput) || isNaN(energiInput) || isNaN(jarakInput) || jarakInput <= 0) {
        alert("Mohon isi semua data dengan angka yang benar!");
        return;
      }

      // Konstanta Parameter Spesifikasi
      const ncv = 42900; // J/g
      const densitas = 0.7425; // g/ml
      const efisiensiMesin = 0.25;
      const efisiensiAlternator = 0.75;

      // 1. Volume Bensin Utama (ml) - Rasio Konversi 1
      const bensinUtama = bensinUtamaInput * 1;

      // 2. Konversi Wh ke Joule (1 Wh = 3600 Joule)
      let joule = energiInput;
      if (satuan === 'Wh') {
        joule = energiInput * 3600;
      }

      // 3. Konsumsi Listrik Tambahan ke Setara Bensin (ml) dari Joule
      const bensinSetaraListrik = (joule / (efisiensiMesin * efisiensiAlternator)) / (ncv * densitas);

      // 4. Total Volume Bensin Konsumsi (ml)
      const totalBensinMl = bensinUtama + bensinSetaraListrik;

      // 5. Hasil Akhir Konsumsi: Jarak (km) / (Total ml / 1000)
      const totalBensinLiter = totalBensinMl / 1000;
      const hasilAkhirKonsumsi = jarakInput / totalBensinLiter;

      // Tampilkan Hasil Perhitungan
      document.getElementById('bensinUtamaVal').innerText = bensinUtama.toLocaleString('id-ID', { maximumFractionDigits: 3 }) + ' ml';
      document.getElementById('jouleVal').innerText = joule.toLocaleString('id-ID', { maximumFractionDigits: 2 }) + ' Joule';
      document.getElementById('setaraBensinVal').innerText = bensinSetaraListrik.toLocaleString('id-ID', { maximumFractionDigits: 4 }) + ' ml';
      document.getElementById('totalBensinVal').innerText = totalBensinMl.toLocaleString('id-ID', { maximumFractionDigits: 3 }) + ' ml (' + totalBensinLiter.toLocaleString('id-ID', { maximumFractionDigits: 5 }) + ' liter)';
      document.getElementById('efisiensiVal').innerText = hasilAkhirKonsumsi.toLocaleString('id-ID', { maximumFractionDigits: 2 }) + ' km/liter';
      
      document.getElementById('hasil').style.display = 'block';
    }
  </script>

</body>
</html>
