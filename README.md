# dictionary
# Nedir ? 
200307 tane İngilizce kelime ve 535911 adet Türkçe kelime bulunan, eşleştirilmiş, kategorilerine ve kelime türlerine göre ayrılmış basit bir sözlük veritabanıdır.

# Nasıl Kullanırım ? 
## JSON 
Bu repositoride 2 adet dosya bulunmaktadır. Bunlardan biri dictionary-json.zip ve dictionary-sql.zip'tir. İlk dosya JSON formatında tutulmaktadır ve toplamda 1460672 adet eşleştirilmiş kelimeler bulunmaktadır. Basit bir şekilde, bu JSON dosyasını aşağıda görmüş olduğunuz Java kodu ile GSON kütüphanesini kullanarak okuyan bir örnek bulunmaktadır.   
Öncelikle eğer GSON kütüphanesi bilgisayarınızda bulunmuyorsa bunu indirin. Ya da maven tabanlı bir projedeyseniz bağımlılığı pom.xml'e ekleyin.  


```
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.6</version>
</dependency>
```
---
English isimli classımız :   
```java
public class English {
    private String word;
    private String type;
    private String category;
    private String tr;
    
   // constructor, getter & setter...
```

```java
public static void main(String[] args) {
        Gson gson = new Gson();
        // İndirmiş olduğumuz JSON dosyasının uzantısı belirttik.
        String path = "/home/kaya/Downloads/dictionary-json/dictionary.json";
        try{
            JsonReader reader = new JsonReader(new FileReader(path));
            English[] data = gson.fromJson(reader, English[].class);
            for(int i=0;i<data.length;i++){
                System.out.println(data[i].toString());
            }
        } catch (FileNotFoundException fileNotFoundException) {
            fileNotFoundException.printStackTrace();
        }
    }
```
Elde edeceğimiz örnek çıktımız : 
```json
English{word='Victory', type='n.', category='Common Usage', turkish='utku'}
English{word='Victory', type='n.', category='General', turkish='utku'}
English{word='Victory', type='n.', category='General', turkish='başarı'}
English{word='Victory', type='n.', category='General', turkish='galebe'}
```


