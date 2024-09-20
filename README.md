# Aplikasi CRUD Data Petugas Menggunakan Java Swing dan PostgreSQL

## Deskripsi Tugas
Pada tugas Pemrograman Berbasis Objek (PBO) pertemuan 5 ini, yaitu mengimplementasikan Aplikasi CRUD Data Petugas Menggunakan Java Swing dan PostgreSQL, dengan database TOKOBUKU2. Pada tugas ini, diminta untuk mengembangkan sebuah Aplikasi Pengelolaan Data Petugas menggunakan bahasa pemrograman Java yang terhubung dengan database PostgreSQL. Aplikasi ini memiliki antarmuka berbasis GUI (Graphical User Interface) yang dibangun menggunakan Java Swing dan mendukung operasi CRUD (Create, Read, Update, Delete) untuk data petugas. 

## Koneksi Database
Dalam tahap koneksi database pastikan sudah menyiapkan database dan membuat tabel petugas dengan struktur yang sesuai, seperti kolom id_petugas, nama, gender, dan umur. Aplikasi ini terhubung ke database menggunakan JDBC, dengan library PostgreSQL yang diperlukan untuk mengelola operasi CRUD (Create, Read, Update, Delete) pada data petugas. Kemudian, perlu memastikan bahwa koneksi ke database berhasil, lalu melanjutkan untuk mengimplementasikan fitur-fitur CRUD dengan antarmuka berbasis Java Swing.

## Pembuatan Antarmuka GUI Data Petugas Menggunakan Java Swing
Tampilan form *Data Petugas* yang saya buat menggunakan *Java Swing* sebagai antarmuka grafis (GUI) terdapat beberapa komponen seperti *JTextField* untuk input data (ID Petugas, Nama Petugas, dan Umur), *JRadioButton* untuk memilih gender (Laki-laki atau Perempuan), serta *JButton* untuk melakukan operasi CRUD (Insert, Update, Delete, Clear). Bagian bawah form dilengkapi dengan *JTable* yang berfungsi untuk menampilkan data petugas yang tersimpan di database. Setiap komponen ini diatur dalam *JPanel* dan dirender di dalam *JFrame* untuk membangun antarmuka aplikasi yang interaktif dan mudah digunakan.

## Fungsi btnInsertActionPerformed
Fungsi ini berfungsi untuk memasukkan data baru ke tabel petugas di database. Data yang dimasukkan terdiri dari ID petugas, nama, gender, dan umur. Setelah data berhasil di-insert, koneksi akan di-commit dan ditutup. Berikut adalah source code yang saya gunakan :
<pre>
  private void btnInsertActionPerformed(java.awt.event.ActionEvent evt) {                                          
        String id_petugas, nama, gender, umur;
        String hehe = tfNama.getText();
        String hoho = tfIDpetugas.getText();
        String huhu = tfUmur.getText();

        if (hoho.trim().isEmpty() && hehe.trim().isEmpty() && huhu.trim().isEmpty()) {
            JOptionPane.showMessageDialog(this, "isi dulu yaa! ");
        } else if (hoho.trim().isEmpty()) {
            JOptionPane.showMessageDialog(this, "isi ID petugas anda dulu! ");
        } else if (hehe.trim().isEmpty()) {
            JOptionPane.showMessageDialog(this, "isi nama anda dulu!");
        } else if (huhu.trim().isEmpty()) {
            JOptionPane.showMessageDialog(this, "isi umur anda dulu!");
        } else {
            try {
                Class.forName(driver);
                conn = DriverManager.getConnection(koneksi, user, password);
                conn.setAutoCommit(false);

                String sql = "INSERT INTO petugas VALUES(?,?,?,?)";
                pstmt = conn.prepareStatement(sql);

                id_petugas = tfIDpetugas.getText();
                nama = tfNama.getText();
                if (laki.isSelected()) {
                    gender = "Laki-laki";
                } else {
                    gender = "Perempuan";
                }
                umur = tfUmur.getText();

                pstmt.setLong(1, Long.parseLong(id_petugas));
                pstmt.setString(2, nama);
                pstmt.setString(3, gender);
                pstmt.setLong(4, Long.parseLong(umur));

                pstmt.executeUpdate();

                JOptionPane.showMessageDialog(null, "data berhasil ditambahkan");
                conn.commit(); // Commit transaksi setelah sejumlah operasi-insert berhasil
                pstmt.close();
                conn.close();

            } catch (ClassNotFoundException | SQLException ex) {
                JOptionPane.showMessageDialog(null, "terjadi kesalahan saat pengisian");
            }
        }
        bersih();
        tampil();
    }                                         
</pre>
Kode tersebut adalah fungsi yang dijalankan ketika tombol "Insert" ditekan untuk memasukkan data petugas ke dalam database PostgreSQL. Pertama, input dari pengguna untuk ID petugas, nama, gender, dan umur divalidasi untuk memastikan tidak ada yang kosong. Jika ada input yang kosong, akan muncul pesan peringatan menggunakan `JOptionPane`. Setelah validasi berhasil, koneksi ke database PostgreSQL dibuka menggunakan `DriverManager`, dan query SQL disiapkan dengan `PreparedStatement` untuk memasukkan data ke tabel **petugas**. Nilai ID petugas, nama, gender (berdasarkan pilihan radio button), dan umur diambil dari input pengguna dan diatur ke dalam query SQL. Jika eksekusi perintah SQL berhasil, data disimpan dan ditampilkan pesan konfirmasi kepada pengguna. Koneksi ke database kemudian ditutup, input di-clear menggunakan method `bersih()`, dan tampilan data diperbarui dengan method `tampil()`. Jika terjadi kesalahan selama proses, error ditangani oleh blok `catch` yang menampilkan pesan kesalahan kepada pengguna.

## Fungsi btnUpdateActionPerformed
Fungsi ini digunakan untuk memperbarui data petugas berdasarkan ID petugas yang diambil dari tfIDpetugas. Data baru yang bisa diubah adalah nama, gender, dan umur. Berikut adalah source code yang saya gunakan :
<pre>
 private void btnUpdateActionPerformed(java.awt.event.ActionEvent evt) {                                          
        String idPetugas, namaPetugasBaru, genderBaru, umurBaru;
        String hehe = tfNama.getText();
        String hoho = tfIDpetugas.getText();
        String huhu = tfUmur.getText();

        if (hoho.trim().isEmpty() && hehe.trim().isEmpty() && huhu.trim().isEmpty()) {
            JOptionPane.showMessageDialog(this, "masukkan dulu data petugas yang ingin diupdate! ");
        } else if (hoho.trim().isEmpty()) {
            JOptionPane.showMessageDialog(this, "isi ID petugas anda dulu! ");
        } else if (hehe.trim().isEmpty()) {
            JOptionPane.showMessageDialog(this, "isi nama anda dulu yang ingin diupdate!");
        } else if (huhu.trim().isEmpty()) {
            JOptionPane.showMessageDialog(this, "isi umur anda dulu yang ingin diupdate!");
        } else {
            try {
                Class.forName(driver);
                String sql = "UPDATE petugas SET nama = ?, gender = ?, umur = ? WHERE id_petugas = ?";
                conn = DriverManager.getConnection(koneksi, user, password);
                pstmt = conn.prepareStatement(sql);

                idPetugas = tfIDpetugas.getText();
                namaPetugasBaru = tfNama.getText();
                if (laki.isSelected()) {
                    genderBaru = "Laki-laki";
                } else {
                    genderBaru = "Perempuan";
                }
                umurBaru = tfUmur.getText();

                pstmt.setString(1, namaPetugasBaru);
                pstmt.setString(2, genderBaru);
                pstmt.setLong(3, Long.parseLong(umurBaru));
                pstmt.setLong(4, Long.parseLong(idPetugas));
                
                int rowsAffected = pstmt.executeUpdate();
                if (rowsAffected > 0) {
                    JOptionPane.showMessageDialog(null, "data berhasil diupdate!");
                    pstmt.close();
                    conn.close();
                    bersih();
                } else {
                    JOptionPane.showMessageDialog(null, "data tidak ditemukan");
                }
            } catch (ClassNotFoundException | SQLException ex) {
            }
            tampil();
        }
    }                        
</pre>
Kode tersebut adalah fungsi yang dijalankan ketika tombol "Update" ditekan untuk memperbarui data petugas yang ada di dalam database PostgreSQL. Pertama, input ID petugas, nama baru, gender baru, dan umur baru divalidasi untuk memastikan tidak ada yang kosong. Jika ada input yang kosong, pesan peringatan ditampilkan menggunakan `JOptionPane`. Jika semua input terisi, koneksi ke database PostgreSQL dibuka dengan `DriverManager`, dan query SQL disiapkan menggunakan `PreparedStatement` untuk memperbarui data petugas berdasarkan ID petugas yang diberikan. Nilai baru dari nama, gender (dari pilihan radio button), dan umur diambil dari input pengguna dan dimasukkan ke dalam query SQL. Jika eksekusi query berhasil dan ada data yang diperbarui, pengguna diberi pesan konfirmasi bahwa data berhasil diupdate. Jika tidak ada data yang ditemukan dengan ID yang diberikan, pesan "data tidak ditemukan" akan muncul. Setelah proses selesai, koneksi ke database ditutup, input di-clear menggunakan method `bersih()`, dan tampilan data diperbarui dengan method `tampil()`.

## Fungsi btnDeleteActionPerformed
Fungsi ini digunakan untuk menghapus data petugas berdasarkan ID Petugas yang dimasukkan di tfIDpetugas. Setelah berhasil, koneksi akan di-commit dan ditutup. Berikut adalah source code yang saya gunakan :
<pre>
   private void btnDeleteActionPerformed(java.awt.event.ActionEvent evt) {                                          
        String idPetugas;
        String hoho = tfIDpetugas.getText();

        if (hoho.trim().isEmpty()) {
            JOptionPane.showMessageDialog(this, "masukkan dulu data petugas yang ingin dihapus!");
        } else {
            try {
                Class.forName(driver);
                conn = DriverManager.getConnection(koneksi, user, password);

                int jawab = JOptionPane.showConfirmDialog(null, "Silakan Konfirmasi?");
                switch (jawab) {
                    case JOptionPane.YES_OPTION:
                        String sql = "DELETE FROM petugas WHERE id_petugas = ?";
                        pstmt = conn.prepareStatement(sql);

                        idPetugas = tfIDpetugas.getText();
                        pstmt.setLong(1, Long.parseLong(idPetugas));
                        pstmt.executeUpdate();

                        pstmt.close();
                        conn.close();
                        bersih();
                        break;

                    case JOptionPane.NO_OPTION:
                        JOptionPane.showMessageDialog(this, "anda menjawab tidak");
                        break;
                }

            } catch (ClassNotFoundException | SQLException ex) {
                System.out.println("terjadi kesalahan saat melakukan operasi insert dalam loop.");
                ex.printStackTrace();
                try {
                    if (conn != null) {
                        conn.rollback();
                    }
                } catch (SQLException e) {
                    System.out.println("Gagal melakukan rollback transaksi.");
                    e.printStackTrace();
                }
            }
            tampil();
        }
    }                                              
</pre>
Kode ini merupakan fungsi yang dijalankan ketika tombol "Delete" ditekan untuk menghapus data petugas dari database PostgreSQL. Pertama, ID petugas divalidasi untuk memastikan tidak kosong. Jika kosong, pesan peringatan muncul menggunakan JOptionPane. Jika terisi, koneksi ke database dibuka menggunakan DriverManager, lalu diminta konfirmasi penghapusan melalui dialog JOptionPane. Jika memilih "Yes", query SQL untuk menghapus data petugas berdasarkan ID disiapkan dan dijalankan dengan PreparedStatement. Jika berhasil, data petugas dihapus, dan input form di-reset dengan method bersih(). Jika pengguna memilih "No", pesan bahwa penghapusan dibatalkan ditampilkan. Setelah proses selesai, koneksi ke database ditutup dan tampilan data diperbarui dengan method tampil().

## Fungsi tampil()
Fungsi ini digunakan untuk menampilkan data petugas yang ada di tabel petugas ke dalam tabel GUI (tbTabel). Berikut adalah source code yang saya gunakan :
<pre>
   public void tampil() {
        try {
            Class.forName(driver);
            String sql = "SELECT * FROM petugas ";
            conn = DriverManager.getConnection(koneksi, user, password);
            stmt = conn.createStatement();

            while (!conn.isClosed()) {

                ResultSet rs = stmt.executeQuery(sql);
                this.tbTabel.setModel(DbUtils.resultSetToTableModel(rs));
                while (rs.next()) {
                    System.out.println(String.valueOf(rs.getObject(1)) + " " + String.valueOf(rs.getObject(2)) + " " + String.valueOf(rs.getObject(3)) + " " + String.valueOf(rs.getObject(4)));
                }
                conn.close();
            }

            stmt.close();

        } catch (ClassNotFoundException | SQLException ex) {
             java.util.logging.Logger.getLogger(DataPetugas.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }
    }
</pre>
Fungsi `tampil()` digunakan untuk menampilkan data petugas dari database PostgreSQL ke tabel yang ada di GUI. Koneksi ke database dibuka dengan `DriverManager`, kemudian query SQL `"SELECT * FROM petugas"` dieksekusi menggunakan `Statement`. Hasil dari query tersebut disimpan dalam `ResultSet` dan kemudian ditampilkan di tabel GUI menggunakan `DbUtils.resultSetToTableModel(rs)`. Selain itu, setiap baris data yang diambil dari `ResultSet` juga dicetak ke konsol untuk keperluan debugging. Setelah semua data ditampilkan, koneksi ke database dan statement ditutup untuk mencegah kebocoran koneksi.

## Fungsi btnClearActionPerformed
Fungsi Clear pada aplikasi ini digunakan untuk mengosongkan semua field input yang ada pada form setelah data dimasukkan, di-update, atau dihapus. Fungsi ini memastikan bahwa field kembali kosong sehingga siap digunakan untuk memasukkan data baru tanpa perlu menghapus isian secara manual. Berikut adalah source code yang saya gunakan :
<pre>
  private void btnClearActionPerformed(java.awt.event.ActionEvent evt) {
    bersih();
</pre>
Dengan contoh implementasi method untuk fungsi Clear (bernama bersih()) berikut :
<pre>
  public void bersih() {
        tfIDpetugas.setText("");
        tfNama.setText("");
        buttonGroup1.clearSelection();
        tfUmur.setText("");
    }
</pre>
Dengan memanggil fungsi bersih(), semua field input seperti ID Petugas, Nama Petugas, pilihan gender (buttonGroup1), dan Umur akan dikosongkan secara otomatis. Ini membantu menjaga agar form tetap bersih dan siap untuk input data baru tanpa harus menghapus isian sebelumnya secara manual. Implementasi dari method bersih() ini bekerja dengan mengosongkan text field menggunakan setText("") dan membersihkan pilihan gender dengan clearSelection() pada buttonGroup1.

## Fungsi tbTabelMouseClicked
Fungsi tbTabelMouseClicked pada kode yang diberikan menangani kejadian ketika tabel tbTabel diklik. Fungsi ini bertujuan untuk memudahkan pengguna dalam mengedit data petugas. Saat pengguna mengklik baris di tabel, data dari baris tersebut akan diisi ke dalam field input di antarmuka pengguna, sehingga pengguna dapat dengan mudah melihat dan mengedit informasi petugas yang dipilih. Berikut adalah source code yang saya gunakan :
<pre>
  private void tbTabelMouseClicked(java.awt.event.MouseEvent evt) {
        int row = tbTabel.getSelectedRow(); // Mendapatkan Baris yang Dipilih
        tfIDpetugas.setText(tbTabel.getValueAt(row, 0).toString());
        tfNama.setText(tbTabel.getValueAt(row, 1).toString()); // Mengisi Field Input
        String gender = tbTabel.getValueAt(row, 2).toString(); // Menentukan Gender
        if (gender.equalsIgnoreCase("Laki-laki")) {
            laki.setSelected(true);
        } else if (gender.equalsIgnoreCase("Perempuan")) {
            pr.setSelected(true);
        } // Menentukan Gender
        tfUmur.setText(tbTabel.getValueAt(row, 3).toString()); // Mengisi Umur
  }
</pre>
Fungsi ini digunakan untuk mengisi field input secara otomatis saat baris pada tabel dipilih oleh pengguna. Ketika baris di tabel diklik, data dari tabel akan diambil sesuai baris dan kolom yang dipilih, lalu diisi ke dalam text field seperti ID Petugas, Nama, Gender, dan Umur.

## DbUtils Class
Kelas DbUtils ini digunakan untuk mengkonversi data dari ResultSet menjadi model tabel (TableModel) yang dapat ditampilkan pada komponen tabel di Java Swing. Metode resultSetToTableModel mengambil metadata kolom dari ResultSet, mendapatkan nama-nama kolom, dan mengisi baris-baris tabel dengan data dari database, lalu menampilkannya dalam bentuk DefaultTableModel.

Semoga penjelasan diatas dapat membantu memahami materi dalam tugas pertemuan 5 mata kuliah Pemrograman Berorientasi Objek ini.
