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