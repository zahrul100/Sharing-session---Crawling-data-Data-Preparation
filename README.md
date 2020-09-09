
## Sharing-session

# Crawling-data-Data-Preparation


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

 ```python3 
namaprovinsi = "bali" #nama provinsi
pagestart = 1 #page awal target cetak
pageend = 25 #page akhir target cetak

if not os.path.exists('E:\Magang/new/'+namaprovinsi): #melakukan create direktori untuk output data
    os.makedirs('E:\Magang/new/'+namaprovinsi)
    
```
Pada tahapan pertama yang dilakukan adalah inisiasi variabel page yang ingin dicetak dan
pembuatan direktori untuk menyimpan hasil output

```python3 
for x in range(pageend-pagestart+1):
    df = tabula.read_pdf("E:\Magang/new/bali.pdf", encoding='utf-8', spreadsheet=True, pages=pagestart+x)
    df.to_csv('E:\Magang/new/'+namaprovinsi+"/page"+str(pagestart+x)+'.csv', encoding='utf-8',index = False)
    print("Page ke = "+str(pagestart+x))

print("done")
    
```
Setelah Direktori dibuat tahap selanjutnya adalah melakukan convert dari pdf ke csv page per page
pada case ini output dari file csv per page disimpan didalam direktori `E:\Magang/new/bali/`




## 2. Cleaning and Formatting Data
