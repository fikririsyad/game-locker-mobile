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

## Tugas 8: Flutter Navigation, Layouts, Forms, and Input Elements

### Jelaskan perbedaan antara `Navigator.push()` dan `Navigator.pushReplacement()`, disertai dengan contoh mengenai penggunaan kedua metode tersebut yang tepat!

| |`Navigator.push()`|`Navigator.pushReplacement()`|
| - | ---------------- | ----------------------------|
| **Fungsi** | Menambahkan *route* ke dalam *stack route* | Menghapus *route* paling atas di *stack route* dan menambahkan *route* yang baru ke dalam *stack route* |
| **Contoh** | Pada halaman main, menggunakan `Navigator.push()` ke halaman profile agar pengguna bisa kembali ke halaman main. | Pada halaman login, menggunakan `Navigator.pushReplacement()` ke halaman main agar pengguna tidak bisa kembali ke halaman login. |

### Jelaskan masing-masing *layout* widget pada Flutter dan konteks penggunaannya masing-masing!
1. `Container` -> kustomisasi tampilan *child*-nya.
1. `Row` -> mengatur posisi *children* secara horizontal.
1. `Column` -> mengatur posisi *children* secara vertikal.
1. `Stack` -> mengatur posisi *children* seperti tumpukan.
1. `GridView` -> mengatur posisi *children* di dalam 2D Grid.
1. `ListView` -> menampilkan *scrollable list* dari *children*-nya di satu sumbu, horizontal atau vertikal.
1. `Expanded` -> mendistribusikan sisa ruang di antara *children*-nya.

### Sebutkan apa saja elemen input pada form yang kamu pakai pada tugas kali ini dan jelaskan mengapa kamu menggunakan elemen input tersebut!
Pada tugas ini, elemen input yang dipakai pada form adalah `TextField` karena widget ini dapat digunakan untuk mengisi semua field yang ada di-form.

### Bagaimana penerapan *clean architecture* pada aplikasi Flutter?
1. *Presentation layer* - Lapisan yang mengurus UI, seperti widget, dan bagaimana pengguna dapat berinteraksi.
1. *Domain layer* - Lapisan yang mengurus logika bisnis bagaimana elemen dalam aplikasi berperilaku.
1. *Data layer* - Lapisan yang mengurus bagaimana aplikasi mengambil dan menyimpan data.

### Jelaskan bagaimana cara kamu mengimplementasikan *checklist* di atas secara *step-by-step*! (bukan hanya sekadar mengikuti tutorial)

1. Membuat direktori `screens` di dalam `lib` dan memindahkan `menu.dart` ke direktori tersebut 

1. Membuat direktori `widgets` di dalam `lib` dan membuat berkas `left_drawer.dart` yang diisi dengan kode:
    ```
    import 'package:flutter/material.dart';
    import 'package:game_locker_mobile/screens/menu.dart';

    class LeftDrawer extends StatelessWidget {
      const LeftDrawer({super.key});

      @override
      Widget build(BuildContext context) {
        return Drawer(
          child: ListView(
            children: [
              const DrawerHeader(
                decoration: BoxDecoration(
                  color: Colors.indigo,
                ),
                child: Column(
                  children: [
                    Text(
                      'Game Locker',
                      textAlign: TextAlign.center,
                      style: TextStyle(
                        fontSize: 30,
                        fontWeight: FontWeight.bold,
                        color: Colors.white,
                      ),
                    ),
                    Padding(padding: EdgeInsets.all(10)),
                    Text("Track all your games here!",
                        textAlign: TextAlign.center,
                        style: TextStyle(
                          fontSize: 15,
                          fontWeight: FontWeight.normal,
                          color: Colors.white,
                        )),
                  ],
                ),
              ),
              ListTile(
                leading: const Icon(Icons.home_outlined),
                title: const Text('Main Page'),
                // Bagian redirection ke MyHomePage
                onTap: () {
                  Navigator.pushReplacement(
                      context,
                      MaterialPageRoute(
                        builder: (context) => MyHomePage(),
                      ));
                },
              ),
              // TODO: AKAN DITAMBAHKAN ListTile ke ShopFormPage()
            ],
          ),
        );
      }
    }
    ```

1. Pada berkas `menu.dart`, meng-import `left_drawer.dart` dan menambahkan `LeftDrawer` di bawah `appBar`:
    ```
    import 'package:game_locker_mobile/widgets/left_drawer.dart';
    ...
    return Scaffold(
      appBar: AppBar(
        ...
      ),
      drawer: const LeftDrawer(),
    )

    ```

1. Membuat berkas `shoplist_form.dart` di direktori `lib/screens` dan diisi dengan kode:
    ```
    import 'package:flutter/material.dart';
    import 'package:game_locker_mobile/widgets/left_drawer.dart';

    class ShopFormPage extends StatefulWidget {
      const ShopFormPage({super.key});

      @override
      State<ShopFormPage> createState() => _ShopFormPageState();
    }

    class _ShopFormPageState extends State<ShopFormPage> {
      final _formKey = GlobalKey<FormState>();
      String _name = "";
      int _price = 0;
      int _amount = 0;
      String _genre = "";
      String _description = "";

      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: const Center(
              child: Text(
                'New Item Form',
              ),
            ),
            backgroundColor: Colors.indigo,
            foregroundColor: Colors.white,
          ),
          drawer: const LeftDrawer(),
          body: Form(
            key: _formKey,
            child: SingleChildScrollView(
                child:
                    Column(crossAxisAlignment: CrossAxisAlignment.start, children: [
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: TextFormField(
                  decoration: InputDecoration(
                    hintText: "Item name",
                    labelText: "Item name",
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(5.0),
                    ),
                  ),
                  onChanged: (String? value) {
                    setState(() {
                      _name = value!;
                    });
                  },
                  validator: (String? value) {
                    if (value == null || value.isEmpty) {
                      return "Nama cannot be empty!";
                    }
                    return null;
                  },
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: TextFormField(
                  decoration: InputDecoration(
                    hintText: "Price",
                    labelText: "Price",
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(5.0),
                    ),
                  ),
                  onChanged: (String? value) {
                    setState(() {
                      _price = int.parse(value!);
                    });
                  },
                  validator: (String? value) {
                    if (value == null || value.isEmpty) {
                      return "Price cannot be empty!";
                    }
                    if (int.tryParse(value) == null) {
                      return "Price has to be integer!";
                    }
                    return null;
                  },
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: TextFormField(
                  decoration: InputDecoration(
                    hintText: "Amount",
                    labelText: "Amount",
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(5.0),
                    ),
                  ),
                  onChanged: (String? value) {
                    setState(() {
                      _amount = int.parse(value!);
                    });
                  },
                  validator: (String? value) {
                    if (value == null || value.isEmpty) {
                      return "Amount cannot be empty!";
                    }
                    if (int.tryParse(value) == null) {
                      return "Amount has to be integer!";
                    }
                    return null;
                  },
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: TextFormField(
                  decoration: InputDecoration(
                    hintText: "Genre",
                    labelText: "Genre",
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(5.0),
                    ),
                  ),
                  onChanged: (String? value) {
                    setState(() {
                      _genre = value!;
                    });
                  },
                  validator: (String? value) {
                    if (value == null || value.isEmpty) {
                      return "Genre cannot be empty!";
                    }
                    return null;
                  },
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: TextFormField(
                  decoration: InputDecoration(
                    hintText: "Description",
                    labelText: "Description",
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(5.0),
                    ),
                  ),
                  onChanged: (String? value) {
                    setState(() {
                      _description = value!;
                    });
                  },
                  validator: (String? value) {
                    if (value == null || value.isEmpty) {
                      return "Description cannot be empty!";
                    }
                    return null;
                  },
                ),
              ),
              Align(
                alignment: Alignment.bottomCenter,
                child: Padding(
                  padding: const EdgeInsets.all(8.0),
                  child: ElevatedButton(
                    style: ButtonStyle(
                      backgroundColor: MaterialStateProperty.all(Colors.indigo),
                    ),
                    onPressed: () {
                      if (_formKey.currentState!.validate()) {
                        showDialog(
                          context: context,
                          builder: (context) {
                            return AlertDialog(
                              title: const Text('Produk berhasil tersimpan'),
                              content: SingleChildScrollView(
                                child: Column(
                                  crossAxisAlignment: CrossAxisAlignment.start,
                                  children: [
                                    Text('Nama: $_name'),
                                    Text('Price: $_price'),
                                    Text('Amount: $_amount'),
                                    Text('Genre: $_genre'),
                                    Text('Description: $_description')
                                  ],
                                ),
                              ),
                              actions: [
                                TextButton(
                                  child: const Text('OK'),
                                  onPressed: () {
                                    Navigator.pop(context);
                                  },
                                ),
                              ],
                            );
                          },
                        );
                        _formKey.currentState!.reset();
                      }
                    },
                    child: const Text(
                      "Save",
                      style: TextStyle(color: Colors.white),
                    ),
                  ),
                ),
              ),
            ])),
          ),
        );
      }
    }
    ```

1. Pada berkas `left_drawer.dart`, meng-import `shoplist_form.dart` dan menambahkan kode di bagian `TODO`:
    ```
    import 'package:game_locker_mobile/screens/shoplist_form.dart';
    ...
    ListTile(
      leading: const Icon(Icons.add_shopping_cart),
      title: const Text('Add Item'),
      // Bagian redirection ke ShopFormPage
      onTap: () {
        Navigator.pushReplacement(
            context,
            MaterialPageRoute(
              builder: (context) => const ShopFormPage(),
            ));
      },
    ),
    ```

1. Pada berkas `menu.dart`, meng-import `shoplist_form.dart` dan menambahkan kode di bagian bawah `onTap` di `InkWell` kelas `InventoryCard`:
    ```
    // Area responsive terhadap sentuhan
    onTap: () {
      // Memunculkan SnackBar ketika diklik
      ScaffoldMessenger.of(context)
        ..hideCurrentSnackBar()
        ..showSnackBar(SnackBar(
            content: Text("You have clicked ${item.name} button!")));
      // Navigate ke route yang sesuai (tergantung jenis tombol)
      if (item.name == "Add Item") {
        Navigator.push(
            context,
            MaterialPageRoute(
              builder: (context) => const ShopFormPage(),
            ));
      }
    },
    ```

1. Membuat berkas `shop_card.dart` di dalam direktori `lib/widgets` dan memindahkan kelas `InventoryItem` di berkas `menu.dart` ke berkas tersebut.

1. Memastikan impor pada setiap file sudah benar.