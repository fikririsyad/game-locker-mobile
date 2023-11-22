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

1. Membuat berkas `inventory_card.dart` di dalam direktori `lib/widgets` dan memindahkan kelas `InventoryItem` di berkas `menu.dart` ke berkas tersebut.

1. Memastikan impor pada setiap file sudah benar.

## Tugas 9: Integrasi Layanan Web Django dengan Aplikasi Flutter

### Apakah bisa kita melakukan pengambilan data JSON tanpa membuat model terlebih dahulu? Jika iya, apakah hal tersebut lebih baik daripada membuat model sebelum melakukan pengambilan data JSON?

Ya, kita dapat menyiapkan struktur data seperti dictionary atau list untuk menyimpan data JSON. Menurut saya, metode ini kurang baik jika dibandingkan dengan membuat model. Karena dengan membuat model, semua data yang diperlukan sudah jelas dan tervalidasi sesuai dengan tipe datanya dan lebih mudah untuk digunakan.

### Jelaskan fungsi dari CookieRequest dan jelaskan mengapa instance CookieRequest perlu untuk dibagikan ke semua komponen di aplikasi Flutter.

CookieRequest adalah class yang digunakan untuk mengatur otentikasi aplikasi Flutter dengan cookies. Instance-nya perlu dibagikan ke semua komponen di aplikasi Flutter agar status login dan cookies tetap konsisten.

### Jelaskan mekanisme pengambilan data dari JSON hingga dapat ditampilkan pada Flutter.

1. Membuat model yang menyesuaikan dengan data json memanfaatkan website [Quicktype](http://app.quicktype.io/).
1. Membuat sebuah fungsi `async` bernama `fetchItem()` yang di dalamnya terdapat `List` untuk menyimpan instance dari model dan sebuah loop yang dalam setiap iterasinya akan parsing data json ke model.
1. Di dalam widget `FutureBuilder`, fungsi tersebut akan disertakan pada bagian `future: ` sehingga jika dipanggil akan ditampilkan dengan ListView.builder.

### Jelaskan mekanisme autentikasi dari input data akun pada Flutter ke Django hingga selesainya proses autentikasi oleh Django dan tampilnya menu pada Flutter.

1. User memasukkan username dan password pada LoginPage.
1. Ketika klik tombol login, input tersebut akan dikirimkan ke Django untuk diautentikasi
1. Kemudian akan mengembalikkan response hasil autentikasi, jika berhasil akan menampilkan HomePage, jika tidak akan berada di page yang sama

### Sebutkan seluruh widget yang kamu pakai pada tugas ini dan jelaskan fungsinya masing-masing.

1. FutureBuilder -> Membuat widget di dalamnya menjadi asynchronous
1. ListView.builder -> Membuat daftar yang bisa di-scroll
1. TextField -> Membuat widget untuk menerima input user
1. ElevatedButton -> Membuat widget button
1. Column -> Widget untuk mengatur posisi secara vertikal
1. Navigator -> Widget untuk mengatur routing
1. CircularProgressIndicator -> Membuat widget loading

### Jelaskan bagaimana cara kamu mengimplementasikan checklist di atas secara step-by-step! (bukan hanya sekadar mengikuti tutorial).

1. Memodifikasi projek Django untuk memungkinkan integrasi dengan flutter.
1. Membuat `login.dart` untuk menampilkan halaman login:
  ```
  import 'package:game_locker_mobile/screens/menu.dart';
  import 'package:flutter/material.dart';
  import 'package:pbp_django_auth/pbp_django_auth.dart';
  import 'package:provider/provider.dart';

  void main() {
    runApp(const LoginApp());
  }

  class LoginApp extends StatelessWidget {
    const LoginApp({super.key});

    @override
    Widget build(BuildContext context) {
      return MaterialApp(
        title: 'Login',
        theme: ThemeData(
          primarySwatch: Colors.blue,
        ),
        home: const LoginPage(),
      );
    }
  }

  class LoginPage extends StatefulWidget {
    const LoginPage({super.key});

    @override
    _LoginPageState createState() => _LoginPageState();
  }

  class _LoginPageState extends State<LoginPage> {
    final TextEditingController _usernameController = TextEditingController();
    final TextEditingController _passwordController = TextEditingController();

    @override
    Widget build(BuildContext context) {
      final request = context.watch<CookieRequest>();
      return Scaffold(
        appBar: AppBar(
          title: const Text('Login'),
        ),
        body: Container(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              TextField(
                controller: _usernameController,
                decoration: const InputDecoration(
                  labelText: 'Username',
                ),
              ),
              const SizedBox(height: 12.0),
              TextField(
                controller: _passwordController,
                decoration: const InputDecoration(
                  labelText: 'Password',
                ),
                obscureText: true,
              ),
              const SizedBox(height: 24.0),
              ElevatedButton(
                onPressed: () async {
                  String username = _usernameController.text;
                  String password = _passwordController.text;

                  // Cek kredensial
                  // TODO: Ganti URL dan jangan lupa tambahkan trailing slash (/) di akhir URL!
                  // Untuk menyambungkan Android emulator dengan Django pada localhost,
                  // gunakan URL http://10.0.2.2/
                  final response = await request.login(
                      "https://fikri-risyad-tugas.pbp.cs.ui.ac.id/auth/login/", {
                    'username': username,
                    'password': password,
                  });

                  if (request.loggedIn) {
                    String message = response['message'];
                    String uname = response['username'];
                    Navigator.pushReplacement(
                      context,
                      MaterialPageRoute(builder: (context) => MyHomePage()),
                    );
                    ScaffoldMessenger.of(context)
                      ..hideCurrentSnackBar()
                      ..showSnackBar(
                          SnackBar(content: Text("$message Welcome, $uname.")));
                  } else {
                    showDialog(
                      context: context,
                      builder: (context) => AlertDialog(
                        title: const Text('Login failed'),
                        content: Text(response['message']),
                        actions: [
                          TextButton(
                            child: const Text('OK'),
                            onPressed: () {
                              Navigator.pop(context);
                            },
                          ),
                        ],
                      ),
                    );
                  }
                },
                child: const Text('Login'),
              ),
            ],
          ),
        ),
      );
    }
  }
  ```
1. Membuat berkas model `item.dart` di `lib/models` dengan memanfaatkan website [Quicktype](http://app.quicktype.io/).
  ```
  // To parse this JSON data, do
  //
  //     final item = itemFromJson(jsonString);

  import 'dart:convert';

  List<Item> itemFromJson(String str) =>
      List<Item>.from(json.decode(str).map((x) => Item.fromJson(x)));

  String itemToJson(List<Item> data) =>
      json.encode(List<dynamic>.from(data.map((x) => x.toJson())));

  class Item {
    Model model;
    int pk;
    Fields fields;

    Item({
      required this.model,
      required this.pk,
      required this.fields,
    });

    factory Item.fromJson(Map<String, dynamic> json) => Item(
          model: modelValues.map[json["model"]]!,
          pk: json["pk"],
          fields: Fields.fromJson(json["fields"]),
        );

    Map<String, dynamic> toJson() => {
          "model": modelValues.reverse[model],
          "pk": pk,
          "fields": fields.toJson(),
        };
  }

  class Fields {
    int user;
    String name;
    int amount;
    String description;
    int price;
    String genre;
    DateTime dateAdded;

    Fields({
      required this.user,
      required this.name,
      required this.amount,
      required this.description,
      required this.price,
      required this.genre,
      required this.dateAdded,
    });

    factory Fields.fromJson(Map<String, dynamic> json) => Fields(
          user: json["user"],
          name: json["name"],
          amount: json["amount"],
          description: json["description"],
          price: json["price"],
          genre: json["genre"],
          dateAdded: DateTime.parse(json["date_added"]),
        );

    Map<String, dynamic> toJson() => {
          "user": user,
          "name": name,
          "amount": amount,
          "description": description,
          "price": price,
          "genre": genre,
          "date_added":
              "${dateAdded.year.toString().padLeft(4, '0')}-${dateAdded.month.toString().padLeft(2, '0')}-${dateAdded.day.toString().padLeft(2, '0')}",
        };
  }

  enum Model { MAIN_ITEM }

  final modelValues = EnumValues({"main.item": Model.MAIN_ITEM});

  class EnumValues<T> {
    Map<String, T> map;
    late Map<T, String> reverseMap;

    EnumValues(this.map);

    Map<T, String> get reverse {
      reverseMap = map.map((k, v) => MapEntry(v, k));
      return reverseMap;
    }
  }
  ```
1. Membuat berkas `list_item.dart` di `lib/screens` yang akan menampilkan item yang sudah disimpan di Django.
  ```
  import 'package:flutter/material.dart';
  import 'package:game_locker_mobile/screens/detail_item.dart';
  import 'package:http/http.dart' as http;
  import 'dart:convert';
  import 'package:game_locker_mobile/models/item.dart';
  import 'package:game_locker_mobile/widgets/left_drawer.dart';

  class ItemPage extends StatefulWidget {
    const ItemPage({Key? key}) : super(key: key);

    @override
    _ItemPageState createState() => _ItemPageState();
  }

  class _ItemPageState extends State<ItemPage> {
    Future<List<Item>> fetchItem() async {
      // TODO: Ganti URL dan jangan lupa tambahkan trailing slash (/) di akhir URL!
      var url = Uri.parse('https://fikri-risyad-tugas.pbp.cs.ui.ac.id/json/');
      var response = await http.get(
        url,
        headers: {"Content-Type": "application/json"},
      );

      // melakukan decode response menjadi bentuk json
      var data = jsonDecode(utf8.decode(response.bodyBytes));

      // melakukan konversi data json menjadi object Item
      List<Item> list_item = [];
      for (var d in data) {
        if (d != null) {
          list_item.add(Item.fromJson(d));
        }
      }
      return list_item;
    }

    @override
    Widget build(BuildContext context) {
      return Scaffold(
          appBar: AppBar(
            title: const Text('Item'),
          ),
          drawer: const LeftDrawer(),
          body: FutureBuilder(
              future: fetchItem(),
              builder: (context, AsyncSnapshot snapshot) {
                if (snapshot.data == null) {
                  return const Center(child: CircularProgressIndicator());
                } else {
                  if (!snapshot.hasData) {
                    return const Column(
                      children: [
                        Text(
                          "There is no item data",
                          style:
                              TextStyle(color: Color(0xff59A5D8), fontSize: 20),
                        ),
                        SizedBox(height: 8),
                      ],
                    );
                  } else {
                    return ListView.builder(
                        itemCount: snapshot.data!.length,
                        itemBuilder: (_, index) => InkWell(
                            onTap: () {
                              Navigator.push(
                                  context,
                                  MaterialPageRoute(
                                      builder: (context) => DetailItemPage(
                                          item: snapshot.data![index])));
                            },
                            child: Container(
                              margin: const EdgeInsets.symmetric(
                                  horizontal: 16, vertical: 12),
                              padding: const EdgeInsets.all(20.0),
                              child: Column(
                                mainAxisAlignment: MainAxisAlignment.start,
                                crossAxisAlignment: CrossAxisAlignment.start,
                                children: [
                                  Text(
                                    "${snapshot.data![index].fields.name}",
                                    style: const TextStyle(
                                      fontSize: 18.0,
                                      fontWeight: FontWeight.bold,
                                    ),
                                  ),
                                  const SizedBox(height: 10),
                                  Text("${snapshot.data![index].fields.price}"),
                                  const SizedBox(height: 10),
                                  Text(
                                      "${snapshot.data![index].fields.description}")
                                ],
                              ),
                            )));
                  }
                }
              }));
    }
  }
  ```
1. Membuat berkas `detail_item.dart` di `lib/screens` dan diisi dengan kode berikut:
  ```
  import 'package:flutter/material.dart';
  import 'package:game_locker_mobile/models/item.dart';

  class DetailItemPage extends StatelessWidget {
    final Item item;

    DetailItemPage({required this.item});

    @override
    Widget build(BuildContext context) {
      return Scaffold(
        appBar: AppBar(
          title: Text(
            item.fields.name,
            style: TextStyle(color: Colors.white),
          ),
          backgroundColor: Colors.green,
          foregroundColor: Colors.white,
        ),
        body: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: <Widget>[
              Text(
                item.fields.name,
                style: Theme.of(context).textTheme.titleLarge,
              ),
              const SizedBox(height: 10),
              Text("Price: ${item.fields.price}"),
              const SizedBox(height: 10),
              Text("Amount: ${item.fields.amount}"),
              const SizedBox(height: 10),
              Text("Genre: ${item.fields.genre}"),
              const SizedBox(height: 10),
              Text("Description : ${item.fields.description}"),
              ElevatedButton(
                  onPressed: () {
                    Navigator.pop(context);
                  },
                  child: Text('Back'))
            ],
          ),
        ),
      );
    }
  }
  ```