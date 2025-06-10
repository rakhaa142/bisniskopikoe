<!DOCTYPE html>Add commentMore actions
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>KopiKoe Dashboard</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body { font-family: 'Segoe UI', sans-serif; background-color: #f2f2f2; margin: 0; padding: 20px; }
    #loginBox {
      background-color: #fff;
      padding: 30px;
      border-radius: 15px;
      box-shadow: 0 5px 20px rgba(0,0,0,0.1);
      width: 360px;
      margin: 100px auto;
      text-align: center;
    }
    #logo { font-size: 48px; }
    #brand { font-size: 28px; font-weight: bold; color: #5e3c2a; margin-bottom: 10px; }
    .input-group { position: relative; margin: 15px 0; }
    input[type="text"], input[type="password"], select {
      width: 100%;
      padding: 12px;
      padding-right: 40px;
      border: 1px solid #ccc;
      border-radius: 10px;
      box-sizing: border-box;
    }
    .eye {
      position: absolute;
      top: 50%;
      right: 12px;
      transform: translateY(-50%);
      cursor: pointer;
      font-size: 18px;
    }
    button {
      background-color: #5e3c2a;
      color: white;
      border: none;
      padding: 12px;
      width: 100%;
      border-radius: 10px;
      font-weight: bold;
      cursor: pointer;
    }
    form, table {
      background-color: #fff; border-radius: 10px; padding: 20px;
      margin: 20px auto; max-width: 900px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }
    h2, h3 { text-align: center; }
    table { width: 100%; border-collapse: collapse; }
    th, td { padding: 10px; border: 1px solid #ccc; text-align: left; }
    .hapus-btn { color: red; cursor: pointer; }
    #exportBtn { background-color: green; color: white; border: none; cursor: pointer; padding: 10px; }
    #dashboard { display: none; }
    .greeting { font-size: 14px; color: #5e3c2a; margin-bottom: 20px; }
  
    .error-msg {
      display: none;
      padding: 8px;
      border-radius: 8px;
      margin-top: 8px;
      font-size: 14px;
      font-weight: bold;
    }
    .error-msg.show {
      display: block !important;
      background-color: #5e3c2a !important;
      color: white !important;
    }

</style>
</head>
<body>

<div id="loginBox">
  <div id="logo">☕</div>
  <div id="brand">kopikoe</div>
  <div class="greeting">Selamat datang di sistem Kopikoe.<br>Lengkapi data keuanganmu dengan mudah.</div>
  <div class="input-group">
    <input type="text" id="username" placeholder="Username">
  </div>
  <div class="input-group">
    <input type="password" id="password" placeholder="Password">
    <span class="eye" id="toggleEye" onclick="togglePassword()">👁</span>
  </div>
  <button onclick="login()">Masuk</button>
</div>

<div id="dashboard">
  <h2>Catatan Keuangan Bisnis</h2>

  <form id="formBahan">
    <h3>Tambah Bahan Baku</h3>
    <input type="text" id="namaBahan" placeholder="Nama Bahan" required>
    <input type="text" id="hargaBahan" placeholder="Harga Total (Rp)" required>
    <input type="text" id="isiBahan" placeholder="Isi Total (gram/ml)" required>
    <div id="errorBahan" class="error-msg"></div>
    <button type="submit">Tambah Bahan</button>
  </form>

  <table id="tabelBahan">
    <thead><tr><th>Nama</th><th>Harga (Rp)</th><th>Isi</th><th>Modal/gr</th><th>Aksi</th></tr></thead>
    <tbody></tbody>
  </table>

  <form id="formResep">
    <h3>Tambah Resep Menu</h3>
    <select id="menuResep" required>
      <option value="">Pilih Menu</option>
      <option value="americano">Americano</option>
      <option value="creamy latte">Creamy Latte</option>
      <option value="gula aren">Gula Aren</option>
    </select>
    <select id="bahanResep" required></select>
    <input type="text" id="jumlahBahan" placeholder="Jumlah Pakai (gram/ml)" required>
    <div id="errorResep" class="error-msg"></div>
    <button type="submit">Tambah Resep</button>
  </form>

  <table id="tabelResep">
    <thead><tr><th>Menu</th><th>Bahan</th><th>Jumlah</th><th>Aksi</th></tr></thead>
    <tbody></tbody>
  </table>

  <form id="formTransaksi">
    <h3>Catat Transaksi</h3>
    <select id="jenisTransaksi">
      <option value="pemasukan">Pemasukan</option>
      <option value="pengeluaran">Pengeluaran</option>
    </select>
    <input type="text" id="keteranganTransaksi" placeholder="Contoh: creamy latte">
    <input type="text" id="jumlahTransaksi" placeholder="Harga Jual (Rp)">
    <select id="metodeBayar">
      <option value="">Pilih Metode Pembayaran</option>
      <option value="QRIS">QRIS</option>
      <option value="Cash">Cash</option>
    </select>
    <div id="errorTransaksi" class="error-msg"></div>
    <button type="submit">Catat</button>
  </form>

  <table id="tabelTransaksi">
    <thead><tr><th>Waktu</th><th>Jenis</th><th>Keterangan</th><th>Jumlah (Rp)</th><th>Cup</th><th>Modal</th><th>Untung</th><th>Metode</th><th>Aksi</th></tr></thead>
    <tbody></tbody>
  </table>

  <div style="text-align:center;"><button id="exportBtn">Export ke Excel</button></div>
  <div id="ringkasanKeuangan" style="max-width:900px;margin:20px auto;padding:20px;background-color:#fff;border-radius:10px;box-shadow:0 2px 10px rgba(0,0,0,0.1);font-weight:bold;color:#5e3c2a;">
    <p>Total Pemasukan: Rp <span id="totalPemasukan">0</span></p>
    <p>Total Pengeluaran: Rp <span id="totalPengeluaran">0</span></p>
    <p>Total Laba: Rp <span id="totalLaba">0</span></p>
    <p>Saldo Akhir: Rp <span id="saldoAkhir">0</span></p>
  </div>

</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<script>
  const bahan = [], resep = [], transaksi = [];

  function togglePassword() {
    const pass = document.getElementById("password");
    const eye = document.getElementById("toggleEye");
    if (pass.type === "password") {
      pass.type = "text"; eye.textContent = "👁‍🗨";
    } else {
      pass.type = "password"; eye.textContent = "👁";
    }
  }

  function login() {
    const u = document.getElementById("username").value.trim();
    const p = document.getElementById("password").value.trim();
    if (u === "admin" && p === "kopikoe123") {
      document.getElementById("loginBox").style.display = "none";
      document.getElementById("dashboard").style.display = "block";
    } else {
      alert("Username atau password salah!");
    }
  }

  function formatRp(val) {
    return parseInt(val || 0).toLocaleString('id-ID');
  }

  function updateBahanResep() {
    const bahanResep = document.getElementById('bahanResep');
    bahanResep.innerHTML = bahan.map(b => <option value="${b.nama}">${b.nama}</option>).join('');
  }

  document.getElementById('formBahan').addEventListener('submit', e => {
    e.preventDefault();
    const nama = document.getElementById('namaBahan').value;
    const harga = parseInt(document.getElementById('hargaBahan').value.replace(/\./g, ''));
    const isi = parseInt(document.getElementById('isiBahan').value.replace(/\./g, ''));
    bahan.push({ nama, harga, isi });
    transaksi.push({ waktu: new Date().toLocaleString(), jenis: 'pengeluaran', keterangan: Bahan: ${nama}, jumlah: harga });
    updateBahanResep(); renderBahan(); renderTransaksi(); e.target.reset();
  });

  document.getElementById('formResep').addEventListener('submit', e => {
    e.preventDefault();
    const menu = document.getElementById('menuResep').value;
    const bahanNama = document.getElementById('bahanResep').value;
    const jumlah = parseInt(document.getElementById('jumlahBahan').value.replace(/\./g, ''));
    resep.push({ menu, bahan: bahanNama, jumlah });
    renderResep(); e.target.reset();
  });

  document.getElementById('formTransaksi').addEventListener('submit', e => {
      e.preventDefault();
      const jenis = document.getElementById('jenisTransaksi').value;
      const ket = document.getElementById('keteranganTransaksi').value.toLowerCase().trim();
      const jmlStr = document.getElementById('jumlahTransaksi').value.replace(/\./g, '');
      const jml = parseInt(jmlStr);
      const metode = document.getElementById('metodeBayar').value;
      const errorMsg = document.getElementById("errorTransaksi");

      if (!jenis || !ket || !jmlStr || isNaN(jml) || !metode) {
        errorMsg.classList.add("show");
        errorMsg.style.backgroundColor = "#5e3c2a";
        errorMsg.style.color = "white";
        return false;
      }
      errorMsg.classList.remove("show");
      const now = new Date();
      let cup = '', totalModal = 0, untung = 0;

      if (jenis === 'pemasukan') {
        const menuResep = resep.filter(r => r.menu === ket);
        menuResep.forEach(r => {
          const b = bahan.find(bb => bb.nama === r.bahan);
          if (b) totalModal += (b.harga / b.isi) * r.jumlah;
        });
        const hargaMenu = { 'americano':8000, 'creamy latte':10000, 'gula aren':10000 };
        cup = Math.floor(jml / (hargaMenu[ket] || 1));
        totalModal *= cup;
        untung = jml - totalModal;
        transaksi.push({ waktu: now.toLocaleString(), jenis, keterangan: ket, jumlah: jml, cup, modal: totalModal, untung, metode });
      } else {
        transaksi.push({ waktu: now.toLocaleString(), jenis, keterangan: ket, jumlah: jml });
      }
      renderTransaksi(); e.target.reset();
    });

  function renderBahan() {
    const tbody = document.querySelector('#tabelBahan tbody');
    tbody.innerHTML = bahan.map((b, i) => <tr><td>${b.nama}</td><td>${formatRp(b.harga)}</td><td>${b.isi}</td><td>${(b.harga/b.isi).toFixed(2)}</td><td><span class="hapus-btn" onclick="hapusBahan(${i})">Hapus</span></td></tr>).join('');
  }

  function renderResep() {
    const tbody = document.querySelector('#tabelResep tbody');
    tbody.innerHTML = resep.map((r, i) => <tr><td>${r.menu}</td><td>${r.bahan}</td><td>${r.jumlah}</td><td><span class="hapus-btn" onclick="hapusResep(${i})">Hapus</span></td></tr>).join('');
  }

  function renderTransaksi() {
    updateRingkasanKeuangan();
    const tbody = document.querySelector('#tabelTransaksi tbody');
    tbody.innerHTML = transaksi.map((t, i) => `
      <tr>
        <td>${t.waktu}</td><td>${t.jenis}</td><td>${t.keterangan}</td>
        <td>${formatRp(t.jumlah)}</td><td>${t.cup || ''}</td>
        <td>${t.modal ? formatRp(t.modal) : ''}</td><td>${t.untung ? formatRp(t.untung) : ''}</td>
        <td>${t.metode || ''}</td>
        <td><span class="hapus-btn" onclick="hapusTransaksi(${i})">Hapus</span></td>
      </tr>`).join('');
  }

  function hapusTransaksi(i) { transaksi.splice(i,1); renderTransaksi(); }
  function hapusBahan(i) { bahan.splice(i,1); renderBahan(); updateBahanResep(); }
  function hapusResep(i) { resep.splice(i,1); renderResep(); }

  document.getElementById('exportBtn').addEventListener('click', () => {
    const wb = XLSX.utils.book_new();
    const ws = XLSX.utils.json_to_sheet(transaksi);
    XLSX.utils.book_append_sheet(wb, ws, "Transaksi");
    XLSX.writeFile(wb, "Catatan_Keuangan.xlsx");
  });

  
    function formatNumberWithDot(x) {
      if (isNaN(x)) return x;
      return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
    }

    document.getElementById('formBahan').addEventListener('submit', e => {
      e.preventDefault();
      const nama = document.getElementById('namaBahan').value.trim();
      const harga = parseInt(document.getElementById('hargaBahan').value.replace(/\./g, ''));
      const isi = parseInt(document.getElementById('isiBahan').value.replace(/\./g, ''));
      const errorMsg = document.getElementById("errorBahan");
      if (!nama || isNaN(harga) || isNaN(isi)) {
        errorMsg.classList.add("show"); errorMsg.style.color = "white";
        errorMsg.textContent = "Wajib isi data dengan benar.";
        return;
      }
      errorMsg.classList.remove("show");
      bahan.push({ nama, harga, isi });
      transaksi.push({ waktu: new Date().toLocaleString(), jenis: 'pengeluaran', keterangan: Bahan: ${nama}, jumlah: harga });
      updateBahanResep(); renderBahan(); renderTransaksi(); e.target.reset();
    });

    document.getElementById('formResep').addEventListener('submit', e => {
      e.preventDefault();
      const menu = document.getElementById('menuResep').value;
      const bahanNama = document.getElementById('bahanResep').value;
      const jumlah = parseInt(document.getElementById('jumlahBahan').value.replace(/\./g, ''));
      const errorMsg = document.getElementById("errorResep");
      if (!menu || !bahanNama || isNaN(jumlah)) {
        errorMsg.classList.add("show"); errorMsg.style.color = "white";
        errorMsg.textContent = "Wajib isi data dengan benar.";
        return;
      }
      errorMsg.classList.remove("show");
      resep.push({ menu, bahan: bahanNama, jumlah });
      renderResep(); e.target.reset();
    });

    document.getElementById('formTransaksi').addEventListener('submit', e => {
      e.preventDefault();
      const jenis = document.getElementById('jenisTransaksi').value;
      const ket = document.getElementById('keteranganTransaksi').value.toLowerCase().trim();
      const jmlStr = document.getElementById('jumlahTransaksi').value.replace(/\./g, '');
      const jml = parseInt(jmlStr);
      const metode = document.getElementById('metodeBayar').value;
      const errorMsg = document.getElementById("errorTransaksi");

      if (!jenis || !ket || !jmlStr || isNaN(jml) || !metode) {
        errorMsg.classList.add("show");
        errorMsg.style.backgroundColor = "#5e3c2a";
        errorMsg.style.color = "white";
        return false;
      }
      errorMsg.classList.remove("show");
      const now = new Date();
      let cup = '', totalModal = 0, untung = 0;

      if (jenis === 'pemasukan') {
        const menuResep = resep.filter(r => r.menu === ket);
        menuResep.forEach(r => {
          const b = bahan.find(bb => bb.nama === r.bahan);
          if (b) totalModal += (b.harga / b.isi) * r.jumlah;
        });
        const hargaMenu = { 'americano':8000, 'creamy latte':10000, 'gula aren':10000 };
        cup = Math.floor(jml / (hargaMenu[ket] || 1));
        totalModal *= cup;
        untung = jml - totalModal;
        transaksi.push({ waktu: now.toLocaleString(), jenis, keterangan: ket, jumlah: jml, cup, modal: totalModal, untung, metode });
      } else {
        transaksi.push({ waktu: now.toLocaleString(), jenis, keterangan: ket, jumlah: jml });
      }
      renderTransaksi(); e.target.reset();
    });

    function formatRp(val) {
      return formatNumberWithDot(parseInt(val || 0));
    }

  
  function updateRingkasanKeuangan() {
    let pemasukan = 0, pengeluaran = 0, laba = 0;
    transaksi.forEach(t => {
      if (t.jenis === "pemasukan") {
        pemasukan += t.jumlah;
        if (t.untung) laba += t.untung;
      } else if (t.jenis === "pengeluaran") {
        pengeluaran += t.jumlah;
      }
    });
    const saldo = pemasukan - pengeluaran;

    document.getElementById("totalPemasukan").textContent = formatRp(pemasukan);
    document.getElementById("totalPengeluaran").textContent = formatRp(pengeluaran);
    document.getElementById("totalLaba").textContent = formatRp(laba);
    document.getElementById("saldoAkhir").textContent = formatRp(saldo);
  }

  window.onload = () => {
    document.getElementById("loginBox").style.display = "block";
    document.getElementById("dashboard").style.display = "none";
    updateBahanResep();
  };
</script>

</body>
</html>
