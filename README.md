# Catatan untuk Pengerjaan Challenge 2

### 1. Cara menyimpan key value di local ?

Dapat menggunakan library [FlutterSecureStorage](https://pub.dev/packages/flutter_secure_storage), contoh kasus untuk menyimpan token maupun sesi login

### 2. Cara mencegah widget rebuild ketika state tidak sesuai
Menggunakan parameter buildWhen di BlocBuilder, lalu isi kondisi yang diinginkan ketika widget harus direbuild

### 3. Cara handle init dan refresh token otomatis
Perhatikan bahwa kelas QueuedInterceptor memiliki 3 method yang dapat di-override, yaitu :
- onRequest : akan terpanggil sebelum request dijalankan
- onResponse : akan terpanggil ketika menerima respon sukses (2xx, 3xx)
- onError : akan terpanggil ketika menerima respon error (4xx, 5xx, timeout, dan tidak ada koneksi)

Jadi biasanya init token akan kita handle di onRequest, sedangkan untuk refresh token akan kita handle di method onError

### 4. Equivalent live data di flutter ?
Dapat menggunakan fitur observable value dari state management [GetX](https://pub.dev/packages/get)

### 5. Bolehkah menggunakan http client selain Dio ?
Silahkan, yang terpenting memiliki fitur interceptor

### 6. Bolehkah menggunakan state management selain BloC ?
Silahkan, namun perlu memperhatikan popularitas library yang digunakan

### 7. Cara handle state login ?
Buat sebuah bloc, misal AuthBloc lalu wrap MaterialApp dengan AuthBloc agar state login menjadi global

### 8. Best practice pemanggilan event bloc untuk pertama kali ?
Menggunakan cascade operator (..), misal
``` dart
BlocProvider (
    create: (context) => AuthBloc()
                        ..add(UserLoginEvent()),
    builder: (context, state) => Scaffold()
)
```

### 9. Di halaman tambah notes tidak ada tombol save, bagaimana mekanisme save notesnya?
Catatan dapat disimpan ketika tombol 'back' ditekan

### 10. Memanggil event bloc lain lewat kelas bloc itu sendiri ?
Dengan memanggil method add

```dart
class NoteBloc extends Bloc<NoteEvent, NoteState> {

    NoteBloc(this._api) : super(NoteInitial()) {
        
        on<SaveNoteEvent>((event, emit) async {
            try {
                await _api.saveNotes();
                add(GetAllNoteEvent());
            }catch(e) {
                // handle error
            }         
        })

        on<GetAllNoteEvent>((event, emit) async {
            // terpanggil setelah SaveNoteEvent sukses
        })
    }
}

```

### 11. Cara mudah mengambil foto dari gallery dan kamera ?
Silahkan gunakan utility class [MediaService](https://wahyudotdev.gitbook.io/petunjuk-challenge-2/) dari fluttercore