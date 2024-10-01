Aplikasi CRUD Data Pesanan Dengan Menggunakan Java Swing dan PostgreSQL

Deskripsi Tugas Pertemuan Kelima
Pada tugas Pemrograman Berbasis Objek (PBO) pertemuan 5, yaitu mengimplementasikan Aplikasi CRUD Data Menu cafe Menggunakan Java Swing dan PostgreSQL,
dengan database PESANAN. Tugas ini untuk mengembangkan sebuah Aplikasi Pengelolaan Data Pesanan menggunakan bahasa pemrograman Java yang terhubung dengan database PostgreSQL.
Aplikasi ini memiliki antarmuka berbasis GUI (Graphical User Interface) yang dibangun menggunakan Java Swing dan mendukung operasi CRUD (Create, Read, Update, Delete)
untuk data petugas.

Koneksi Database
Dalam tahap koneksi database pastikan sudah menyiapkan database dan membuat tabel petugas dengan struktur yang sesuai, seperti kolom id_pesanan, Menu, harga, dan qty.
Aplikasi ini terhubung ke database menggunakan JDBC, dengan library PostgreSQL yang diperlukan untuk mengelola operasi CRUD (Create, Read, Update, Delete) pada data petugas.
Kemudian, perlu memastikan bahwa koneksi ke database berhasil, lalu melanjutkan untuk mengimplementasikan fitur-fitur CRUD dengan antarmuka berbasis Java Swing.

Fungsi btnInsertActionPerformed
Fungsi btnInsert ini berfungsi untuk memasukan data baru ke tabel Pesanan di database. Data yang dimasukan terdiri dari id_pesanan, menu, harga, qty.
Ketika data sudah berhasil di insert maka koneksi akan di-commit dan ditutup. Berikut tampilan source code yang saya buat : 
private void jBInsertActionPerformed(java.awt.event.ActionEvent evt) {
        // TODO add your handling code here:

        if (txtId.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtMenu.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtHarga.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtQty.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else {
            try {
                Class.forName(driver);
                conn = DriverManager.getConnection(koneksi, user, password);
                conn.setAutoCommit(false);

                String sql = "INSERT INTO Pesanan(id_pesanan, Menu_Makanan, Harga, Qty) VALUES(?,?,?,?)";
                pstmt = conn.prepareStatement(sql);

                String id_pesanan, Menu_makanan, Harga, Qty;
                id_pesanan = txtId.getText();
                Menu_makanan = txtMenu.getText();
                Harga = txtHarga.getText();
                Qty = txtQty.getText();
                pstmt.setInt(1, Integer.parseInt(id_pesanan));
                pstmt.setString(2, Menu_makanan);
                pstmt.setString(3, Harga);
                pstmt.setString(4, Qty);

                pstmt.executeUpdate();
                conn.commit();

                pstmt.close();
                conn.close();

                JOptionPane.showMessageDialog(null, "Succes to insert");
            } catch (ClassNotFoundException | SQLException ex) {
                JOptionPane.showMessageDialog(null, "Failed to insert " + ex.getMessage());
            }
        }

        showTable();
    }

Fungsi dari kode diatas yaitu menangani aksi ketika tombol “insert” (atau “tambah”) ditekan oleh pengguna. Tujuannya yaitu untuk mengambil data dari field input, memvalidasi,
dan menyimpannya ke dalam database postgreSQL. Pertama yaitu memeriksa apakah ada field yang kosong ketika field tidak terisi maka akan muncul perintah
yang menunjukan bahwa data harus terisi. dimana , setiap field diperiksa satu persatu. dan , jika ada yang kosong maka proses akan berhenti disini dan tidak akan menyimpan data
ke database. Jika, field sudah terisi, kode akan mencoba untuk membuat driver JDBC yang diperlukan untuk berkomunikasi dengan database.menggunakan DriverManager.
getConnection(), kode akan membuat koneksi ke database dengan parameter koneksi, user, dan password. Selanjutnya, Kode menyiapkan pernyataan SQL insert 
untuk menambahkan data ke tabel pesanan. Nilai dari masing-masing field input diambil dan disisipkan dalam variabel : id_pesanan, Menu_makanan, Harga, dan Qty.
Lalu, data akan dimasukan ke dalam pernyataan SQL menggunakan pstmt.setInt() untuk id_pesanan dan pstmt.setString() untuk yang lainnya. Dengan memanggil pstmt.executeUpdate(),
kode menjalankan pernyataan SQL yang telah disiapkan. Setelah itu, conn.commit() digunakan untuk menyimpan perubahan ke database. Jika tidak ada kesalahan, data akan ditambahkan ke tabel Pesanan. Jika semua proses berhasil, maka akan muncul pemberitahuan bahwa data berhasil dimasukan. Namun, jika terjadi kesalahan saat data diproses entah itu kesalahan koneksi atau kesalahan SQL. maka, akan muncul pemberitahuan yang menunjukan pesan kesalahan. Metode showTable() dipanggil karena bertujuan untuk memperbarui tampilan tabel data yang ditampilkan di antarmuka pengguna yang menampilkan data perubahan.

Fungsi btnUpdateActionPerformed
private void jBUpdateActionPerformed(java.awt.event.ActionEvent evt) {                                         
        // TODO add your handling code here:
        String id_pesanan, Menu_makanan, Harga, Qty;
        if (txtId.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtMenu.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtHarga.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtQty.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else {
            try {
                Class.forName(driver);
                String sql = "UPDATE Pesanan SET Menu_makanan = ?, Harga = ?, Qty = ? WHERE id_pesanan = ?";
                conn = DriverManager.getConnection(koneksi, user, password);
                pstmt = conn.prepareStatement(sql);

                id_pesanan = txtId.getText();
                Menu_makanan = txtMenu.getText();
                Harga = txtHarga.getText();
                Qty = txtQty.getText();

                pstmt.setString(1, Menu_makanan);
                pstmt.setString(2, Harga);
                pstmt.setString(3, Qty);
                pstmt.setString(4, id_pesanan);

                int rowsAffected = pstmt.executeUpdate();
                if (rowsAffected > 0) {
                    JOptionPane.showMessageDialog(null, "Success to update");
                    bersih();
                } else {
                    JOptionPane.showMessageDialog(null, "Failed to Update");
                }
            } catch (ClassNotFoundException | SQLException ex) {
                JOptionPane.showMessageDialog(null, "Error: " + ex.getMessage());
            } catch (NumberFormatException ex) {
                JOptionPane.showMessageDialog(null, "NIM harus berupa angka");
            } finally {
                try {
                    if (pstmt != null) {
                        pstmt.close();
                    }
                    if (conn != null) {
                        conn.close();
                    }
                } catch (SQLException e) {
                    JOptionPane.showMessageDialog(null, "Error closing resources: " + e.getMessage());
                }
            }
        }
        showTable();

    }                                        


	Fungsi metode ini yaitu untuk menangani aksi tombol “Update” (atau “ Perbarui”) saat ditekan. Yang bertujuan untuk mengambil data dari field input,memvalidasi,
 dan memperbarui data yang sesuai di database.Pertama yaitu mendeklarasikan untuk menyimpan nilai dari field id_pesanan, Menu_makanan, Harga, dan Qty. lalu,
 kode akan memeriksa field input tersebut terisi. Jika salah satu field ada yang kosong maka akan muncul pemberitahuan menggunakan JOptionPane, yang memberitahu bahwa data harus terisi. Jika, field sudah terisi makan akan lanjut ke proses berikutnya. Setelah validasi, kode akan mencoba untuk membuat driver JDBC yang diperbarui koneksi ke database dibuka menggunakan DriverManager.getConnection() dengan parameter yang diperlukan (koneksi, user, password). Kode menyiapkan SQL UPDATE untuk memperbarui data di dalam data pesanan.
 Dengan memanggil pstmt.executeUpdate(), pernyataan SQL dieksekusi untuk melakukan pembaruan. Jumlah baris yang terpengaruh oleh pernyataan SQL disimpan dalam variabel rowsAffected. Jika rowsAffected lebih besar dari 0, artinya pembaruan berhasil, dan dialog keberhasilan ditampilkan. Jika ada kesalahan konversi (misalnya, saat memparsing angka), 
 dialog dengan pesan kesalahan akan ditampilkan. Metode showTable() dipanggil karena bertujuan untuk memperbarui tampilan tabel data yang ditampilkan di antarmuka pengguna yang menampilkan data perubahan.

Fungsi btnDeleteActionPerformed

private void jBDeleteActionPerformed(java.awt.event.ActionEvent evt) {                                         
        String id_pesanan;
        id_pesanan = txtId.getText();

        try {
            Class.forName(driver);
            conn = DriverManager.getConnection(koneksi, user, password);

            int jawab = JOptionPane.showConfirmDialog(null, "Silakan Konfirmasi?");
            switch (jawab) {
                case JOptionPane.YES_OPTION:
                    String deleteSql = "DELETE FROM Pesanan WHERE id_pesanan = ?";
                    pstmt = conn.prepareStatement(deleteSql);
                    pstmt.setString(1, id_pesanan);
                    pstmt.executeUpdate();
                    pstmt.close();
                    conn.close();

                    break;
                case JOptionPane.NO_OPTION:
                    JOptionPane.showMessageDialog(this, "Kamu menjawab tidak");
                    break;
            }
        } catch (ClassNotFoundException | SQLException ex) {
            JOptionPane.showConfirmDialog(null, "Cek Lagi!!!", "Error", JOptionPane.ERROR_MESSAGE);
            ex.printStackTrace();
        } finally {
            if (pstmt != null) {
                try {
                    pstmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        bersih();
        showTable();


    }                                        

Fungsi metode ini yaitu untuk menangani aksi tombol “Delete” (atau “ Hapus”) saat ditekan. Tujuannya yaitu untuk menghapus data pesanan yang sesuai dari tabel pesanan
di database jika telah mendapatkan konfirmasi dari pengguna. Pertama yaitu pada variabel id_pesanan dideklarasikan dan diisi dengan nilai dari field input txtId, 
yang merupakan dari id_pesanan yang akan dihapus. Di dalam blok try, kode pertama-tama mencoba untuk memuat driver JDBC. Kemudian, koneksi ke database dibuka menggunakan DriverManager.getConnection() dengan parameter yang diperlukan (koneksi, user, password). Lalu, kode akan meminta konfirmasi dari pengguna sebelum menghapus data.
Jika sudah ada komfirmasi dari pengguna maka akan ada pesan pemberitahuan “silakan konfirmasi?”. JOptionPane.showConfirmDialog() mengembalikan nilai yang menunjukkan 
pilihan pengguna (Ya atau Tidak). Jika memilih “ya” maka kode akan menyiapkan pernyataan SQL DELETE untuk menghapus data dari tabel pesanan berdasarkan id_pesanan. 
pstmt.setString(1, id_pesanan) digunakan untuk menetapkan ID pesanan yang akan dihapus. pstmt.executeUpdate() menjalankan pernyataan SQL untuk menghapus data. 
Dan jika memilih “Tidak” (JOptionPane.NO_OPTION), maka akan muncul dialog yang memberi tahu bahwa mereka memilih untuk tidak menghapus data. 
Jika ada kesalahan dari kelas atau dari SQL. maka, akan muncul pemberitahuan kesalahan dan stack trace dari pengecualian dicetak ke konsol. 
Metode showTable() dipanggil karena bertujuan untuk memperbarui tampilan tabel data yang ditampilkan di antarmuka pengguna yang menampilkan data perubahan.
Fungsi showTable
Berfungsi showTable yaitu  untuk menampilkan data dari tabel Pesanan di dalam aplikasi Java, biasanya dalam bentuk tabel GUI. 
Berikut tampilan source code yang saya buat : 
public void showTable() {
        try {
            // TODO code application logi
            Class.forName(driver);
            String sql = "SELECT * FROM Pesanan order by Id_pesanan";
            conn = DriverManager.getConnection(koneksi, user, password);
            stmt = conn.createStatement();

            while (!conn.isClosed()) {

                ResultSet rs = stmt.executeQuery(sql);
                this.TabelPesanan.setModel(UtilsDb.resultSetToTableModel(rs));
                while (rs.next()) {
                    System.out.println(String.valueOf(rs.getObject(1)) + " " + String.valueOf(rs.getObject(2)) + " " + String.valueOf(rs.getObject(3) + " " + String.valueOf(rs.getObject(4))));
                }
                conn.close();//move this outside the while loop
            }

            stmt.close();

        } catch (ClassNotFoundException | SQLException ex) {

        }
    }

Fungsi dari metode di atas yaitu bertanggung jawab untuk menampilkan data dari tabel pesanan dalam bentuk
GUI (Graphical User Interface).

Fungsi TabelPesananAncestorAdded
Fungsi TabelPesananAncestorAdded yaitu bagian dari event handling dalam aplikasi GUI java (kemungkinan menggunakan swing) 
yang dijalankan ketika komponen tabelpesanan ditambahkan ke dalam hierarki tampilan. Berikut tampilan source code yang saya buat : 

private void TabelPesananAncestorAdded(javax.swing.event.AncestorEvent evt) {                                           
        // TODO add your handling code here:
        int row = TabelPesanan.getSelectedRow();
        if (row >= 0) {
            txtId.setText(TabelPesanan.getValueAt(row, 0).toString());
            txtMenu.setText(TabelPesanan.getValueAt(row, 1).toString());
            txtHarga.setText(TabelPesanan.getValueAt(row, 2).toString());
            txtQty.setText(TabelPesanan.getValueAt(row, 3).toString());
        }
    }                                          

Fungsi UtilsDb Class

	Kelas DbUtils ini digunakan untuk mengkonversi data dari ResultSet menjadi model tabel (TableModel) yang dapat ditampilkan pada komponen tabel di Java Swing.
 Metode resultSetToTableModel mengambil metadata kolom dari ResultSet, mendapatkan nama-nama kolom, dan mengisi baris-baris tabel dengan data dari database,
 lalu menampilkannya dalam bentuk DefaultTableModel. Berikut tampilan source code yang saya buat : 

public class UtilsDb {
    public static TableModel resultSetToTableModel(ResultSet rs) {
        try {
            ResultSetMetaData metaData = rs.getMetaData();
            int numberOfColumns = metaData.getColumnCount();
            Vector columnNames = new Vector();

            // Get the column names
            for (int column = 0; column < numberOfColumns; column++) {
                columnNames.addElement(metaData.getColumnLabel(column + 1));
            }

            // Get all rows.
            Vector rows = new Vector();

            while (rs.next()) {
                Vector newRow = new Vector();

                for (int i = 1; i <= numberOfColumns; i++) {
                    newRow.addElement(rs.getObject(i));
                }

                rows.addElement(newRow);
            }

            return new DefaultTableModel(rows, columnNames);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}


Fungsi UtilsDb yaitu untuk mengonversi objek resultset (yang berisi hasil query dari database) menjadi objek TableModel
(yang dapat digunakan untuk menampilkan data dalam tabel GUI). 











