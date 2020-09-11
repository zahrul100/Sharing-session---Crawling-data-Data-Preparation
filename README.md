
# Sharing Session Crawling data dan Data Preparation

## Requirement
1. Menginstall [ Anaconda3 ](https://docs.anaconda.com/anaconda/install/windows/) / Jupyter notebook

2. [ File PDF Untuk Crawling  ](https://drive.google.com/file/d/1gcnAAHPgjZqSc20yw22m9U7Hejr2mpN0/view?usp=sharing)

3. Menginstal Library Berikut Pada Anaconda Prompt
   
 * **Pandas** 
 ```
 conda install -c anaconda pandas
 ```
  * **Numpy** 
 ```
 conda install -c anaconda numpy 
 ```
 * **Tabula** 
``` 
conda install -c conda-forge tabula-py 
```

## Daftar Isi
  - [1. Crawling From PDF](#1-Crawling-From-PDF)
  
  - [2. Cleaning and Formatting Data](#2-Cleaning-and-Formatting-Data)
  
## 1. Crawling From PDF
Pada Sharing Session kali ini kita akan membahas tentan Crawling Data dengan sumber table dari pdf 
kedalam Format Csv
* **Input**
![component](gambar/inputspdf.jpeg)
* **Output**

![component](gambar/outputpdf.jpeg)
### Tahapan Eksekusi
### Library
   ```python3 
   import pandas
   import tabula
   import glob
   import os
   
   ```
 Pada Sharing Session kali ini kita menggunakan beberapa Library python yaitu 
 
* `Pandas` Untuk Penanganan Data
* `tabula` Untuk Crawling Data dari pdf
* `glob`   Untuk Mengambil File dari Direktori
* `os`     Untuk Menjalankan Perintah operating system pada python

### Source Code

Pada tahapan pertama yang dilakukan adalah inisiasi variabel page yang ingin dicetak dan
pembuatan direktori untuk menyimpan hasil output
```python3
namaprovinsi = "bali"
pagestart = 1
pageend = 25
```
```python3
if not os.path.exists('E:\Magang/new/'+namaprovinsi):
    os.makedirs('E:\Magang/new/'+namaprovinsi)
```

![component](gambar/1.jpeg)

Setelah Direktori dibuat tahap selanjutnya adalah melakukan convert dari pdf ke csv page per page
pada case ini output dari file csv per page disimpan didalam direktori `E:\Magang/new/bali/`

```python3
for x in range(pageend-pagestart+1):
    df = tabula.read_pdf("E:\Magang/new/bali.pdf", encoding='utf-8', spreadsheet=True, pages=pagestart+x)
    df.to_csv('E:\Magang/new/'+namaprovinsi+"/page"+str(pagestart+x)+'.csv', encoding='utf-8',index = False)
    print("Export page -----> "+str(pagestart+x))

print("export ke csv selesai")
```

![component](gambar/2.jpeg)

File Csv Per Page akan tersimpan di direktori `E:\Magang/new/bali/` ,Maka Langkah Selanjutnya adalah menggabungkan
file csv tersebut menjadi 1.
Disini kita perlu menyimpan terlebih dahulu semua nama csv yang ada dalam direketori dengan menggunakan library *glob*
```python3
path = r'E:\Magang/new/'+namaprovinsi
all_files = glob.glob(path + "/*.csv")
```

![component](gambar/3.jpeg)

Selanjutnya kita membuka file csv tersebut dan menyimpannya dalam sebuah array.
yang lalu akan disatukan menggunakan library pada pandas yaitu concat
```python3
li = []
```

```python3
for filename in all_files:
    df = pandas.read_csv(filename, index_col=None, header=0)
    li.append(df)
```

```python3
frame = pandas.concat(li, axis=0)
```

![component](gambar/4.jpeg)

Berikut adalah output table yang disatukan
```
frame
```
![component](gambar/5.jpeg)

Setelah terconvert nama column akan berantakan dan tidak urut,
maka kira merename dan menata ulang column menggunakan pandas
```python3
frame = frame.rename(columns={"K O D E":"id_kelurahan","NAMA PROVINSI /\rKABUPATEN / KOTA":"kabupaten","LUAS\rWILAYAH\r(Km2)":"kecamatan","JUMLAH\rPENDUDUK\r(Jiwa)":"kelurahan","K E T E R A N G A N":"desa",})
```

```python3
kolomoutput = ['id_kelurahan', 'kabupaten', 'kecamatan','kelurahan','desa']
```

```python3
frame = frame.reindex(columns=kolomoutput)
```


![component](gambar/6.jpeg)
![component](gambar/8.jpeg)

Ouput akan seperti berikut

![component](gambar/9new.jpeg)

Karena Masih ada id_kelurahan yang masih null,
maka kita perlu melakukan filter pada menggunakan pandas
```python3
frame = frame[frame['id_kelurahan'].str.len() > 0]
```

![component](gambar/10.jpeg)

Dan selanjutnya adalah tahap terakhir yaitu export ke csv

```python3
frame.to_csv('E:\Magang/bali.csv', index=False,sep = ",")
```
![component](gambar/11.jpeg)

output akan seperti berikut

![component](gambar/outputpdf.jpeg)



## 2. Cleansing and Formatting Data
Seringkali raw data yang diterima seorang Data Scientist tidak dapat langsung digunakan untuk modelling, untuk tipe data seperti ini maka diperlukan tahap pre-processing; salah satunya adalah data cleansing.
Berikut ini contoh cleansing dan formating data Pulau Bali dengan hasil crawling sebagai berikut [ File Csv untuk Pulau Bali  ](https://drive.google.com/file/d/1nFHnrPCmEQhEuFcRavMP0HkbilYNuPQK/view?usp=sharing)

* Input :

   <img width="423" alt="15" src="https://user-images.githubusercontent.com/36990780/92756928-72def300-f3b7-11ea-947b-7f752485185d.PNG">
     
* Output :

   ![21](https://user-images.githubusercontent.com/36990780/92846635-7c497900-f412-11ea-8d69-cd68cd79faf2.jpg)
   
Berikut ini untuk script cleansing menggunakan python :

<img width="400" alt="1" src="https://user-images.githubusercontent.com/36990780/92688340-6337be00-f367-11ea-971c-d19d03eb145f.PNG">

```
import pandas as pd
import numpy as np
```
   Keterangan :

   * ```import pandas as pd ```:  mengimport library pandas as pd terlebih dahulu (pandas untuk membersihkan data mentah ke bentuk data yang dapat diolah)
   * ```import numpy as np ```: mengimport library numpy as pd terlebih dahulu (numpy untuk mengubah python ke pemodelan ilmiah)

```
provinsi = "Bali"
```
   Untuk menyimpan nama kolom pada provinsi 

## Membaca file csv
```
df = pd.read_csv("E:\Magang/Bali.csv",sep=',')
```
   Keterangan :
   
   * ```pd.read_csv``` : untuk membaca file dengan format CSV dan mengkonversinya menjadi pandas Dataframe
   * ```sep='``` : parameter sep=',' sesuai separator pada file
   * contoh separator : 
      <img width="291" alt="17" src="https://user-images.githubusercontent.com/36990780/92835918-063f1500-f406-11ea-8366-b584b45565a6.PNG">
   
```
df.head()
```
   Keterangan :
   * Untuk mendapatkan n baris data teratas; 
   * Output :
   
      <img width="342" alt="2" src="https://user-images.githubusercontent.com/36990780/92689707-b3b01b00-f369-11ea-8a3d-4ad663e7f6fb.PNG">

## Mengisi data kosong pada kolom kabupaten dan kecamatan
```python3
   df['kabupaten'] = df['kabupaten'].fillna(method='ffill')
   df['kecamatan'] = df['kecamatan'].fillna(method='ffill')
```
 Keterangan :
   * fillna untuk mengganti setiap NaN dengan nilai non -NaN pertama pada kolom yang sama di atasnya.
   * Output :
   
     <img width="435" alt="19" src="https://user-images.githubusercontent.com/36990780/92843642-2a532400-f40f-11ea-9dfc-908def3aa116.PNG">
 
 ## Meyeleksi kolom kelurahan sesuai dengan output yang diminta 
 ```
 df = df[df['id_kelurahan'].str.len() >10]
 ```
   Keterangan :
   * ```str.len()``` untuk mendapatkan jumlah panjang sebuah string
   * Output :
   
      <img width="423" alt="20" src="https://user-images.githubusercontent.com/36990780/92843758-4bb41000-f40f-11ea-880f-a42d5e0bca46.PNG">
   
 ## Menghilangkan integer pada kolom desa, kelurahan, kecamatan dan kabupaten
 ```python3
 df['desa'] = df['desa'].str.replace('\r\d+', '')
 df['kelurahan'] = df['kelurahan'].str.replace('\r\d+', '')
 df['kecamatan'] = df['kecamatan'].str.replace('\r\d+', '')
 df['kabupaten'] = df['kabupaten'].str.replace('\r\d+', '')
 ```
   Keterangan :
   * ```str.replace ('\r\d+', '')``` untuk mengganti '\r' dan '\d+' dengan karakter lain menjadi ''(dimana \d+ adalah regex untuk nomor integer)
   * Output :
      
      <img width="362" alt="5" src="https://user-images.githubusercontent.com/36990780/92690122-94fe5400-f36a-11ea-9675-130c0d912c2a.PNG">
 
 ## Memasukkan kolom provinsi ke dalam dataframe
 ```python3
 df['provinsi'] = provinsi
 ```
   Keterangan :
   * untuk memasukkan kolom provinsi ke dalam variable
   
   * Output :
      
      <img width="399" alt="6" src="https://user-images.githubusercontent.com/36990780/92690199-c5de8900-f36a-11ea-82fd-1f819e66ab8a.PNG">

## Menambahkan kolom keterangan sesuai dengan Output yang diminta
 ```python3
 df['keterangan'] = np.where(df['kelurahan'].isnull(), 'desa', 'kelurahan')
 ```
   Keterangan :
   * ```np.where``` atau numpy.where adalah suatu kondisi mendapatkan entri dalam array yang memenuhi kondisi
   * ```isnull()``` adalah argumen ekspresi yang diperlukan adalah varian yang berisi ekpresi numerik atau ekspresi string.
   * Output :
      
      <img width="455" alt="7" src="https://user-images.githubusercontent.com/36990780/92690319-fc1c0880-f36a-11ea-806c-d1bc4e606679.PNG">

## Mengubah nama kolom kelurahan dan kabupaten
```python3
df = df.rename(columns={"id_kelurahan":"id_dukcapil","kabupaten":"kabupaten_kota"})
```
   Keterangan :
   * ```df.rename()``` untuk mengubah nama pada kolom tertentu (bisa lebih dari 1)
   
   * Output :
      
      <img width="455" alt="8" src="https://user-images.githubusercontent.com/36990780/92690393-253c9900-f36b-11ea-9d67-3e2163d39ea0.PNG">
 
 ## Memisahkan string pada kolom id_prov, id_kab, id_kec, dan id_kel 
 ```python3
df['id_prov'] = df['id_dukcapil'].str.split('.').str[:1]
df['id_kab'] = df['id_dukcapil'].str.split('.').str[:2]
df['id_kec'] = df['id_dukcapil'].str.split('.').str[:3]
df['id_kel'] = df['id_dukcapil'].str.split('.').str[:4]
```
   Keterangan :
   * ```str.split(.)``` untuk memisahkan string di sekitar separator atau pembatas; disini separator berupa tanda "."
   * ```str[:n]``` untuk  mengekstrak seluruh sekuensi string dari awal sampai ke-n 
   * Output :
      
      <img width="647" alt="9" src="https://user-images.githubusercontent.com/36990780/92690788-c88dae00-f36b-11ea-9e45-75c8daf56834.PNG">

## Menggabungkan 
```python3
df['id_prov'] = df['id_prov'].str.join('') 
df['id_kab'] = df['id_kab'].str.join('') 
df['id_kec'] = df['id_kec'].str.join('') 
df['id_kel'] = df['id_kel'].str.join('') 
```
   Keterangan :
   * ```str.join()``` untuk  mengembalikan string di mana elemen urutan telah bergabung dengan pemisah atau separator str; disini kami menggunakan pembatas ('')
   * Output :
      
      <img width="610" alt="10" src="https://user-images.githubusercontent.com/36990780/92690897-eeb34e00-f36b-11ea-97a0-977350475ee3.PNG">
      
## Menambahkan kolom kelurahan_desa    
```python3
df['kelurahan_desa']=np.where(df['kelurahan'].isnull(), df['desa'], df['kelurahan'])
```
   keterangan :
   * ```np.where(kondisi,benar,salah)```
   * artinya jika kolom kelurahan berisi nilai null artinya "true" maka kolom kelurahan berisi data yang ada di kolom desa, tetapi jika "false" atau data tidak berisi null maka kolom kelurahan berisi data yang ada di kolom kelurahan.
   * Output :
      
      <img width="680" alt="11" src="https://user-images.githubusercontent.com/36990780/92691005-1d312900-f36c-11ea-8c52-2023847e17c2.PNG">

## Menghapus kolom kelurahan dan desa
```python3
del df['kelurahan']
del df['desa']
```
   Keterangan : 
   * ```perintah del df []``` untuk menghapus kolom; disini perintah tersebut untuk menghapus kolom kelurahan dan desa
   * Output :
      
      <img width="584" alt="12" src="https://user-images.githubusercontent.com/36990780/92691114-4651b980-f36c-11ea-9867-0047d4fe7293.PNG">

## Menyimpan nama kolom
```python3
columnsTitles = ['id_dukcapil', 'id_prov', 'id_kab','id_kec','id_kel','provinsi','kabupaten_kota','kecamatan','kelurahan_desa','keterangan']
```
   Keterangan :
   * ```ColumnsTitles``` berfungsi untuk menyimpan nama kolom yang mau diambil sesuai kebutuhan kita.
 
## Mengurutkan nama kolom sesuai input yang diminta 
 ```python3
 df = df.reindex(columns=columnsTitles)
 ```
   keterangan :
   * ```df.reindex()``` berfungsi untuk proses membuat ulang index pada tabel berdasarkan data yang terdapat dalam tabel
   * Output :
   
      <img width="600" alt="13" src="https://user-images.githubusercontent.com/36990780/92691314-96c91700-f36c-11ea-9cd9-0a5c413c5b3a.PNG">

## Menyimpan dataframe ke dalam bentuk csv
```python3
df.to_csv('E:\Magang/sprint/outputprov_'+provinsi+'.csv', index=False)
```
   Keterangan : 
   * untuk menyimpan data frame dalam bentuk csv dan disimpan ke direktori kita
      <img width="458" alt="14" src="https://user-images.githubusercontent.com/36990780/92691551-ee678280-f36c-11ea-9312-eef618f2b4f2.PNG">
   
      



   







