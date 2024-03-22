# REFLECTION

## Milestone 1

Apa yang ada pada method handle_connection?


method ini akan menangani koneksi dari klien. Method ini akan membaca request HTTP dari klien dan mencetaknya ke konsol.


``` let buf_reader = BufReader::new(&mut stream);```


Membuat BufReader baru dari TcpStream untuk membaca data dari klien


```let http_request: Vec<_> = buf_reader .lines() .map(|result| result.unwrap()).take_while(|line| !line.is_empty()) .collect();  ```


Membaca request HTTP dari klien baris per baris dan mengumpulkannya ke dalam Vec<> bernama http_request

```println!("Request: {:#?}", http_request);```

Mencetak request HTTP ke konsol dengan format debug.

## Milestone 2

Line tambahan pada handle_request()

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

![Commit 2 screen capture](https://github.com/gnh374/advprog-module6/assets/121223135/b2bd7089-8574-4489-abce-761f75205571)

