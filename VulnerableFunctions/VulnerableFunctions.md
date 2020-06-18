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
