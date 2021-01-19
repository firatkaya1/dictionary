# dictionary
# Nedir ? 
200307 tane İngilizce kelime ve 535911 adet Türkçe kelime bulunan, eşleştirilmiş, kategorilerine ve kelime türlerine göre ayrılmış basit bir sözlük veritabanıdır.

# Nasıl Kullanırım ? 
## JSON 
Bu repositoride 2 adet dosya bulunmaktadır. Bunlardan biri **dictionary-json.zip** ve **dictionary-sql.zip**'tir. İlk dosya JSON formatında tutulmaktadır ve toplamda 1460672 adet eşleştirilmiş kelimeler bulunmaktadır. Basit bir şekilde, bu JSON dosyasını aşağıda görmüş olduğunuz Java kodu ile GSON kütüphanesini kullanarak okuyan bir örnek bulunmaktadır.   
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
```
English{word='Victory', type='n.', category='Common Usage', turkish='utku'}
English{word='Victory', type='n.', category='General', turkish='utku'}
English{word='Victory', type='n.', category='General', turkish='başarı'}
English{word='Victory', type='n.', category='General', turkish='galebe'}
```

## SQL

Repositoride bulunan **dictionary-sql.zip** isimli dosyayı bilgisayarınıza indirerek kullanmış olduğunuz MySQL'e import edin. schema'nın ismi **lemon** diye geçebilir. 

Elde ettiğimiz schema dosyası toplamda 5 adet tablo içermektedir. Bunlar category, english, translate, turkish ve type'dir. Aşağıda bulunan resim, sizlere ufakta olsa tablolar arasındaki ilişkilerin ne olduğunu anlamanızda kolaylık sağlayacaktır. 

![image](https://raw.githubusercontent.com/firatkaya1/dictionary/main/database.png)

### Sorgular

Aşağıda bulunan hazır sorgular ile tablolar üzerinde aramak istediğiniz değerleri elde edebilirsiniz.

"hello" kelimesinin türkçe anlamlarını, kategorilerini ve tiplerini getirmek istediğimizi varsayalım. 
```sql
SELECT translate.id as id,english.word as word_en,turkish.word as word_tr,type.name as type,category.name as category    
FROM translate     
INNER JOIN english ON translate.english_id = english.id    
INNER JOIN category ON translate.category_id = category.id    
INNER JOIN type ON translate.type_id = type.id    
INNER JOIN turkish ON translate.turkish_id = turkish.id    
WHERE translate.english_id IN (SELECT id FROM english WHERE word LIKE "hello");
```
Elde edeceğimiz çıktı aşağıdaki gibi olacaktır : 

```sh
+-------------+---------+--------------+--------------+--------------+
| id          | word_en | word_tr      | type         | category     |
+-------------+---------+--------------+--------------+--------------+
| 635276      | hello   | merhaba      | interjection | Common Usage | 
| 635277      | hello   | merhaba      | interjection | General      | 
| 635278      | hello   | selam        | interjection | General      | 
| 635279      | hello   | alo          | interjection | General      | 
+-------------+---------+--------------+--------------+--------------+
```
Bir problemle karşılaşırsanız bana yazabilirsiniz. 

[me@kayafirat.com](mailto:me@kayafirat.com?subject=[GitHub]%20dictionary)

