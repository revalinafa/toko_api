# 🛒 TokoKita --- Flutter CRUD App

Aplikasi Flutter sederhana untuk melakukan **CRUD Produk** dengan
backend API (PHP CodeIgniter 4).

## Nama : Revalina Fidiya Anugrah

## NIM : H1D023011

## Shift Praktikkum : B

## Shift KRS : D

------------------------------------------------------------------------

# 📸 Screenshot Aplikasi

(Silakan ganti path screenshot dengan file Anda sendiri.)

### Login Page
<img width="324" height="713" alt="Screenshot 2025-11-24 155758" src="https://github.com/user-attachments/assets/9c4d1b42-54a8-4303-b627-e18335938954" />



### Register Page
<img width="330" height="706" alt="Screenshot 2025-11-24 160158" src="https://github.com/user-attachments/assets/212d6beb-8058-4307-a5ee-9688df2241b8" />


### Produk Page
<img width="318" height="705" alt="Screenshot 2025-11-24 155809" src="https://github.com/user-attachments/assets/427db1b4-81df-408d-8a84-9696d06c24e9" />



### Produk Detail
<img width="332" height="710" alt="Screenshot 2025-11-24 160044" src="https://github.com/user-attachments/assets/c973378b-1aeb-4907-8350-256a2a4fe482" />



### Produk Form
<img width="328" height="707" alt="Screenshot 2025-11-24 155840" src="https://github.com/user-attachments/assets/cace9631-d07e-478f-bf49-583c9f806ce8" />


### Popup Warning
<img width="331" height="708" alt="Screenshot 2025-11-24 160127" src="https://github.com/user-attachments/assets/1ea9af7c-2c7f-40f6-96a0-fdd104da6a36" />



------------------------------------------------------------------------

# 🔄 PROSES APLIKASI --- LANGKAH PER LANGKAH

# 🔐 1. PROSES LOGIN

## a. Mengisi Form Login

![login](path_gambar_login)

**Penjelasan:** - Pengguna mengisi email & password\
- Form divalidasi (tidak boleh kosong)\
- Jika valid → memanggil POST API login

**Kode validasi form:**

``` dart
if (_formKey.currentState!.validate()) {
  _submit();
}
```

------------------------------------------------------------------------

## b. Request API untuk Login

**Kode Flutter memanggil LoginBloc:**

``` dart
LoginBloc.login(
  email: _emailTextboxController.text,
  password: _passwordTextboxController.text,
).then((value) { ... });
```

**Kode di LoginBloc:**

``` dart
var response = await Api().post(ApiUrl.login, body);
var jsonObj = json.decode(response.body);
return Login.fromJson(jsonObj);
```

------------------------------------------------------------------------

## c. Popup Sukses / Gagal

### Popup Sukses

![success_popup](path_gambar_success)

``` dart
SuccessDialog(
  description: "Login berhasil",
  okClick: () {
    Navigator.pushReplacement(
      context,
      MaterialPageRoute(builder: (_) => const ProdukPage()),
    );
  },
);
```

### Popup Gagal

![warning_popup](path_gambar_warning)

``` dart
WarningDialog(
  description: "Login gagal, email atau password salah",
);
```

**Menyimpan token setelah login:**

``` dart
UserInfo().setToken(data.token);
```

------------------------------------------------------------------------

# 🧑‍💻 2. PROSES REGISTRASI

## a. Mengisi Form Registrasi

![register](path_gambar_register)

Input terdiri dari: - Nama\
- Email\
- Password\
- Konfirmasi Password

**Contoh validasi password:**

``` dart
if (value!.length < 6) return "Password minimal 6 karakter";
```

------------------------------------------------------------------------

## b. Request Registrasi ke API

``` dart
RegistrasiBloc.registrasi(
  nama: _namaTextboxController.text,
  email: _emailTextboxController.text,
  password: _passwordTextboxController.text,
);
```

**Di RegistrasiBloc:**

``` dart
var response = await Api().post(ApiUrl.registrasi, body);
return Registrasi.fromJson(json.decode(response.body));
```

------------------------------------------------------------------------

## c. Popup Registrasi

Jika sukses:

``` dart
SuccessDialog(
  description: "Registrasi berhasil, silahkan login",
  okClick: () {
    Navigator.pop(context);
    Navigator.pop(context);
  },
);
```

Jika gagal:

``` dart
WarningDialog(description: "Registrasi gagal");
```

------------------------------------------------------------------------

# 📦 3. MENAMPILKAN LIST PRODUK

## a. Halaman List Produk

![produk_list](path_gambar_produk_list)

**Kode menampilkan FutureBuilder:**

``` dart
FutureBuilder<List<Produk>>(
  future: ProdukBloc.getProduks(),
  builder: (context, snapshot) { ... }
);
```

**Mengambil data produk dari API:**

``` dart
var response = await Api().get(ApiUrl.listProduk);
var jsonObj = json.decode(response.body);

List<dynamic> listProduk = jsonObj['data'];
```

**Kode ListTile:**

``` dart
ListTile(
  title: Text(produk.namaProduk!),
  subtitle: Text("Rp ${produk.hargaProduk}"),
  onTap: () => navigateToDetail(produk),
);
```

------------------------------------------------------------------------

# ➕ 4. PROSES MENAMBAH PRODUK

## a. Form Tambah Produk

![produk_form](path_gambar_produk_form)

User mengisi: - Kode\
- Nama\
- Harga

------------------------------------------------------------------------

## b. Submit Data ke API

``` dart
ProdukBloc.addProduk(
  produk: Produk(
    kodeProduk: produk.kodeProduk,
    namaProduk: produk.namaProduk,
    hargaProduk: produk.hargaProduk,
  ),
);
```

**Di BLoC:**

``` dart
var response = await Api().post(ApiUrl.createProduk, body);
var jsonObj = json.decode(response.body);
return jsonObj['status'];
```

------------------------------------------------------------------------

## c. Popup Sukses

``` dart
SuccessDialog(
  description: "Produk berhasil ditambah",
  okClick: () => Navigator.pop(context),
);
```

------------------------------------------------------------------------

# ✏️ 5. PROSES MENGEDIT PRODUK

## a. Membuka Halaman Detail

![produk_detail](path_gambar_produk_detail)

Ada tombol: - EDIT\
- DELETE

------------------------------------------------------------------------

## b. Form Edit Produk

![produk_form_edit](path_gambar_produk_form_edit)

------------------------------------------------------------------------

## c. Request Update Produk

``` dart
ProdukBloc.updateProduk(
  produk: produkYangDiupdate,
);
```

**Di BLoC:**

``` dart
var response = await Api().put(ApiUrl.updateProduk(id), jsonEncode(body));
```

------------------------------------------------------------------------

## d. Popup Sukses

``` dart
SuccessDialog(description: "Produk berhasil diperbarui");
```

------------------------------------------------------------------------

# 🗑️ 6. PROSES MENGHAPUS PRODUK

## a. Popup Konfirmasi Delete

![confirm_delete](path_gambar_confirm_delete)

``` dart
AlertDialog(
  title: Text("Yakin ingin menghapus data ini?")
);
```

------------------------------------------------------------------------

## b. Request DELETE

``` dart
ProdukBloc.deleteProduk(id: produk.id);
```

**Di BLoC:**

``` dart
var response = await Api().delete(ApiUrl.deleteProduk(id));
```

------------------------------------------------------------------------

## c. Popup Sukses

``` dart
SuccessDialog(description: "Produk berhasil dihapus");
```

------------------------------------------------------------------------

# 📁 Struktur Folder

    lib/
     ├── bloc/
     │    ├── login_bloc.dart
     │    ├── logout_bloc.dart
     │    ├── produk_bloc.dart
     │    └── registrasi_bloc.dart
     ├── helpers/
     │    ├── api.dart
     │    ├── api_url.dart
     │    ├── app_exception.dart
     │    └── user_info.dart
     ├── model/
     │    ├── login.dart
     │    ├── produk.dart
     │    └── registrasi.dart
     ├── ui/
     │    ├── login_page.dart
     │    ├── registrasi_page.dart
     │    ├── produk_page.dart
     │    ├── produk_form.dart
     │    └── produk_detail.dart
     ├── widget/
     │    ├── success_dialog.dart
     │    └── warning_dialog.dart
     └── main.dart

------------------------------------------------------------------------

# 🔗 Backend API (CI4)

-   POST /login\
-   POST /registrasi\
-   GET /produk\
-   POST /produk\
-   PUT /produk/{id}\
-   DELETE /produk/{id}

------------------------------------------------------------------------

# 🚀 Cara Menjalankan

## Flutter

    flutter pub get
    flutter run

## Backend

    php spark serve

------------------------------------------------------------------------

# ✨ Author

**Revalina Fidiya Anugrah --- H1D023011**
