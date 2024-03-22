# REFLECTION

## Milestone 1

1. Apa yang ada pada method handle_connection?


method ini akan menangani koneksi dari klien. Method ini akan membaca request HTTP dari klien dan mencetaknya ke konsol.


``` let buf_reader = BufReader::new(&mut stream);```


Membuat BufReader baru dari TcpStream untuk membaca data dari klien


```let http_request: Vec<_> = buf_reader .lines() .map(|result| result.unwrap()).take_while(|line| !line.is_empty()) .collect();  ```


Membaca request HTTP dari klien baris per baris dan mengumpulkannya ke dalam Vec<> bernama http_request

```println!("Request: {:#?}", http_request);```

Mencetak request HTTP ke konsol dengan format debug.

## Milestone 2

2. Line tambahan pada handle_request()

``` let status_line = "HTTP/1.1 200 OK ```


Menyiapkan status line untuk response HTTP yang akan dikirim ke klien. Status line ini menyatakan bahwa respons sukses dengan kode status 200 (OK).

```let contents = std::fs::read_to_string("hello.html").unwrap(); ```


Membaca isi dari file hello.html ke dalam string contents

```let length = contents.len();```

Menghitung panjang (jumlah byte) dari isi file hello.html

```let response = format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");```


Membuat response HTTP yang akan dikirim ke klien. Response terdiri dari status line, header Content-Length yang menyatakan panjang konten, dan isi konten dari file hello.html


``` stream.write_all(response.as_bytes()).unwrap(); ```

 Mengirim response ke klien dalam bentuk byte array setelah diubah dari string menggunakan as_bytes(). Jika terjadi kesalahan dalam proses pengiriman, program akan panic dan keluar dengan menggunakan unwrap()

![Commit 2 screen capture](https://github.com/gnh374/advprog-module6/assets/121223135/6ae0a253-c64e-497b-a73d-a883c680d405)


## Milestone 3

3. Cara membedakan response dan mengapa dibutuhkan?

Response dibedakan dengan melakukan validasi terhadap baris pertama dari HTTP request

```let request_line = buf_reader.lines().next().unwrap().unwrap(); ```


Kita hanya ingin mengambil baris pertama dari HTTP Request sehingga kita gunakan method ```next``` kemudian ```unwrap``` yang pertama akan mengurus objek ```Option``` dan  ```unwrap``` yang kedua akan mengurus objek ```Result``` 

```
 if request_line == "GET / HTTP/1.1" {
        let status_line = "HTTP/1.1 200 OK";
        let contents = std::fs::read_to_string("hello.html").unwrap();
        let length = contents.len();

        let response = format!(
            "{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}"
        );

        stream.write_all(response.as_bytes()).unwrap();
    } else {
        let status_line = "HTTP/1.1 404 NOT FOUND";
        let contents = std::fs::read_to_string("404.html").unwrap();
        let length = contents.len();

        let response = format!(
            "{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}"
        );

        stream.write_all(response.as_bytes()).unwrap();
    }
```

Selanjutnya kita akan melakukan pengecekan isi dari request_line. Jika sesuai dengan GET request ke path makan akan dimunculkan halaman hello.html. Jika selain itu, maka akan dimunculkan halaman 404.html


Disini saya juga membuat halaman 404.html
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Hello!</title>
  </head>
  <body>
    <h1>Oops!</h1>
    <p>Sorry, I don't know what you're asking for.</p>
    <p>Rust is running from Gabriella's machine.</p>
  </body>
</html>

 ```

Selanjutnya saya melakukan refactor untuk mengurangi duplikasi
```
fn handle_connection(mut stream: TcpStream) { 
    let buf_reader = BufReader::new(&mut stream);
    let request_line = buf_reader.lines().next().unwrap().unwrap();

  
    let (status_line, filename) = if request_line == "GET / HTTP/1.1" {
        ("HTTP/1.1 200 OK", "hello.html")
    } else {
        ("HTTP/1.1 404 NOT FOUND", "404.html")
    };

    let contents = fs::read_to_string(filename).unwrap();
    let length = contents.len();

    let response =
        format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");

    stream.write_all(response.as_bytes()).unwrap();
}

```

Perlu dilakukan validating request untuk memastikan bahwa request yang diterima oleh server sesuai dengan apa yang diharapkan dan tidak mengandung masalah keamanan atau kesalahan lainnya. 

Response yang diberikan perlu dibedakan untuk membedakan konten response dan kode status HTTP yang dikirimkan ke klien. Hal ini penting agar response yang diterima sesuai dengan apa yang direquest oleh user. Selain itu, pembedaan response juga dapat membantu meningkatkan efisiensi dan menjaga keamanan.

![Screenshot 2024-03-22 114423](https://github.com/gnh374/advprog-module6/assets/121223135/4bd1aadf-a475-4073-982e-4a523b208104)







