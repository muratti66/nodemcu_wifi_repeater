## NodeMCU ESP8266 Wifi Extender / Wifi Çoklayıcı - Güçlendirici Türkçe Döküman

Merhabalar, birçok yerden parça parça toplayıp extenderımı tamamladığım bu projemi faydalı olması dileğiyle paylaşıyorum
<br>Adım adım izleyerek sizde uygun maliyette olan bu modül ile wifi ağınızı genişletebilirsiniz diye düşünüyorum.


### Hazırlık;

- Arduino IDE nizi hazırlayın
- NodeMCU windows/mac/linux driverından ilgili olanı yükleyin


### Başlıyoruz;
- IDEnizi aşağıdaki kodu yapıştırın ve "*" kullanılan yerleri lütfen kendinize göre düzenleyin

~~~
#include <SoftwareSerial.h>
#include <ESP8266WiFi.h>

#define CONS_SPEED 115200 //* eger daha onceden baglanti hizini degistirdiyseniz

// Evdeki/Mevcut AP Cihazin Bilgileri
const char* client_ssid     = "SSID";  //*
const char* client_password = "SIFRE"; //*
const char* esp_hostname = "CLIENT_HOSTNAME"; //* Evdeki/Mevcut APye katilacak cihaza bir isim verin
// Olusturulacak Yazilimsal AP Bilgileri
const char* server_ssid     = "SOFT_AP"; //*
const char* server_password = "SIFRE"; //*
int server_channel = 11; //* Optional
IPAddress local_IP(192,168,254,1); //* Yazilimsal AP IP Adresi
IPAddress subnet(255,255,255,0); //* Yazilimsal AP Subnet
IPAddress gateway(192,168,254,1); //* Yazilimsal AP IP Ag Gecidi

void setup() {
  Serial.begin(CONS_SPEED);

  Serial.println("");
  Serial.println("Device Started..");
  //Bu bolumde evdeki/mevcut APye katilim baslar
  WiFi.mode(WIFI_AP_STA);
  Serial.print("Connecting to ");
  Serial.print(client_ssid);
  Serial.print(" : ");
  Serial.println(WiFi.begin(client_ssid, client_password) ? "Ready" : "Failed!");
  Serial.print("Waiting ap initialization..");
  while (WiFi.status() != WL_CONNECTED) {
    WiFi.hostname(esp_hostname);
    delay(500);
    Serial.print(".");
  }
  Serial.println(" Device Ready ...");
  Serial.print("WiFi connected, IP address : ");
  Serial.println(WiFi.localIP());
  //Bu bolumde Yazilimsal AP olusturulup yayin yapmaya baslar
  Serial.print("Soft-AP Creating ... ");
  WiFi.softAPConfig(local_IP, gateway, subnet);
  Serial.println(WiFi.softAP(server_ssid, server_password, server_channel) ? "Ready" : "Failed!");
  Serial.print("Soft-AP Created, IP address : ");
  Serial.println(WiFi.softAPIP());

  Serial.println("Operation Ended .");
}

void loop() {}
~~~

- Kodu derleyip cihaza yükleyin


### Mevcut AP veya Modeminizde;
- DHCP ayarlarına girip cihazın ip adresini 192.168.1.254 olarak sabitleyin.
- Yazılımsal AP kullandığı ip aralığı 192.168.254.1-254 dür. Bu aralık erişilebilir olması ve bu aralıkta alınan adresin modeme erişebilmesi için routing eklenmesi gerekmektir. 
<br>Bu ayarı Network 192.168.254.1, Subnet Mask 255.255.255.0 Gateway 192.168.1.254 şeklinde tanımlayın.


### Sonuç olarak;
NodeMCU Wifi bağlantısı yaptığınızda internete erişebileceğinizi göreceksiniz..
<br>Bağlantının çalışmaması durumunda Arduino IDE üzerinden seri port ekranı vasıtası ile çıktıları kontrol edebilirsiniz.
