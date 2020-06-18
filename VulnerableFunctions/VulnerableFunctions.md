# Programlama Dillerindeki Ölümcül Açıklar
Bu yazımızda programlama dillerinde sıkça kullanılan fonksiyonlardan ve bunlarda yer alan açıklardan bahsedeceğiz. Ek olarak da bu açıkların oluşmasında engel olacak şekilde kodlarımızı nasıl düzenleyebileceğimize yer vereceğiz.
## Python
İlk önce Python' da yer alan bazı fonksiyonları inceleyeceğiz. Burada input, str.format, eval ve exec fonksiyonlarını sırasıyla ele alacağız. Python'da karşımıza çıkan açıklar genellikle "Code Execution" başlığı altında incelenebilir. Bahsedeceğimiz açıklar için yazılan örnekler input fonksiyonu hariç Python 3 kullanılarak yazılmıştır. input fonksiyonu için Python 2 kullanılmıştır.
### input()
Bu fonksiyonu kullanıcıdan veri almak amacıyla kullanıyoruz. Örneğini aşağıda görebilirsiniz.
```
res = input("Guess the number: ")
```
Kullanım şekli Python 2 ve 3' de aynı olan input fonksiyonu, Python 2' de aldığı veriyi direkt kullanırken Python 3' de string' e çevirir. Python 2' deki kullanımını aşağıdaki gibi örneklendirebiliriz.
```
import random
secret_number = random.randint(1,500)
print "Pick a number between 1 to 500"
while True:
    res = input("Guess the number: ")
    if res==secret_number:
        print "You win"
        break
    else:
        print "You lose"
        continue
```
Yukarıda yer alan kod çalıştırıldığında sizden bir input beklemektedir. İstenilen input yerine secret_number yazıldığında ise ekrana "You win" yazısını bastığını görebilirsiniz. Bu da bize input fonksiyonunun kod çalıştırabildiğini gösteriyor. Peki bu açığı kapatma şansımız var mı? Python 3' de bu açığın oluşmamasının ana nedeni input fonksiyonunun aldığı değer üzerinde string dönüşümü yapmasıydı. benim bu noktada aklıma gelen ilk çözüm aşağıdaki gibi oldu.
```
int(input("Guess the number: "))
```
Ancak bu ifade ile kod çalıştırıldığında aynı açığın devam ettiğini görebilirsiniz. 
Bu açığı kapatma maksadıyla Python 2' de raw_input fonksiyonu kullnılabilir.
```
raw_input("Guess the number: ")
```
Python 3' de input fonksiyonundaki bu açık kapatılarak raw_input fonksiyonu ise kullanım dışı bırakılmıştır.
### str.format()
Bu fonksiyon string ifadeleri biçimlendirmek amacıyla kullanılmaktadır.
```
text = "hello {name}"
print(text.format(name = "word"))
```
Yukarıda yer alan örneğin çıktısı aşağıdaki gibidir.
```
hello word
```
Ancak bu fonksiyonda da "Code Execution" açığı ile karşılaşmaktayız. Yukarıdaki kod örneğinde "word" yerine locals() yazdığımızda aşağıdaki gibi bir çıktı elde edeceğiz.
```
hello {'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x0000025F5EF04550>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, '__file__': '.\\strformat.py', '__cached__': None, 'secret': 'IAMPASSWORD', 'text': 'hello {name}'}
```
Bu açığın oluşmaması için kullanıcıdan veri alınan yerlerde format string kullanılmaması gerekir.
### eval()
Bu fonksiyon string olarak verilen bir ifadenin çalıştırılabilir olmasını sağlamaktadır.
```
eval("print('hello')")
```
Uygulamalarda genellikle matematiksel işlemleri gerçekleştirmek amacıyla kullanılır. Bu fonksiyonun kod çalıştırılabilir olması demek onun bir açık barındırdığını göstermez. Bu noktada önemli olan uygulama içerisinde nasıl kullanıldığıdır. Örneğin aşağıdaki gibi kullanıcıdan input aldığımız bir yerde kullanırsak güvenlik açığı oluşturmuş oluruz.
```
eval(input("Write mathematical expression"))
```
Eğer eval yerine ast.literal_eval fonksiyonu kullanılırsa input olarak verilen ifadeler için python veri yapıları olup olmadığı kontrol edilerek işlem yapılır.
### exec()
Bu fonksiyon eval fonksiyonunda olduğu gibi kod çalıştırabilmemizi sağlamaktadır. Aynı yöntemler kullanılarak uygulamadaki açıktan faydalanılabilir. Peki bu iki fonksiyon aarsındaki fark nedir? Öncelikle eval fonksiyonu single expression çalıştırır. Ancak exec fonksiyonu kod bloğu çalıştırabilmektedir. Aşağıda bir örneğini görebilirsiniz.
```
prog = 'a=5\nb=10\nprint(a+b)'
exec(prog)
```
Diğer bir fark ise eval dönüş değeri olarak çalıştırdığı ifadenin sonucunu verir, exec ise her zaman none döner, dönüş değeri exec için önemli değildir.
## C
İkinci olarak C' de yer alan fonksiyonları inceeyeceğiz. Burada gets, strcpy, strcat ve printf fonksiyonlarını ele alacağız. Aşağıda da görülebileceği üzere C dilinde yer alan bu fonksiyonlardaki açıklar buffer overflow ile alakalıdır.
### gets()
Bu fonksiyon kullanıcıdan input almak amacıyla kullanılmaktadır.
```
int main(){
   char name[10];
   gets(name);
}
```
Örnekte de olduğu gibi gets() fonksiyonu kullanılarak alınan verinin boyutunda herhangi bir sınırlama yapamıyoruz. Veriye sınırlama getirilmemesi overflow' a sebep olabilir. Bunu önlemek için aynı işlevi gören fgets() methodu kullanılabilir.
```
int main(){
  char name[10];
  fgets(name, 10, stdin);
}
```
Örnekte de görüldüğü gibi fgets() parametre olarak uzunluk bilgisi alabilmekte dolayısıyla dinamik bir şekilde buffer için yer ayrılabilir ya da ayrılan yere göre uzunluk bilgisi girilebilir. Bu şekilde buffer overflow atağının önüne geçilebilir.
### strcpy()
Bu fonksiyon bir string ifadeyi başka bir string ifadenin içerisine kopyalamamıza yardımcı oluyor. Ancak gets() fonksiyonunda olduğu gibi source ve destination string değerlerinin boyutları sınırlandırılmadığı için buffer overflow açığı oluşabilmektedir.
```
int main(){
char name[10];
strcpy(name, "hello");
}
```
strcpy() fonksiyonu yerine strlcpy() fonksiyonu kullanılacak olursa aşağıdaki örnekte olduğu gibi kopyalanacak olan string ifadenin boyutu sınırlandırılmış olur.
```
int main(){
char name[10];
strlcpy(name, "hello", 10);
}
```
### strcat()
Bu fonksiyon iki string ifadeyi birleştirmek için kullanılır.
```
int main(){
   char first[10] = "hello ";
   char second[10] = "word";
   strcat(first, second);
   printf("%s", first);
}
```
Yukarıdaki kodun çıktısı aşağıdaki gibidir.
```
hello word
```
strcpy() fonksiyonunda olduğu gibi source değişkeninin boyu sınırlandırılmadığı durumda birleştirilmiş ifadelerin atılacağı destination değişkeninin boyunun aşılma ihtimali doğar. Bu da buffer over flow attağı yapılabilir anlamına geliyor.
### printf()
Bu fonksiyon ekrana yazı yazmak amaçlı kullanılmaktadır.
```
int main(){
    char name[10];
    gets(name);
    printf(name);
}
```
Yukarıdaki örnekte kullanıcıdan alınan name bilgisi yerine format parametreleri girilerek "Format String Attack" yapılabilir. Stack'de tutulan verileri çekebiliriz. Örneğin name yerine "%s" yazdığımız durumda stack'de tutulan string bir ifadeyi çıktımızda görebiliriz.
```
printf("%s", name);
```
Örnekte yapılan ufak değişiklik ile yazılan kodu güvenli hale getirebiliriz. String format atağına hedef olmamak için yapılması gereken ilk adım format paremetreleri ile bu parametreleri kullanan fonksiyonlarda parametrelere karşılık gelen değişken sayısı eşit olması gerekir. Kullanıcıdan alınan veriler bu fonksiyonlarda direkt kullanılmamalıdır.

## PHP
Son olarak PHP'de yer alan preg_replace ve assert fonksiyonlarını ele alacağız.
### preg_replace()
Bu fonksiyon string bir ifade üzerinde değişiklik yapmamıza yardımcı olur.
```
<?php 
   $text = 'The quick brown fox jumped over the lazy dog.'; 
   echo "ORIGINAL: ".$text."<br>"; 
   $reg[1] = '/brown/';
   $reg[0] = '/fox/';
   $news[0] = 'bear'; 
   $news[1] = 'black'; 
   ksort($reg); 
   ksort($news); 
   echo "RESULT: ".preg_replace($reg, $news, $text)."<br>"; 
?>
```
Yukarıda verilen kodun çıktısı aşağıdaki gibi olmaktadır.
```
ORIGINAL: The quick brown fox jumped over the lazy dog. 
RESULT: The quick black bear jumped over the lazy dog.
```
Örnekten de anlaşılacağı gibi ifadeler üzerinde rahatlıkla değişiklik yapmamıza sağlayan bu fonksiyon regex ifadeleri çalıştırabiliyor.
```
$baska = $_GET["baska"];
foreach($baska as $neyi=>$neyle){     
   echo "<br>RESULT: ".preg_replace($neyi, $neyle, $text)."<br>";           
}
```
Örneğimize yukarıdaki satırları ekledikten sonra çalıştırdığımızda kullanıcıdan aldığımız veriyi adres çubuğunda görebiliriz. Bu veriyi aşağıdaki gibi düzenlediğimizde system('id') fonksiyonunun çalıştırılır.
![resultss](https://github.com/fuxything/topluluk-makale/blob/master/VulnerableFunctions/img.png)

Çalışan kodun çıktısı ise aşağıdaki gibidir.
```
ORIGINAL: The quick brown fox jumped over the lazy dog. 
RESULT: The quick black bear jumped over the lazy dog. 
uid=1(daemon) gid=1(daemon) groups=1(daemon) SON HALİ: The quick black bear jumped over the lazy uid=1(daemon) gid=1(daemon) groups=1(daemon).
```
Bağlantıda yer alan i modifier e ile yer değiştirilerek istenilen PHP fonksiyonunun çalışması sağlanmış oldu. preg_replace fonksiyonunu aşağıdaki gibi kullandığımız durumda system('id') fonksiyonunu çalıştıramaz.
```
preg_replace('#' . preg_quote($neyi, '#') . '#', $neyle, $text);
```
preg_quote fonksiyonu özel karakterlerin önüne '\' getirir. Özel karakterler ise şunlar: ". \ + * ? [ ^ ] $ ( ) { } = ! < > | : -". Yukarıda bahsedilen i ve e modifier'lar ise PHP'de yer alan "pattern modifier"larıdır. Bunlardan i harfler için e ise eval işlevini temsil eder.
### assert()
.......

### Referanslar:
* https://medium.com/swlh/hacking-python-applications-5d4cd541b3f1
* https://medium.com/@int0x33/day-49-common-c-code-vulnerabilities-and-mitigations-7eded437ca4a
* https://www.yeahhub.com/code-execution-preg_replace-php-function-exploitation/
