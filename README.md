[create.php](https://github.com/user-attachments/files/29633649/create.php)

<!DOCTYPE html>
<html>
<head>
    <title>FORM DATA PELANGGAN</title>
    <!-- Link ke file style.css -->
    <link rel="stylesheet" href="style.css">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/css/bootstrap.min.css" integrity="sha384-zCbKRCUGaJDkqS1kPbPd7TveP5iyJE0EjAuZQTgFLD2ylzuqKfdKlfG/eSrtxUkn" crossorigin="anonymous">
</head>
<body>
<div class="container">
    <?php
    //Include file koneksi, untuk koneksikan ke database
    include "koneksi.php";

    //Fungsi untuk mencegah inputan karakter yang tidak sesuai
    function input($data) {
        $data = trim($data);
        $data = stripslashes($data);
        $data = htmlspecialchars($data);
        return $data;
    }
    //Cek apakah ada kiriman form dari method post
    if ($_SERVER["REQUEST_METHOD"] == "POST") {

        $nama=input($_POST["nama"]);
        $email=input($_POST["email"]);
        $alamat=input($_POST["alamat"]);
        $no_tlp=input($_POST["no_tlp"]);


        //Query input menginput data kedalam tabel anggota
        $sql="insert into table_pelanggan (nama,email,alamat,no_tlp) values
		('$nama','$email','$alamat','$no_tlp')";

        //Mengeksekusi/menjalankan query diatas
        $hasil=mysqli_query($kon,$sql);

        //Kondisi apakah berhasil atau tidak dalam mengeksekusi query diatas
        if ($hasil) {
            header("Location:index.php");
        }
        else {
            echo "<div class='alert alert-danger'> Data Gagal disimpan.</div>";

        }

    }
    ?>
   <h2 style="color: white;">Input Data</h2>



    <form action="<?php echo $_SERVER["PHP_SELF"];?>" method="post">
        <div class="form-group"  style="color: white;">
            <label>Nama:</label>
            <input type="text" name="nama" class="form-control" placeholder="Masukan Nama" required />
        </div>
        <div class="form-group"style="color: white;">
            <label>Email:</label>
            <input type="text" name="email" class="form-control" placeholder="Masukan email" required />
        </div>
        <div class="form-group"style="color: white;">
            <label>Alamat:</label>
            <input type="text" name="alamat" class="form-control" placeholder="Masukan alamat" required/>
        </div>
                </p>
        <div class="form-group"style="color: white;">
            <label>No Telpon:</label>
            <input type="text" name="no_tlp" class="form-control" placeholder="Masukan No telpon" required/>
        </div>       

        <button type="submit" name="submit" class="btn btn-primary">Submit</button>
    </form>
</div>
</body>
</html>
<!DOCTYPE html>
<html>
<head>
    <!-- Link ke file style.css -->
    <link rel="stylesheet" href="style.css">

    <!-- Link ke Bootstrap -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/css/bootstrap.min.css" integrity="sha384-zCbKRCUGaJDkqS1kPbPd7TveP5iyJE0EjAuZQTgFLD2ylzuqKfdKlfG/eSrtxUkn" crossorigin="anonymous">
</head>
<title>PELANGGAN COFFEE</title>
<body>
    <nav class="navbar navbar-dark bg-dark">
        <span class="navbar-brand mb-0 h1">SELAMAT DATANG DI CAFE ARIEL</span>
    </nav>

    <div class="container">
        <br>
        <h4 class="text-center" style="color: white;">DATA PELANGGAN COFFE</h4>

        <?php
        include "koneksi.php";

        // Proses penghapusan data
        if (isset($_GET['id_pelanggan'])) {
            $id_pelanggan = htmlspecialchars($_GET["id_pelanggan"]);

            $sql = "DELETE FROM table_pelanggan WHERE id_pelanggan='$id_pelanggan'";
            $hasil = mysqli_query($kon, $sql);

            // Kondisi apakah berhasil atau tidak
            if ($hasil) {
                header("Location:index.php");
            } else {
                echo "<div class='alert alert-danger'>Data Gagal dihapus.</div>";
            }
        }
        ?>

        <table class="my-5 table table-bordered table-light">
            <thead class="table-dark">
                <tr>
                    <th>No</th>
                    <th>Nama</th>
                    <th>Email</th>
                    <th>Alamat</th>
                    <th>No Telpon</th>
                    <th>Aksi</th>
                </tr>
            </thead>
            <tbody>
                <?php
                $sql = "SELECT * FROM table_pelanggan ORDER BY id_pelanggan DESC";
                $hasil = mysqli_query($kon, $sql);
                $no = 0;

                while ($data = mysqli_fetch_array($hasil)) {
                    $no++;
                ?>
                    <tr>
                        <td><?php echo $no; ?></td>
                        <td><?php echo htmlspecialchars($data["nama"]); ?></td>
                        <td><?php echo htmlspecialchars($data["email"]); ?></td>
                        <td><?php echo htmlspecialchars($data["alamat"]); ?></td>
                        <td><?php echo htmlspecialchars($data["no_tlp"]); ?></td>

                        <td>
                            <a href="view.php?id_pelanggan=<?php echo htmlspecialchars($data['id_pelanggan']); ?>"
                            class="btn btn-warning btn-sm" role="button">lihat</a>
                            <a href="update.php?id_pelanggan=<?php echo htmlspecialchars($data['id_pelanggan']); ?>" 
                            class="btn btn-info btn-sm" role="button">Ubah</a>
                            <a href="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);
                            ?>?id_pelanggan=<?php echo htmlspecialchars($data['id_pelanggan']); ?>" 
                            class="btn btn-primary btn-sm" role="button" onclick="return confirm('Apakah Anda yakin ingin menghapus data ini?')">hapus</a>
                        </td>
                    </tr>
                <?php
                }
                ?>
            </tbody>
        </table>
        <a href="create.php" class="btn btn-primary" role="button">Tambah Data</a>
    </div>
</body>
</html>

<?php

$host="localhost";
$user="root";
$password="";
$db="projek_kopi_ariel";

$kon = mysqli_connect($host,$user,$password,$db);
if (!$kon){
        die("Koneksi Gagal:".mysqli_connect_error());
        
}
?>
body {
    background-image: url('kopi.jpg'); /* Ganti dengan path gambar Anda */
    background-position: center center; /* Menempatkan gambar di tengah */
    background-size: cover; /* Membuat gambar menutupi seluruh halaman */
    background-attachment: fixed; /* Membuat gambar tetap di tempat saat scroll (opsional) */
    margin: 0;
    padding: 0;
}

<?php
// Include koneksi ke database
include "koneksi.php";

// Fungsi untuk membersihkan input
function input($data) {
    $data = trim($data);
    $data = stripslashes($data);
    $data = htmlspecialchars($data);
    return $data;
}

// Inisialisasi variabel default
$data = [
    'id_pelanggan' => '',
    'nama' => '',
    'email' => '',
    'alamat' => '',
    'no_tlp' => '',
];

// Mendapatkan data berdasarkan ID customer
if (isset($_GET['id_pelanggan']) && is_numeric($_GET['id_pelanggan'])) {
    $id_pelanggan = input($_GET["id_pelanggan"]);
    $sql = "SELECT * FROM table_pelanggan WHERE id_pelanggan = $id_pelanggan";
    $hasil = mysqli_query($kon, $sql);

    if ($hasil && mysqli_num_rows($hasil) > 0) {
        $data = mysqli_fetch_assoc($hasil);
    } else {
        echo "<div class='alert alert-warning'>Data tidak ditemukan!</div>";
        exit; // Hentikan script jika data tidak ditemukan
    }
}

// Proses update data jika form disubmit
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $id_pelanggan = htmlspecialchars($_POST["id_pelanggan"]);
    $nama = input($_POST["nama"]);
    $email = input($_POST["email"]);
    $alamat = input($_POST["alamat"]);
    $no_tlp = input($_POST["no_tlp"]);

    // Query update
    $sql = "UPDATE table_pelanggan SET 
                nama = '$nama', 
                email = '$email', 
                alamat = '$alamat',
                no_tlp = '$no_tlp'
            WHERE id_pelanggan = $id_pelanggan";

    // Eksekusi query
    $hasil = mysqli_query($kon, $sql);

    // Cek hasil eksekusi
    if ($hasil) {
        header("Location: index.php");
    } else {
        echo "<div class='alert alert-danger'>Data gagal disimpan: " . mysqli_error($kon) . "</div>";
    }
}
?>
<!DOCTYPE html>
<html>
<head>
    <title>Update Data Pelanggan</title>
    <!-- Link ke file style.css -->
    <link rel="stylesheet" href="style.css">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/css/bootstrap.min.css" crossorigin="anonymous">
</head>
<body>
<div class="container mt-5" style="color: white;">
    <h2>Update Data</h2>
    <form action="<?php echo htmlspecialchars($_SERVER['PHP_SELF']) . '?id_pelanggan=' . urlencode($data['id_pelanggan']); ?>" method="post">
        <div class="form-group">
            <label for="nama">Nama:</label>
            <input type="text" id="nama" name="nama" class="form-control" value="<?php echo htmlspecialchars($data['nama']); ?>" required>
        </div>
        <div class="form-group">
            <label for="email">Email:</label>
            <textarea id="email" name="email" class="form-control" rows="5" required><?php echo htmlspecialchars($data['email']); ?></textarea>
        </div>
        <div class="form-group">
            <label for="alamat">Alamat:</label>
            <input type="text" id="alamat" name="alamat" class="form-control" value="<?php echo htmlspecialchars($data['alamat']); ?>" required>
        </div>
        <div class="form-group">
            <label for="no_tlp">No Hp:</label>
            <input type="text" id="no_tlp" name="no_tlp" class="form-control" value="<?php echo htmlspecialchars($data['no_tlp']); ?>" required>
        </div>
        <input type="hidden" name="id_pelanggan" value="<?php echo htmlspecialchars($data['id_pelanggan']); ?>">
        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
</div>
</body>
</html>

<?php
// Include koneksi ke database
include "koneksi.php";

// Inisialisasi data default
$data = [
    'id_pelanggan' => 'Tidak ditemukan',
    'nama' => 'Tidak ditemukan',
    'email' => 'Tidak ditemukan',
    'alamat' => 'Tidak ditemukan',
    'no_tlp' => 'Tidak ditemukan',
];

// Cek apakah id_pelanggan ada dalam URL
if (isset($_GET['id_pelanggan']) && is_numeric($_GET['id_pelanggan'])) {
    $id_pelanggan = htmlspecialchars($_GET['id_pelanggan']);

    // Query untuk mendapatkan data pelanggan berdasarkan id_pelanggan
    $sql = "SELECT * FROM table_pelanggan WHERE id_pelanggan = $id_pelanggan";
    $hasil = mysqli_query($kon, $sql);

    // Jika data ditemukan, masukkan ke dalam array $data
    if ($hasil && mysqli_num_rows($hasil) > 0) {
        $data = mysqli_fetch_assoc($hasil);
    }
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>View Data Pelanggan</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/css/bootstrap.min.css" crossorigin="anonymous">
    <style>
        body {
            background-image: url('kopi.jpg');
            background-size: cover;
            background-attachment: fixed;
            background-position: center;
            color: #fff;
        }
        .container {
            background: rgba(0, 0, 0, 0.7);
            padding: 30px;
            border-radius: 10px;
            margin-top: 50px;
        }
        .table th, .table td {
            color: #fff;
        }
        .btn-primary {
            background-color: #5a5a5a;
            border-color: #5a5a5a;
        }
        .btn-primary:hover {
            background-color: #fff;
            color: #000;
            border-color: #fff;
        }
    </style>
</head>
<body>
<div class="container">
    <h2 class="text-center mb-4">Detail Data Pelanggan</h2>
    <table class="table table-bordered">
        <tr>
            <th>ID Pelanggan</th>
            <td><?php echo htmlspecialchars($data['id_pelanggan']); ?></td>
        </tr>
        <tr>
            <th>Nama</th>
            <td><?php echo htmlspecialchars($data['nama']); ?></td>
        </tr>
        <tr>
            <th>Email</th>
            <td><?php echo htmlspecialchars($data['email']); ?></td>
        </tr>
        <tr>
            <th>Alamat</th>
            <td><?php echo htmlspecialchars($data['alamat']); ?></td>
        </tr>
        <tr>
            <th>No HP</th>
            <td><?php echo htmlspecialchars($data['no_tlp']); ?></td>
        </tr>
    </table>

    <div class="text-center">
        <a href="index.php" class="btn btn-primary">Kembali</a>
    </div>
</div>
</body>
</html>
