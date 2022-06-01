# GOOGLE-AS-STAN-LE-SESL-KONTROL-ED-LEN-LAMBA
Sese duyarlı şekilde açılıp kapatılan Led projesi

Kullanacağımız Malzemeler:
•	Raspberry Pi 
•	Led
•	Dişi-dişi jumper kablo
•	USB mikrofon 



![image](https://user-images.githubusercontent.com/105684427/171407442-67d3eb32-dbea-4014-9b35-8f6aa0c23bfd.png)


Dişi-dişi jumper kablolar sayesinde ledi Raspberry Pi’ ın GPIO pinlerinden uygun olana bağladık.
Mikrofonumuz ise USB portlardan herhangi birine takılabiliyor olmalı. Sesli geri dönüt almak için jack kısmına hoparlörü bağladık. Devrede de gördüğünüz gibi ledin katotunu GND hattına, anotunu ise GPIO17 adlı 11. sıradaki pine bağladık. 

Bu projede sesi algılamak için Google Speech API servisini kullandık. Bu servis internet üzerinden veri alışverişi yaptığı için Raspberry Pi ya Wi-Fi ya da ethernet kablosu ile internete bağlı olmalıdır.
Ses Komutu ile Led Kontrolü için Raspberry Pi Ayarları
Konsol ekranında şu kod parçacıklarını yazarak modülleri indirdik.
sudo apt-get install mpg321 
pip3 install SpeechRecognition
pip3 install PyAudio 
pip3 install RPi.GPiO

Hoparlör ve Mikrofon Yapılandırması
Şimdi hoparlör ve mikrofonunun yazılımsal olarak hangi porta bağlı olduğunu bulmak için aşağıdaki kodu çalıştırıyoruz.


![image](https://user-images.githubusercontent.com/105684427/171407514-c96ea2fc-72cf-47c3-baad-b7d9f0dfc06e.png)
Bu kod bize hoparlör ve mikrofonumuzun bağlı olduğu portların yazılımsal yerini söylüyor ve şuna benzer bir çıkış veriyor.


![image](https://user-images.githubusercontent.com/105684427/171407558-20fa8255-27b4-414d-b99c-d91f96a0b63b.png)

Mikrofon ve hoparlör yapılandırılması için bu “hw” ve “index” değerleri gerekli olacak. Biz mikrofonu USB portuna taktığımız için “USB PnP Sound Device” isminin mikrofonumuzu belirttiğini biliyoruz. PnP Device’ ın karşısındaki “hw:1,0” ve “index=1” değerini not aldık. Aynı şekilde hoparlörümüz de “Headphone” yani jack girişine taktığımız için “Headphones” kısmı da hoparlörümüzün yazılımsal yerini bize söyledi. Hoparlör içinse “hw:2,0” değerini not aldık.
Konsol ekranına şu kodu yazıp çalıştırıyoruz.
![image](https://user-images.githubusercontent.com/105684427/171407609-99e5435b-f8d9-428d-ab2e-1f680703279c.png)


Bu kod bize etc klasörü içinde asound.conf adında bir dosya açtı. Önümüze gelen ekrana aşağıdaki yapılandırma kodunu yazdık.
![image](https://user-images.githubusercontent.com/105684427/171407668-da45866f-7eeb-4194-8001-9a5e0b9dfe65.png)

Bu bölümde mikrofon ve hoparlör için daha önceden not alığımız “hw” değerlerini girdik . Aksi halde ekipmanlarımız doğru çalışmaz.

Hoparlör ve mikrofonumuzun ses düzeyini ayarlamak için konsol ekranına şu kodu yazdık
                    Alsamixer
 “master” bölümünü %100′ e çıkartıp ve ses kartını açmak için F6 ya basınca karşımıza şöyle bir ekran gelecek.
 
 ![image](https://user-images.githubusercontent.com/105684427/171407788-e6bea847-b327-4921-b754-24e9e2d63846.png)




Burada ilk olarak ses kartımızı seçiyoruz. Sonra da klavyeden iki kere TAB tuşuna basıp, ses kartımızın tüm ayarlarını görebileceğimiz ekrana geliyoruz.

![image](https://user-images.githubusercontent.com/105684427/171407828-aa774ad6-2b7e-4f53-995a-b1be525f76be.png)


Şimdi de klavyeden ESC tuşu ile arayüzü kapatalım. Yine alsamixer ->F6 yapıp bu sefer “headphones” kısmına girelim. Bizi şöyle bir arayüz karşılayacak.
![image](https://user-images.githubusercontent.com/105684427/171407893-ae344433-5f40-4daf-a59b-4a66a288aa37.png)
Aşağıdaki kodla Raspberry Pi ayarları kısmına gidip hoparlörümüzün yapılandırmasını bitirelim.
           sudo raspi-config
Bu kısımda System Options -> Audio -> Headphones seçip onayladık.


Hoparlörünüzden sesli dönüt almak için os.system ile .mp3 uzantılı dosyaları ekledik.

Python kodumuzu çalıştırdığımızda tek yapmanız gereken mikrofona doğru “Işığı Aç” ya da “Işığı Kapat” şeklinde konuşmak olacaktır. Unutmayalım ki anahtar kelimemiz “AÇ” ve “KAPAT” olduğundan kodumuz sadece bu kelimelere duyarlıdır.









