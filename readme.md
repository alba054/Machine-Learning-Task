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

 2. ***Generate***  **semua kemungkinan aturan**
Sama seperti langkah pertama, pada langkah kedua ini juga kita akan menentukan batas minimum (confidence, lift, support) yang bisa dipilih dari 3 kriteria tersebut. Pada langkah ini juga semua kemungkinan yang ada mengikuti prinsip apriori dimana semua subset dari kandidate aturan yang ada akan dieliminasi jika kurang dari batas minimum yang ditentukan, misalnya min_lift(1), jika terdapat lift(X -> Y) = 0.8 < 1, maka lift(X -> Y) dan subset dari (X -> Y) akan dieliminasi.

# Algoritma FP Growth
FP Growth atau *Frequent Pattern Growth* adalah algoritma hasil dari perbaikan secara performa dari algoritma apriori. FP Growth menggunakan data struktur yang dinamakan *frequent pattern tree* atau *FP tree*.
Langkah - langkah yang digunakan pada algoritma FP Growth sebagai berikut,
1. Tahap membangkitkan *FP tree* dari semua item yang memiliki support lebih dari min_support yang telah ditentukan.
2. Tahap Pembangkitan Conditional Pattern Base Conditional Pattern Base merupakan subdata yang berisi prefix path (lintasan awal) dan suffix pattern (pola akhiran). Pembangkitan conditional pattern base didapatkan melalui FP-Tree yang telah dibangun sebelumnya.
3. Tahap Pembangkitan Conditional FP-Tree Pada tahap ini, support count dari setiap item pada setiap conditional pattern base dijumlahkan, lalu setiap item yang memiliki jumlah support count lebih besar atau sama dengan minimum support count akandibangkitkan dengan conditional FP-Tree.
4. Tahap Pencarian Frequent Itemset Apabila Conditional FP-Tree merupakan lintasan tunggal (single path), maka didapatkan frequent itemset dengan melakukan kombinasi item untuk setiap conditional FP-Tree. Jika bukan lintasan tunggal, maka dilakukan pembangkitan FPGrowth secara rekursif (proses memanggil dirinya sendiri).

# Implementasi Pada Python
Pada bagian ini saya menggunakan dataset film yang bersumber dari [kaggle.com](kaggle.com).

    # membaca file dengan format csv
    df = pd.read_csv('/content/gdrive/MyDrive/dataset/tmdb_5000_movies.csv')
    df.head()

![enter image description here](https://drive.google.com/file/d/16X1tqjDKYqYTRJHMnkSUAiZ9yMywfP23/view?usp=sharing)

    data = list(df['genres'].apply(split_genres))
    data[:5] # genre has been parsed and ready to be encode
kode diatas digunakan untuk mengekstrak genre dari format :
[{'id' : 1, 'name' : 'nama_genre0'}, {'id' : 2, 'name' : 'nama_genre1'}] menjadi format
['nama_genre0', 'nama_genre1']

    te = TransactionEncoder()
    te_data = te.fit(data).transform(data) # transform data into group of genres
    df = pd.DataFrame(te_data,columns=te.columns_)
    df.head()

kode diatas akan mentransformasi dataset dengan membuat tabel pivot dengan nama - nama genre sebagai kolomnya tabel tersebut seperti dibawah,
![enter image description here](https://drive.google.com/file/d/16X1tqjDKYqYTRJHMnkSUAiZ9yMywfP23/view?usp=sharing)

    df1 = apriori(df, min_support=0.03, use_colnames=True) # find frequent value of genres using apriori algorithm
    df1

setelah data ditransformasi maka barulah kita menggunakan library *mlxtend.frequent_patterns.apriori* untuk mencari nilai - nilai support untuk setiap kombinasi genre yang muncul pada 1 film. Data ditransformasi agar dapat menyesuaikan format yang dibutuhkan oleh library tersebut. Pada kode diatas digunakan minimum support sebesar 0.03 (3%)
![enter image description here](https://drive.google.com/file/d/16X1tqjDKYqYTRJHMnkSUAiZ9yMywfP23/view?usp=sharing)

rules = association_rules(df1, metric="lift", min_threshold=1) # create association rules

    rules = association_rules(df1, metric="lift", min_threshold=1) # create association rules
    rules.loc[rules['lift'] == rules['lift'].max()] # select rules with the highest lift
 
setelah itu kita membuat association rules yang mungkin dari nilai - nilai support yang didapatkan sebelumnya. Pada kode di atas kita menggunakan metric(ukuran) lift untuk digunakan sebagai batasan aturan yang dihasilkan sebesar 1(lift 1 berarti genre tidak saling berasosiasi).
![enter image description here](https://drive.google.com/file/d/16X1tqjDKYqYTRJHMnkSUAiZ9yMywfP23/view?usp=sharing)


