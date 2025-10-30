# Tugas 2
Nama: Adelia Isra Ekaputri

NIM: 10241004

## Kode

```php
<?php
mysqli_report(MYSQLI_REPORT_STRICT);
try {
    $mysqli = new mysqli("localhost", "root", "");

    $query = "CREATE DATABASE IF NOT EXISTS latihanPL";
    $mysqli->query($query);

    $mysqli->select_db("latihanPL");

    $query = "DROP TABLE IF EXISTS mahasiswa";
    $mysqli->query($query);

    $query = "CREATE TABLE mahasiswa (
        id INT PRIMARY KEY AUTO_INCREMENT,
        nama VARCHAR(50),
        nim VARCHAR(50),
        alamat VARCHAR(50),
        tanggal_update TIMESTAMP
    )";
    $mysqli->query($query);

    $sekarang = new DateTime('now', new DateTimeZone('Asia/Jakarta'));
    $timestamp = $sekarang->format("Y-m-d H:i:s");

    $query = "INSERT INTO mahasiswa
    (nama, nim, alamat, tanggal_update) VALUES
        ('Adelia Isra Ekaputri', '10241004', 'KM 15', '$timestamp'),
        ('Devina Dian Saputri', '10241022', 'BDS', '$timestamp'),
        ('Laudya Aprilia Khoirum', '10241038', 'KM 10', '$timestamp'),
        ('Nazwa Nur Hafizhah', '10241056', 'Rapak', '$timestamp'),
        ('Zalfa Putri Sopyandi', '10241076', 'Gunung Belah', '$timestamp')";
    $mysqli->query($query);

    $result = $mysqli->query("SELECT nama, nim, alamat, tanggal_update FROM mahasiswa");

    echo "<table border='1' cellpadding='5' cellspacing='0'>
            <tr>
                <th>Nama</th>
                <th>NIM</th>
                <th>Alamat</th>
                <th>Tanggal Update</th>
            </tr>";
    while ($row = $result->fetch_assoc()) {
        echo "<tr>
                <td>{$row['nama']}</td>
                <td>{$row['nim']}</td>
                <td>{$row['alamat']}</td>
                <td>{$row['tanggal_update']}</td>
            </tr>";
    }
    echo "</table>";

} catch (Exception $e) {
    echo "Koneksi / Query bermasalah: ".$e->getMessage()." (".$e->getCode().")";
} finally {
    if (isset($mysqli)) {
        $mysqli->close();
    }
}
?>
```

## Penjelasan Kode
1. Koneksi ke MySQL
   ```php
   $mysqli = new mysqli("localhost", "root", "");
   ```
   Kode ini membuat koneksi ke server MySQL lokal dengan user `root` tanpa password. Jika koneksi berhasil, kita bisa menjalankan query ke database.

2. Pembuatan dan Pemilihan Database
   ```php
   $query = "CREATE DATABASE IF NOT EXISTS latihanPL";$mysqli->query($query);
   $mysqli->select_db("latihanPL");
   ```
   - Jika database latihanPL belum ada, dibuat secara otomatis.
   - Kemudian koneksi diarahkan untuk menggunakan database `latihanPL`.

3. Pembuatan Tabel
   ```php
   $query = "DROP TABLE IF EXISTS mahasiswa";$mysqli->query($query);
   $query = "CREATE TABLE mahasiswa (
       id INT PRIMARY KEY AUTO_INCREMENT,
       nama VARCHAR(50),
       nim VARCHAR(50),
       alamat VARCHAR(50),
       tanggal_update TIMESTAMP
   )";
   $mysqli->query($query);
    ```
    - Tabel lama `mahasiswa` dihapus jika ada.
    - Tabel baru dibuat dengan kolom `id`, `nama`, `nim`, `alamat`, dan `tanggal_update`.

4. Mengisi Data ke Tabel
   ```php
   $sekarang = new DateTime('now', new DateTimeZone('Asia/Jakarta'));
   $timestamp = $sekarang->format("Y-m-d H:i:s");

    $query = "INSERT INTO mahasiswa
    (nama, nim, alamat, tanggal_update) VALUES
        ('Adelia Isra Ekaputri', '10241004', 'KM 15', '$timestamp'),
        ...
        ('Zalfa Putri Sopyandi', '10241076', 'Gunung Belah', '$timestamp')";
    $mysqli->query($query);
    ```
    - Membuat nilai waktu sekarang (`timestamp`)
    - Memasukkan 5 baris data mahasiswa ke dalam tabel `mahasiswa`.

5. Mengambil Data dari Tabel
   ```php
   $result = $mysqli->query("SELECT nama, nim, alamat, tanggal_update FROM mahasiswa");
   ```
   - Query SELECT mengambil semua baris dari tabel `mahasiswa`.
   - Hasil query disimpan ke variabel `$result`.

6. Menampilkan Data ke Web dalam Bentuk Tabel HTML
   ```php
   echo "<table border='1' cellpadding='5' cellspacing='0'>
        <tr>
            <th>Nama</th>
            <th>NIM</th>
            <th>Alamat</th>
            <th>Tanggal Update</th>
        </tr>";
    while ($row = $result->fetch_assoc()) {
        echo "<tr>
              <td>{$row['nama']}</td>
              <td>{$row['nim']}</td>
              <td>{$row['alamat']}</td>
              <td>{$row['tanggal_update']}</td>
        </tr>";
    }
    echo "</table>";
    ```
    - Pertama dibuat header tabel HTML (`<th>`).
    - Lalu dengan `while ($row = $result->fetch_assoc())`, setiap baris hasil query diambil satu per satu dalam bentuk array asosiatif (`$row['nama']`, `$row['nim']`, dll).
    - Data tersebut ditaruh ke dalam baris HTML (`<tr>` … `</tr>`).
    - Hasil akhirnya berupa tabel HTML yang bisa ditampilkan di browser.

## Hasil
<img src="Hasil Tabel.png">

## Alur Sederhana
1. PHP terhubung ke MySQL → buat DB & tabel.
2. Data mahasiswa dimasukkan ke tabel.
3. PHP mengambil data dengan query `SELECT`.
4. Data di-looping, lalu dicetak dalam format HTML `<table>`.
5. Browser membaca HTML tersebut, sehingga pengguna melihat tabel rapi berisi data mahasiswa.