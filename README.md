# dictionary
# Nedir ? 
200307 tane İngilizce kelime ve 535911 adet Türkçe kelime bulunan, eşleştirilmiş, kategorilerine ve kelime türlerine göre ayrılmış basit bir sözlük veritabanıdır.

# Nasıl Kullanırım ? 
## JSON 
Bu repositoride 2 adet dosya bulunmaktadır. Bunlardan biri dictionary-json.zip ve dictionary-sql.zip'tir. İlk dosya JSON formatında tutulmaktadır ve toplamda 1460672 adet eşleştirilmiş kelimeler bulunmaktadır. Basit bir şekilde, bu JSON dosyasını aşağıda görmüş olduğunuz Java kodu ile GSON kütüphanesini kullanarak okuyan bir örnek bulunmaktadır.   
Öncelikle eğer GSON kütüphanesi bilgisayarınızda bulunmuyorsa bunu indirin. Ya da maven tabanlı bir projedeyseniz bağımlılığı pom.xml'e ekleyin.  
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


