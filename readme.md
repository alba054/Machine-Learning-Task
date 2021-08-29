# Association Rules
*Association rules* adalah sebuah metode yang berbasis *machine learning* untuk menemukan hubungan yang menarik antar variabel di sebuah dataset yang besar. *Association rules* digunakan untuk mengidentifikasi sebuah aturan yang kuat dengan menggunakan ukuran/batasan yang di berikan. Metode ini banyak digunakan untuk *market basket analysis* dimana sebuah toko akan menggunakan *insight* dari aturan - aturan yang diberikan untuk menaikkan jumlah keuntungan mereka misalnya, menaruh teh dan gula di satu rak yang sama sehingga banyak pembeli yang datang akan lebih cepat menemukan kedua barang tersebut secara bersamaan.
Beberapa ukuran yang digunakan :
 1. **Support**
Support mengukur seberapa sering suatu produk(item) muncul di dalam transaksi(record) .
 2. **Confidence**
Confidence mengukur kemungkinan munculnya (x -> y) y ketika x ada. Menarik sebuah kesimpulan dari confidence akan membatasi inferensi yang akan dilakukan karena confidence tidak memberikan sebuah *insight* yang cukup.
 3. **Lift**
Lift adalah ukuran kemungkinan munculnya (x -> y) y ketika x ada dengan mempertimbangkan seberapa sering y muncul di transaksi(record). Jika y terlalu banyak(melampaui jauh) x maka kemungkinan besar Lift akan lebih kecil dari 1 dimana untuk Lift < 1, menandakan item x dan y tidak saling berasosiasi.

# Algoritma Apriori
Apriori adalah sebuah algoritma untuk menghitung *frequent_pattern*(seberapa sering) sebuah item muncul di dataset yang memiliki relasi yang besar. Dimulai dari mengidentifikasi seberapa sering item - item individual muncul di dataset dan mengambil semua kombinasi item - item pada dataset. Algoritma apriori dapat digunakan untuk menentukan *association rules* yang menunjukkan tren item dari sebuah dataset. Algoritma ini banyak digunakan pada kasus seperti *market basket analysis*.
Terdapat 2 langkah umum pada algoritma apriori :
1. ***Generate*** **semua himpunan bagian dari item - item di dataset**.
Sebelum men-*generate* subset pada item - item di dataset berikan batas minimum pada support yang akan diambil. Sehingga hasil dari semua subset yang akan dihasilkan bisa lebih kecil dan bermakna. Menggunakan prinsip apriori dapat membuat pencarian(*generating*) subset lebih efisien karena menurut prinsip apriori, semua subset dari item yang lebih kecil dari min-support yang telah ditentukan akan lebih kecil juga sehingga kita dapat menghilangkan semua subset dari item - item tersebut secara formal dapat ditulis, support(X) >= support({X, ...}.
![ilustrasi prinsip algoritma apriori](https://miro.medium.com/max/500/1*3C8TKEtyZHpYbesLwLeCxQ.gif)

