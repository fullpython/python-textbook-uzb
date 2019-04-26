*********
Functions
*********


Decorator lar
=============


Ba'zida biz bir necha funksiyalarni bir xil yo'l bilan o'zgartirmoqchi bo'lamiz -- masalan, biz har bir funksiya boshlanish va tugashi oldidan biron ishni bajarmoqchi bo'lamiz, yoki qo'shimcha parameter beramzi, yoki output ini boshqa  format ga o'zgartiramiz.

Bizda o'zgarishlarni barcha funksiyalarga yozmaslikka yaxshi sabab bor -- balki bu funksiya definitioni ni juda ortiqcha va noqulay qilar, va balki biz o'zgarishlarni istalgan funksiyaga tez va oson qo'llay oladigan (va kerak holda ochiradigan) qilmoqchidirmiz.

Bu muammoni yechish uchun, biz funksiyani o'zgartiradigan funksiya yozishimiz mumkin. Biz bunday funksiyalarni *decorator* deymiz. Bizning funksiyamiz funksiya objectini o'zining parametri qilib oladi, va yangi funksiya objectini qaytaradi -- va biz yangi funksiya ni eskisiga almashtirish uchun yangi funksiya qiymatini eski funksiya ning nomi bilan almashtiramiz. Masalan, mana bu decorator qachon funksiya ishlatilganda funksiya nomi va argumentini log file ga yozib boradi::

    # decorator ni define qilamiz
    def log(original_function):
        def new_function(*args, **kwargs):
            with open("log.txt", "w") as logfile:
                logfile.write("Function '%s' called with positional arguments %s and keyword arguments %s.\n" % (original_function.__name__, args, kwargs))

            return original_function(*args, **kwargs)

        return new_function

    # mana bu  decorate qilish uchun funksiya
    def my_function(message):
        print(message)

    # and here is how we decorate it
    my_function = log(my_function)


Bizning decorator imiz ichida ( tashqi funksiya) biz orin almashadigan funksiya yozdik va uni qaytardik. O'rin almashadigan funksiya(ichki funksiya) log message yozadi va shunchaki haqiqiy funksiyani chaqiradi va uning qiymatini qaytaradi.

Shuni eslab qolingki, original funksiyani decorate qilingan funksiya bilan o'zgartirganimizda, decorator funksiyasi faqat bir marta chaqiriladi , biroq ``my_function`` ichki funksiya esa har safar uni ishlatganimizda chaqiriladi. ichki funkisya  ham o'zining (masalan ``args`` and ``kwargs``) ham decorator nin scope idagi (masalan ``original_function``) variable lardan foydalana oladi.


Ichki funksiya ``*args`` and ``**kwargs`` larni parameter sifatida olishidan, biz bu decoratorni parameter listi qanaqa  bo'lishidan qat'i nazar xohlagan funksiya decoratori qilib ishlatishimiz mumkin. Ichki funksiya xohlagan parameterlarni oladi, v ashunchaki ularni original funksiyaga jo'natadi. Biroq, agar biz  original funksiyaga noto'g'ri paramater larni yuborsak  ichida error paydo bo'laveradi.

Funksiyalarga decoratorlani qo'llashning qisqacha syntax dagi yo'li bor: Biz decorate qilmoqchi bo'lgan har bir funksiyamiz definitionidan oldin ,``@`` symbol i va decorator nomi bilan ishlatishimiz mumkin:: 

    @log
    def my_function(message):
        print(message)


Funksiya definitionidan oldingi ``@log`` , funksiya definitionidan keyingi ``my_function = log(my_function)`` bilan bir xil ma'noni bildiradi.

Biz decorator imizga qo'shimcha parameterlar berishimiz mumkin. Masalan, bizning  log qiladigan decoratorimizga ixtiyoriy  log file ni ko'rsatishimiz mumkin::

    def log(original_function, logfilename="log.txt"):
        def new_function(*args, **kwargs):
            with open(logfilename, "w") as logfile:
                logfile.write("Function '%s' called with positional arguments %s and keyword arguments %s.\n" % (original_function.__name__, args, kwargs))

            return original_function(*args, **kwargs)

        return new_function

    @log("someotherfilename.txt")
    def my_function(message):
        print(message)

Python da class method larni decorate qiladigan bir necha built-in decorator lar mavjud. Bu haqida keyingi bo'limda o'rganamiz.


.. Note:: Decorator funksiya bo'lishi shart emas -- bu istagan callable object bolishi mumkin. Ayrim odamlar decoratorni class sifatida yozishni xush ko'rishadi.

Mashq 6
----------


#. ``log`` decorator misolini qaytadan yozing,  decorator ham funksiya nomini, parameterlarini va return qiymat ni log qilib ketsin.

#. Bu decorator ni ikkita argument olib ular yig'indisini qaytaradigan funksiya da test qilib ko'ring. Funksiya natijasini print qiling  va file ga nima log qilinganini ni toping.
