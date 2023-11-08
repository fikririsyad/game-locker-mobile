# Game Locker 🎮

## Tugas 7: Elemen Dasar Flutter
### Apa perbedaan utama antara *stateless* dan *stateful widget* dalam konteks pengembangan aplikasi Flutter?

*Stateless widget* adalah *widget* yang tidak dapat berubah. Contohnya adalah `Icon`, `IconButton`, dan `Text`. *Stateful widget* adalah *widget* yang dinamis atau dapat berubah ketika merespons suatu *event* dari interaksi pengguna. Contohnya `Checkbox`, `Radio`, dan `TextField`.


### Sebutkan seluruh *widget* yang kamu gunakan untuk menyelesaikan tugas ini dan jelaskan fungsinya masing-masing.

1. `AppBar` - komponen yang menampilkan konten pada bagian atas layar
1. `Scaffold` - Membuat tata letak dasar aplikasi
1. `Text` - Menampilkan text
1. `Icon` - Menampilkan icon
1. `SingleChildScrollView` - Membuat konten bisa di-*scroll* jika melebihi ukuran layar
1. `Padding` - Memberikan jarak antara konten dengan *border*
1. `Column` - Membuat tata letak vertikal
1. `SnackBar` - Menampilkan pesan di bagian bawah layar
1. `InkWell` - Membuat *area* yang akan merespons sentuhan
1. `Material` - Memberikan latar belakang berwarna pada tampilan widget lain.

### Jelaskan bagaimana cara kamu mengimplementasikan checklist di atas secara step-by-step (bukan hanya sekadar mengikuti tutorial)

1. Membuka terminal dan membuat proyek Flutter baru, lalu masuk ke direktori proyek tersebut:
    ```
    flutter create game_locker_mobile
    cd game_locker_mobile
    ```

1. Membuat repositori baru di GitHub dengan nama `game-locker-mobile`, lalu melakukan `git init` di direktori lokal. Kemudian melakukan `add`-`commit`-`push` ke repositori di GitHub.

1. Membuat berkas baru bernama `menu.dart` di dalam direktori `lib` dan masukkan kode berikut:
    ```
    import 'package:flutter/material.dart';
    ```

1. Membuka berkas `lib/main.dart` dan memindahkan seluruh `class MyHomePage` dan `class _MyHomePageState` ke berkas `menu.dart` di bawah kode *import* yang sebelumnya dimasukkan.

1. Menambahkan kode import berikut di atas berkas `main.dart`:
    ```
    import 'package:game_locker_mobile/menu.dart';
    ```
    dan mengganti `MyHomePage(title: 'Flutter Demo Home Page')` menjadi `MyHomePage()`

1. Mengubah *widget* halaman menjadi *stateless* dengan menghapus semua kode dari `final String title;` sampai akhir baris berkas. Kemudian, mengubah `class MyHomePage` menjadi seperti berikut:
    ```
    class MyHomePage extends StatelessWidget {
        MyHomePage({Key? key}) : super(key: key);

        @override
        Widget build(BuildContext context) {
            return Scaffold(
                ...
            );
        }
    }
    ```

1. Membuat *class* untuk merepresentasikan *button* seperti berikut ini:
    ```
    class InventoryItem {
        final String name;
        final IconData icon;
        final Color color;

        InventoryItem(this.name, this.icon, this.color);
    }
    ```

1.  Menambahkan kode ini di dalam `class MyHomePage` di bawah `MyHomePage({Key? key}) : super(key: key);`:
    ```
    final List<InventoryItem> items = [
        InventoryItem("Lihat Item", Icons.checklist, Colors.red),
        InventoryItem("Tambah Item", Icons.add_shopping_cart, Colors.green),
        InventoryItem("Logout", Icons.logout, Colors.blue),
    ];
    ```

1. Membuat *class* barun untuk menampilkan `InventoryItem` dalam bentuk *card*.
    ```
    class InventoryCard extends StatelessWidget {
      final InventoryItem item;
    
      const InventoryCard(this.item, {super.key}); // Constructor

      @override
      Widget build(BuildContext context) {
        return Material(
          color: item.color,
          child: InkWell(
            // Area responsive terhadap sentuhan
            onTap: () {
              // Memunculkan SnackBar ketika diklik
              ScaffoldMessenger.of(context)
                ..hideCurrentSnackBar()
                ..showSnackBar(SnackBar(
                    content: Text("Kamu telah menekan tombol ${item.name}!")));
            },
            child: Container(
              // Container untuk menyimpan Icon dan Text
              padding: const EdgeInsets.all(8),
              child: Center(
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    Icon(
                      item.icon,
                      color: Colors.white,
                      size: 30.0,
                    ),
                    const Padding(padding: EdgeInsets.all(3)),
                    Text(
                      item.name,
                      textAlign: TextAlign.center,
                      style: const TextStyle(color: Colors.white),
                    ),
                  ],
                ),
              ),
            ),
          ),
        );
      }
    }
    ```
1. Mengisi `return` pada `Widget build` di dengan kode ini:
    ```
    return Scaffold(
      appBar: AppBar(
        title: const Text(
          'Game Locker',
        ),
      ),
      body: SingleChildScrollView(
        // Widget wrapper yang dapat discroll
        child: Padding(
          padding: const EdgeInsets.all(10.0), // Set padding dari halaman
          child: Column(
            // Widget untuk menampilkan children secara vertikal
            children: <Widget>[
              const Padding(
                padding: EdgeInsets.only(top: 10.0, bottom: 10.0),
                // Widget Text untuk menampilkan tulisan dengan alignment center dan style yang sesuai
                child: Text(
                  'Game Locker', // Text yang menandakan toko
                  textAlign: TextAlign.center,
                  style: TextStyle(
                    fontSize: 30,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
              // Grid layout
              GridView.count(
                // Container pada card kita.
                primary: true,
                padding: const EdgeInsets.all(20),
                crossAxisSpacing: 10,
                mainAxisSpacing: 10,
                crossAxisCount: 3,
                shrinkWrap: true,
                children: items.map((InventoryItem item) {
                  // Iterasi untuk setiap item
                  return InventoryCard(item);
                }).toList(),
              ),
            ],
          ),
        ),
      ),
    );
    ```