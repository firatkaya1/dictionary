[For Turkish ](https://github.com/firatkaya1/dictionary/blob/main/README_TR.md) :point_left:  
[For English](https://github.com/firatkaya1/dictionary/blob/main/README.md) :point_left:  

# What is this?
It is a simple dictionary database consisting of 200307 English words and 535911 Turkish words, matched, separated by categories and word types.  
[Download JSON](https://raw.githubusercontent.com/firatkaya1/dictionary/main/dictionary-json.zip)    
[Download SQL](https://raw.githubusercontent.com/firatkaya1/dictionary/main/dictionary-sql.zip)   
# Usage  
## JSON 
There are 2 files in this repository. One of them is **dictionary-json.zip** and **dictionary-sql.zip**. The first file is kept in JSON format and there are 1460672 paired words in total. In simple terms, there is an example reading this JSON file using the GSON library with the Java code you see below.   

Firstly, if you don't have the GSON library on your computer, download it. Or if you are in a maven based project, add the dependency to pom.xml.

```
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.6</version>
</dependency>
```
---
English class :   
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
        // JSON File path
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
Our sample output:
```
English{word='Victory', type='n.', category='Common Usage', turkish='utku'}
English{word='Victory', type='n.', category='General', turkish='utku'}
English{word='Victory', type='n.', category='General', turkish='başarı'}
English{word='Victory', type='n.', category='General', turkish='galebe'}
```

## SQL
Download the file named **dictionary-sql.zip** in the repository to your computer and import it into the MySQL you used. Schema's name might be ** lemon **.  

The schema file we have obtained contains 5 tables in total. These are category, english, translate, turkish and type. The picture below will help you understand what the relationships between the tables are, even if it is small.

![image](https://raw.githubusercontent.com/firatkaya1/dictionary/main/database.png)

### Queries

You can obtain the values you want to search on the tables with the ready queries below.   

For example, suppose you want to translate the Turkish meaning of the word "hello".
```sql
SELECT translate.id as id,english.word as word_en,turkish.word as word_tr,type.name as type,category.name as category    
FROM translate     
INNER JOIN english ON translate.english_id = english.id    
INNER JOIN category ON translate.category_id = category.id    
INNER JOIN type ON translate.type_id = type.id    
INNER JOIN turkish ON translate.turkish_id = turkish.id    
WHERE translate.english_id IN (SELECT id FROM english WHERE word LIKE "hello");
```
Our sample output:

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
Contact me if you encounter any problems.

[me@kayafirat.com](mailto:me@kayafirat.com?subject=[GitHub]%20dictionary)

