*******
Classes
*******

Biz dictionary ni o'zaro bog'liq datalarni guruhlashda ishlatishimiz mumkinligini allaqachon ko'rdik va endi odatiy ishlatiladigan ifodalar guruhidan tez tez foydalanish uchun funksiya yarata olamiz. Bu funksiya bir necha input parameterlari orqali amal bajaradi. Barcha turdagi data lar uchun hamma funksiyalarni qo'llab bo'lmaydi. **Class** lar data va uning ustida bajariladigan funksiyalarni guruhlash usulidir.

Class bu shunchaki string, integer yoki list ga o'xshagan data type. Biz bu data type dan object yaratganimizda, biz uni shu classning **instance** i deymiz.

Oldinroq aytganimizdek, boshqa tillarda ba'zi narsalar object ba'zilari esa yo'q. Python da, hamma narsa object dir - hamma narsa biror bir class ning instance i. Python ning oldingi versiyalarida bult-in(pythonning ozining class lari) va foydalanuvchi yaratgan class lar orasida farq bor edi, biroq hozir ularni umuman farqlab bo'lmaydi. Class lar va type larning o'zlari ham object dir, va ularning type i ``type`` dir. Siz xohlagan object ning type ini ``type`` funksiyasi orqali topishingiz mumkin:: 

    type(any_object)


Object ichida saqlaydigan data qiymatlarimiz **attribute** lar deyiladi va objectga aloqador funksiyalar **method** lar deyiladi. Biz string,listga o'xshagan ba'zi built-in objectlarning method larini ishlatdik.
Biz objectlarimizni design qilayotganimizda, biz narsalarni birga qanday guruhlashimiz va bizning objectlarimiz  nimani namoyish etishini aniqlashimiz zarur.


Ba'zida biz real hayotga tabiatan mos tushadigan object larni yozamiz. Masalan, agar biz kimyoviy reaksiyani simulyatsiya qilmoqchi bo'lsak,  bizda ``Molecule`` objectini ifodalash uchun birlashadigan ``Atom`` objectlari bo'lishi mumkin. Biroq, har doim ham barcha kodni objectlar real hayotdagi analoglari bilan mos tushadigan qilish  majburiy ham emas, tavsiya ham qilnmaydi va har doim ham imkoni yoq.

Ba'zida, shunchaki bir necha funksiyalarni guruhlash foydali bo'lgani uchun, biz haqiqiy hayotda umuman ekvivalent i bo'lmagan objectlarni ham yaratamiz.

Class ni define(compyuterga aytish, aniqlash) qilish va ishlatish
=================================================================

Bu yerda Person(Odam) haqida informatiyani o'zida saqlovchi oddiy class misol keltirilgan::

    import datetime # biz buni date objectlari uchun ishlatamiz

    class Person:

        def __init__(self, name, surname, birthdate, address, telephone, email):
            self.name = name
            self.surname = surname
            self.birthdate = birthdate

            self.address = address
            self.telephone = telephone
            self.email = email

        def age(self):
            today = datetime.date.today()
            age = today.year - self.birthdate.year

            if today < datetime.date(today.year, self.birthdate.month, self.birthdate.day):
                age -= 1

            return age

    person = Person(
        "Jane",
        "Doe",
        datetime.date(1992, 3, 12), # year, month, day
        "12 Bo'gishamol Ko'chasi, Tashkent",
        "97 999 9999",
        "jane.doe@example.com"
    )

    print(person.name)
    print(person.email)
    print(person.age())

Biz class definition i ni ``class`` keyword i bilan boshlaymiz, undan keyin class nomi va 2 nuqta(:). Biz 2 nuqtadan oldingi qavslar orasiga **parent** class larni list qilib berishimiz mumkin, biroq bu misoldagi klass da hali ular yo'q, shunday ekan bu haqaida batafsil keyinroq.

Class ni ichida, ikkita funksiya yozamiz - bu bizning object larimizning methodlari. Birinchisi ``__init__``, maxsus method. Biz class object ini chaqirganimizda, class dan yangi **instance** yaratiladi, va darrov bu yangi objectda biz class objectiga bergan barcha parametrlarimiz bilan  ``__init__`` methodi ishlaydi. Bu method ning maqsadi biz bergan data bilan yangi objectni sozlash.

Ikkinchi method bu odamning tug'ilgan kuni va hozirgi vaqtdan foydalanib uning yoshini hisoblaydigan method.

.. Note:: ``__init__`` ba'zida object ning *constructor* i deb ataladi, chunki u boshqa tillarda constructor qanday vazifani bajarsa pythonda shunday vazifada ishlatiladi, ammo bu texnik jihatdan noto'g'ri -- uni *initialiser* deb atash yaxshiroq. ``__new__`` degan boshqa bir method constructorga ko'proq analog(o'xshash) bor, lekin u deyarli ishlatilmaydi. 

Ikkala method ning definintionida ham ``self`` degan birinchi parametr borligini sezgan bo'lsangiz kerak, va biz bu parametrni osha methodlar ichida ishlatganmiz -- lekin methodni chaqirayotganimizda bu parametrni bermaganmiz. Chunki, objectning methodini chaqirganimizda, *objectning o'zi* avtomatik ravishda birinchi parametr bo'lib yuboriladi. Bu bizga objectning methodi ichidan turib objectning property laridan foydalanish imkonini beradi.

Ayrim tillarda bu parameter *implicit* -- ya'ni, bu funksiya definitionda ko'rinmaydi -- va biz unga maxsus keyword orqali bog'lanamiz. Pythonda esa bu explicit(ochiq aytilgan) chiqarib qo'yilgan. Lekin bu aynan ``self`` deb nomlanishi shart emas, lekin bu ko'pchilik yozadigan usul.

Endi siz ``__init__``  funksiyasi attribute lar yaratib ularni biz yuborgan parameter larga sozlab qo'yganini ko'rishingiz mumkin. Biz attribute lar va parameter larga bir xil nomlarni ishlatdik, lekin bu majburiy emas.


``age`` funksiyasi ``self`` dan tashqari boshqa parametr olmaydi -- bu faqat objectning attribute larida saqlanayotgan informatsiya va hozirgi vaqtdan(``datetime`` modul i dan foydalanib olinadigan) foydalanadi

Shuni eslab qolingkim ``birthdate`` attribute ning o'zi ham object.  ``date`` class i ``datetime`` modul i ichida yozilgan, va biz bu class ning  yangi instance i dan  ``Person`` class ning instance ni yaratganimizda bithdate parametri sifatida foydalanamiz. Biz uni ``Person`` ga parametr qilib berishdan oldin biron variable olib assign qilishimiz kerak emas; biz uni shunchaki ``Person`` ni chaqiganiizda yaratamiz, xuddi boshqa parametrlar uchun string so'zlarini yartganimizdek.

Funksiyani define qilish funksiyani darhol ishga tushirmasligini bilib qo'ying. Class ni define qilish ham hech narsani darrov ishlatib yubormaydi -- bu shunchaki Pythonga class haqida aytib qo'yadi. Python classning barcha definition qismini ishlatmaguncha class tanilmaydi, demak siz bitta class dagi xohlagan methoddan turib shu class dagi boshqa method lardan foydalanishingiz mumkin, va hattoki class ichida shu class ning o'zini ham ishlatishingiz mumkin. Siz method ni chaqirgan vaqtingizgacha, butun class shubhasiz define(compyuterga aytilgan) qilingan bo'ladi.

Mashq 1
-------

#. Quyidagi variable lar nimani bildirishini  va ularning scope ini tushuntiring

    #. ``Person``
    #. ``person``
    #. ``surname``
    #. ``self``
    #. ``age`` (funksiya nomi)
    #. ``age`` (funksiya ichida ishlatilgan variable)
    #. ``self.email``
    #. ``person.email``

Instance attribute lari
=======================

Shuni bilib qo'yish kerakki, ``__init__`` funksiyasi ichida object ga sozlab qoyilgan attributelar bizning object ning olishi mumkin bo'lgan barcha attribute lari ro'yxati emas.

Ayrim tillarda objectning attribute larini class definition da aytishga majbursiz, object yaratilganda bu attributlar uchun oldindan joy ajratib qo'yiladi, va siz keyinchalik bu objectga boshqa attribute qo'sha olmaysiz. Pythonda , siz objectga  yangi attribute lar hattoki yangi method lar ni yo'lida qo'shib ketaverasiz. Aslida, attributlarni sozlashga kelganda ``__init__`` funksiyasi hech qanday o'ziga xos emas. Biz objectga cache qilingan age qiymatini ``age`` funksiyasi ichida ham saqlab qo'yishimiz mumkin::

    def age(self):
        if hasattr(self, "_age"):
            return self._age

        today = datetime.date.today()

        age = today.year - self.birthdate.year

        if today < datetime.date(today.year, self.birthdate.month, self.birthdate.day):
            age -= 1

        self._age = age
        return age

.. Note:: Attribute yoki method nomini (``_``) bilan boshlash , bu "private"(xususiy) ichki property ekanini ko'rsatish uchun foydalaniladigan odat va to'g'ridan to'g'ri chaqirilmasligi kerak. Yanada real misol, bizning cache qilingan qiymatimiz ba'zida eskirib qolishi mumkin va qayta hisoblanishi kerak -- toki biz doim doim to'g'ri qiymat olishimizga ishonch hosil qilish uchun ``age`` methodini ishlatishimiz kerak.

Biz hattoki umuman objectdan tashqari bog'liq bo'lmagan attributlarni  qo'shishimiz mumkin::

    person.pets = ['cat', 'cat', 'dog']

Objectning methodlari uchun objectning attribute lari qiymatlarini *update* qilish (yangilash) odatiy holat, biroq methodda lar ichida ``__init__`` da initialise qilinmagan attributelarni yaratish yomon  holat sanaladi. Objectdan tashqarida propertilarni o'zboshimchalik bilan sozlash undanda yomonroq hisoblanadi, toki bu object-oriented paradigm (yo'li) ni buzadi [batafsil keyingi mavzuda]

Object yaatilganda ``__init__`` methodi boshqa barcha narsadan birinchi ishlashi shubhasiz -- demak bu objectning barcha datalari initialisationi uchun eng yaxshi joy. Agar bir ``__init__`` methodidan tashqarida yangi attribute yaratsak, biz bu attribute initialise qilinmasdan oldin chaqirish ehtimolligi boligini risk(tavakkal) qilamiz.

In the ``age`` example above we have to check if an ``_age`` attribute exists on the object before we try to use it, because if we haven't run the ``age`` method before it will not have been created yet. It would be much tidier if we called this method at least once from ``__init__``, to make sure that ``_age`` is created as soon as we create the object.

Barcha attribute larning ular bo'sh qiymatga sozlasakda ``__init__`` ichida initialise qilishimiz, bizning kodimizda kamroq xato bo'lishini ta'minlaydi. Bu yana osongina o'qin tushunishga yodam beradi -- biz bir qarashda objectimizning qanaqa attribute lari borligini ko'ra olamiz.


``__init__`` methodi (``self`` dan boshqa) biron bir parameter olishga majbur emas va u butunlay bo'sh bolishi mumkin.

``getattr``, ``setattr`` va ``hasattr``
----------------------------------------

Agar biz objectning attribute ining qiymatini uning nomini yozmasdan get yoki set qilmoqchi bo'lsakchi? Biz bazida barcha attribute lar ni loop da aylanib chiqib ularda bir xil amalni bajarmoqchi bo'lamiz, xuddi mana bu dictionarydan foydalangan misoldagidek::

    for key in ["a", "b", "c"]:
        print(mydict[key])

Qanday qilib objectda ham shunga o'xshash yo'l qilish mumkin? Biz ``.`` operator idan foydalana olmaymiz, chunki bundan keyin attribute nomini yozishimiz kerak. Agar bizning attribute iz nomi variableda string qiymat sifatida saqlanayotgan bo'lsa, bizobjectning attribute qiymatlarini olish uchun ``getattr`` funksiyasidan foydalanishimiz kerak::

    for key in ["a", "b", "c"]:
        print(getattr(myobject, key, None))

Shuni eslab qolingki, ``getattr`` bult-in funksiya,  objectning methodi emas: bu ozining birinchi parametri sifatida objectni olyapti. Ikkinchi parameter varible nomi string ko'rinishida va ixtiyoriy uchinchi paramter - agar attribute mavjud bo'lmasa default qaytariladigan natija. Agar biz default qiymatni bermasak, attribute mavjud bo'lmagan holatlarda ``getattr`` xato qaytaradi.

Tepadagiga o'xshab, ``setattr`` bizga attribute ni qiymatini sozlash imkonini beradi. Bu misolda  biz dictionary dan datani objectga copy qildik::

    for key in ["a", "b", "c"]:
        setattr(myobject, key, mydict[key])


``setattr`` ning birinchi parameter i object, ikkinchisi attribute nomi, va uchinchisi attribute uchun yangi qiymat.
Oldingi ``age`` funksiya misolida ko'rganimizdek, ``hassattr``  attribute borligini tekshiradi.


Bizni ``getattr`` niattribute nomlarini  to'gridan to'g'ri yozilgan holda ishlatashimiz cheklab qo'yilmagan, biroq bu tavsiya etilmaydi: bu keraksiz ish,  attribute ga egalik qilishning qiyin usuli::

    getattr(myobject, "a")

    # ikkalasi bir xil

    myobject.a


Siz bu funksiyalarni faqatgina biror bir yaxshi sababingiz bo'lsagina ishlatishingiz kerak.

Mashq 2
-------

#. ``Person`` class ni qayta yozib chiqingki, yangi person instance yaratilganda personning yoshi i birinchi marta hisoblansin, (so'ralgan vaqtda) agar kun  oxirgi marta hisoblangan kundan o'zgargan bo'lsa qayta hisoblansin.

Class attribute lari
====================


``Person`` instance ida define qilingan barcha attribute lar, *instance attribute* lari -- ular ``__init__`` methodi ishlaganda instance ga qo'shiladi. Biroq, biz *class* da yozilgan attribute larni ham define qilishimiz mumkin. Bu attribute lar shu class ning  barcha instance lari tomonidan foydalana oladi. Ko'pchilik holatlarda ular xuddi instance attribute laridek o'zini tutadi, biroq siz bilishingiz kerak bo'lgan ba'zi ogohlantirishlar bor.


Biz class attribute larini class ichida, method definitionlari bilan bir xil darajadagi indentatsiya(4 ta probel) da define qilamiz:: 

    class Person:

        TITLES = ('Dr', 'Mr', 'Mrs', 'Ms')

        def __init__(self, title, name, surname):
            if title not in self.TITLES:
                raise ValueError("%s is not a valid title." % title)

            self.title = title
            self.name = name
            self.surname = surname


Ko'rib turganingizdek, biz ``TITLES`` class attributiga xuddi instance attributidan foydalangandek foydalandik -- bu, (method ichida ``self`` variable i bilan chaqirilgan) instance objectda property ko'rinishida foydalanildi.

Biz yaratgan barcha ``Person`` objectlari bir xil ``TITLES`` class attributini baham ko'radi.

Class attribute lari ko'pincha ma'lum bir class ga juda ham bog'liq konstantlarni define qilish uchun ishlatiladi. Biz class attribute larini class instanse laridan foydalanishimizga qaramasdan, biz ularni  instance yaratmagan holda class objectlaridan ham ishlatsak bo'ladi::

    
    # biz class attributiga instance dan turib foydalandik
    person.TITLES


    # biroq biz class ning ozidan ham foydalansak bo'ladi
    Person.TITLES

Shuni bilib qo'yingki, class objecti *instance* attribute laridan foydalana olmaydi -- ular faqat instance yaratilganda yaratiladi! ::

    # Bu error chiqaradi
    Person.name
    Person.surname


Class attributelari ham ba'zida defualt attribute qiymatlari berish uchun ishlatiladi::

    class Person:
        deceased = False

        def mark_as_deceased(self):
            self.deceased = True


Biz instanceda class attribute bilan bir xil nomdagi attribute ni sozlaganimizda, biz instance attribute ni class attribute  *ustiga yozyapmiz* (override) va instance attribute class attribute dan ustunlikni olib qo'yadi.  Agar biz 2 ta ``Person`` objecti yaratib, ularning biriga ``mark_as_deceased`` methodini chaqirsak, ikkinchisiga ta'sir qilmagan bo'lamiz. Biroq, Biz ehtiyot bo'lishimiz kerak, toki class attribute mutable type bo'lsa -- chunki agar biz uni bita joyda o'zgartirsak shu classning barcha objectlari ga  bir vaqtda *ta'sir* qilgan bo'lamiz. 
Shuni eslab qoling, barcha instanselar bitta class attributeni ishlatishadi::

    class Person:
        pets = []

        def add_pet(self, pet):
            self.pets.append(pet)

    jane = Person()
    bob = Person()

    jane.add_pet("cat")
    print(jane.pets)
    print(bob.pets) # oops!


Bunday holatlarda qilishimiz *kerak* bo'lgan ish shuki ``__init__`` ichida mutable attribute ni *instance attribute*  sifatida initialise qilish. Shunda har bir instance o'zining alohida copy siga ega bo'ladi::

    class Person:

        def __init__(self):
            self.pets = []

        def add_pet(self, pet):
            self.pets.append(pet)

    jane = Person()
    bob = Person()

    jane.add_pet("cat")
    print(jane.pets)
    print(bob.pets)

Shuni esda tutingki method definition lari class attribute lari bilan bitta scope da , demak biz class attribute nomlarini method definition larida (faqat methodlar *ichida* define qilingan ``self`` siz) ishlatsak bo'ladi::

    class Person:
        TITLES = ('Dr', 'Mr', 'Mrs', 'Ms')

        def __init__(self, title, name, surname, allowed_titles=TITLES):
            if title not in allowed_titles:
                raise ValueError("%s is not a valid title." % title)

            self.title = title
            self.name = name
            self.surname = surname


Class method lari ham bo'ladimi? Ha. KEyingi qismda ularni qanday qilib decorator bilan define qilish ni ko'amiz.

Mashq 3
-------


#. ``name``, ``surname`` and ``profession`` attribute lari orasidagi farqni tushuntiring va ular class ning turli instance larida qanday qiymatlar olishi mumkin::

    class Smith:
        surname = "Smith"
        profession = "smith"

        def __init__(self, name, profession=None):
            self.name = name
            if profession is not None:
                self.profession = profession

Class decorator lari
====================

Oldingi bo'limda biz decorator lar haqida o'rgandik -- funksiyalrning holatini o'zgartirish uchun ishlatiladigan funksiyalar. Class definitionida ishlatiladigan built-in decoratorlar mavjud.

``@classmethod``
----------------

Xuddi barcha class instance lari bir ga foydalanadigan umumiy class *attribute* larini define qilganimizdek, class *method* larini ham define qilishimiz mumkin. Biz bunga oddiy methodni ``@classmethod`` decorator i bilan decorate qilib erishishimiz mumkin.

Hali ham class methodlari o'zining birinchi parameter i da o'zini chaqiruvchi objectni oladi, biroq odatda biz bu parameter ni ``self`` emas ``cls`` deb nomlaymiz. Agar biz instance dan class method ni chaqirsak, bu parameter instance object ni o'z ichiga oladi, biroq bu methodni class dan chaqirsak bu class objectni o'z ichiga oladi. ``cls`` parametrini chaqirib, instance object dagi attribute lar bo'lishi kafolatlanmaganini o'zimizga eslatamiz.

Class method lari nima uchun yaxshi? Ularni qay holatlarda ishlatgan ma'qul? Ba'zida hech qanday class instance i yaratmasdan turib ham, faqat class konstantalari va boshqa attributelaridan foydalanib bajariladigan topshiriqlar bo'ladi. Agar biz instance method larini shu topshiriqlar uchun qo'llaydigan bo'lsak, biz hech qanday sababsiz instance object yaratishga majbur bo'lamiz, bu esa resurslarni bekorga sarflashdir. Ba'zida biz Faqat gina o'zaro bog'liq konstantalar va ular ustidagi amallarni guruh  qilish uchun class yozamiz -- biz bu class larni hech qachon instantiate(instance yaratish) qilmasligimiz mumkin.

Ba'zida class ni konstruktoriga to'g'ri formatda yuborish uchun input ni qayta ishlagandan keyin instance yaratuvchi class method yozishimiz foydali. BU konstructorni to'g'ri, aniq bolishiga imkon beradi va constructorda(__init__ ichida) hech qanday qayta ishlovchi va tozalovchi murakkab kod yozishga majbur bo'lmaymiz::

    class Person:

        def __init__(self, name, surname, birthdate, address, telephone, email):
            self.name = name
            # (...)

        @classmethod
        def from_text_file(cls, filename):
            # text file dan barcha parameter larni olish
            return cls(*params) # Bu Person(*params) ni chaqirish bilan bir xil.

``@staticmethod``
-----------------

Static methodning birinchi paramterida uni chaqiruvchi object yuborilmaydi. Bu static method class yoki instance ning boshqa qismiga bog'lana olmaydi. Biz uni instance yoki clas objecti dan chaqirishimiz mumkin, biroq ular ko'pincha class method lariga o'xshab class objectlari dan chaqiriladi.

Agar biz bir biriga , yoki class ning istalgan bir boshqa datasiga bo'glanishi majbur bo'lmagan o'zaro bog'liq methodlarni guruhlash uchun class dan foydalanayotgan bo'lsak, biz bu usuldan foydalanishimiz mumkin. Static methodning afzalligi shundaki, biz method definition laridan keraksiz ``cls`` yoki ``self`` parameterlarni olib tashlaymiz. Yomon tomoni esa agar biz bir method ichidan shu class dagi boshqa methodni chaqirmoqchi bo'lsak class nomini to'liq yozishimiz kerak, bu esa class method ichida mavjud bo'lgan ``cls`` variable ni ishlatishdan kora kodni ortiqcha kod yozishga majbur qiladi.

Bu yerda 3 xil type dagi methodlarning qisqacha farqlarini taqqoslash::

    class Person:
        TITLES = ('Dr', 'Mr', 'Mrs', 'Ms')

        def __init__(self, name, surname):
            self.name = name
            self.surname = surname

        def fullname(self): # instance method
            # instance object ga self orqali bog'lansa bo'ladi
            return "%s %s" % (self.name, self.surname)

        @classmethod
        def allowed_titles_starting_with(cls, startswith): # class method
            # class yoki instance object ga cls orqali bog'lansa bo'ladi
            return [t for t in cls.TITLES if t.startswith(startswith)]

        @staticmethod
        def allowed_titles_ending_with(endswith): # static method
            # class yoki instance object uchun parameter yo'q
            # biz Person ni to'g'ridan to'g'ri ishlatishga majburmiz 
            return [t for t in Person.TITLES if t.endswith(endswith)]


    jane = Person("Jane", "Smith")

    print(jane.fullname())

    print(jane.allowed_titles_starting_with("M"))
    print(Person.allowed_titles_starting_with("M"))

    print(jane.allowed_titles_ending_with("s"))
    print(Person.allowed_titles_ending_with("s"))

``@property``
--------------

Ba'zida objectning boshqa property laridan foydalanib objectning dinamik preopertisini yaratadigan methoddan foydalanamiz. Ba'zida siz bitta atributga bog'lanish va uni olish uchun methoddan foydalanasiz. Siz bu attribute ga to'g'ridan to'g'ri bog'lanishdan ko'ra uning qiymatini yangilaydigan boshqa methoddan foydalanasiz. Bunday methodlar *getter* lar va *setter* lar deyiladi, chunki ular attribute ning qiymatini "get"(olish, bog'lanish) and "set"(yangilash, sozlash) amallarini bajaradi.

Ba'zi tillarda barcha attributelar uchun getters va setters ishlatish, va ularning qiymatlariga to'g'ridan to'g'ri bog'lanmaslik maslahat beriladi -- va tilning aytrim xususiyatlari  attribute larga setter va getter dan tashqari bo'g'lanishni cheklab qo'yadi. Python da esa, oddiy attributega to'g'ridan to'g'ri bog'lanish mumkin, va ularning barchasiga getters va setters lar yozish ortiq cha ishdek. Setters lar murakkab asssignment operator larini ishlata olmasligi uchun noqulaydir::

    class Person:
        def __init__(self, height):
            self.height = height

        def get_height(self):
            return self.height

        def set_height(self, height):
            self.height = height

    jane = Person(153) # Jane ning bo'yi 153cm 

    jane.height += 1 # Jane 1 cm ga o'sdi
    jane.set_height(jane.height + 1) # Jane yana o'sdi

Ko'rib turganimizdek, height attribute ini setter orqali o'oshirishda ortiqcha ish bor. Albatta biz attributni berilgan parameterga oshiruvchi yana bir *ikkichi* setter yozishimiz mumkin -- lekin biz shunga o'xshagan ishni har bir attribute uchun va har bir turdagi o'zgarish uchun yozib chiqishimiz kerak. Biz joyida o'zgartirish ga o'xshagan muammo ga duch kelishimiz mumkin, xuddi listga qiymatni qo'shishdek.

Setters va getters larning foydali tomoni shuki, objectni ishlatadigan kodaga tegmasdan object ichida yaratiladigan attribute ning  yaratilishini o'zgartirishimiz mumkin. Masalan, tasavvur qiling biz boshida ``fullname`` attribute i bo'lgan ``Person`` class yaratdik, ammo keyinroq biz classga fullname ni yaratish uchun ishlatadigan alohida ``name`` va ``surname`` attribute lar qo'shmoqchimiz. Agar biz ``fullname`` attribute iga doim setter bilan murojaat qilsak, biz shunchaki setterni kodini o'zgartiramiz -- setterni chaqiradigna kodlarni yangilash shart emas.

Agar bizning kodimiz ``fullname`` ga to'g'rida  to'gri bog'lansachi? Biz to'g'ri qiymat qaytaradigan ``fullname`` methodini yozishimiz mumkin, biroq method *chaqirilishi* kerak. Baxtimizga, ``@property`` decoratori methodning attribute ga o'xshab harakatlanish imkonini beradi::

    class Person:
        def __init__(self, name, surname):
            self.name = name
            self.surname = surname

        @property
        def fullname(self):
            return "%s %s" % (self.name, self.surname)

    jane = Person("Jane", "Smith")
    print(jane.fullname) # qavslarsiz!

Bizning attribute imizni setter va deleter larini define qilish uchun ishlatishimiz mumkin bo'lgan decoratorlar ham mavjud(deleter objectimizdan attribute ni o'chirib tashlaydi). Getter, setter va deleter method larining barchasi bir xil nomda bo'lishi kerak::
    
    class Person:
        def __init__(self, name, surname):
            self.name = name
            self.surname = surname

        @property
        def fullname(self):
            return "%s %s" % (self.name, self.surname)

        @fullname.setter
        def fullname(self, value):
            # bu juda murakkab usul real hayotda
            name, surname = value.split(" ", 1)
            self.name = name
            self.surname = surname

        @fullname.deleter
        def fullname(self):
            del self.name
            del self.surname

    jane = Person("Jane", "Smith")
    print(jane.fullname)

    jane.fullname = "Jane Doe"
    print(jane.fullname)
    print(jane.name)
    print(jane.surname)

Mashq 4
-------

#. ``Numbers`` nomli class yarating, bittagina ``MULTIPLIER`` degan class attribute  ,  ``x`` va ``y`` (ikkalasi ham sonlar bo'lishi kerak) parameter larini oladigan construktor bo'lsin.

    #. ``x`` va ``y`` yig'indisini qaytaradigan ``add`` methodini yozing 
    #. Bitta  ``a`` parameterni oladigan va uni ``MULTIPLIER`` ga ko'paytirib qaytaradigan ``multiply`` nomli method yozing.
    #. ``b`` va ``c`` parameterlarni olib ,  ``b`` - ``c`` ni qaytaradigan ``subtract`` nomli static method yozing.
    #. ``x`` va ``y`` ning qiymatini tuple qilib qaytaradigan ``value`` methodini yozing. Bu methodni property qiling, ``x`` va ``y`` ningqiymatlarini boshqaradigan setter va deleter yozing.


Object ni tadqiq etish(yaxshilab o'rganish)
===========================================

Biz objectda qanaqa property lar mavjudligini ``dir`` funksiyasi bilan tekshirishimiz mumkin::

    class Person:
        def __init__(self, name, surname):
            self.name = name
            self.surname = surname

        def fullname(self):
            return "%s %s" % (self.name, self.surname)

    jane = Person("Jane", "Smith")

    print(dir(jane))    


Bu natijada biz attribut va method larni ko'rishimiz mumkin -- lekin bu yerdagi boshqa narsalar nima? Biz *inheritance* (meroslik) ni keyingi bo'limda ko'ramiz, lekin hozircha siz bilishingiz kerak bo'lgan narsa shuki: siz explicit(ataylab) aytmagan bo'lsangizda siz yozgan istalgan class ning ``object`` degan parent class mavjud -- demak, sizning class ingiz boshqa Python object laridek  bir necha default attribute va methodlar i bor.
 
.. Note:: Python 2 da biz ``object`` classidan explicit(ataylab) *inherit* qilib olishga majbur edik, bo'lmasa bizning classimiz o'zimiz yozgan method va atrributlardan tashqari bo'm bo'sh, ortiqcha narsalari bo'lmasdi.  ``object`` class idan inherit qilinmagan class lar "old-style classes"(eski usul classlari) deyiladi va ularni ishlatish maslahat berilmasdi. Agar biz Person class ini python 2 da yozmoqchi bo'lganimizda biz birinchi qatorga ``class Person(object):`` deb yozardik. 

Shuning uchun ham agar initialisation qilmoqchi bo'lmasangiz class ingizning  ``__init__`` methodini yozmasdan tashlab ketishingiz mumkin, --  ``object``  class idan inherit (meros) qilib olingan default ``__init__`` ishlatiladi. Agar siz o'zingizning ``__init__`` methodingizni yozsangiz, bu method default ni *override* (ustiga yozish) qiladi. Ba'zida biz buni *overloading* deb ataymiz.

Python object idagi ko'pgina built-in default attribute va methodlarning nomlari ikkitalik underscore ``_`` bilan boshlanib tugaydi, masalan ``__init__`` yoki ``__str__`` . Bu property larning nomlari ularning maxsus ma'noni anglatishini bildiradi -- agar siz shu nomdagi attributlarni overload qilmoqchi bo'lmasangiz bunday nomdagi attributlarni yaratmang. Bu property lar ko'pincha method bo'ladi , va ba'zida *magic methods* deyiladi.

Biz ``dir`` ni istalgan objectda qo'llashimiz mumkin. Siz bu funksiyani hozirgacha ko'rgan istalgan objectlarga qo'lashingiz mumkin, masalan number lar, list lar, string va funksiya lar, va ularning qanday umumiy built-in property lari borligini ko'ring.


Bu yerda object ning maxsus property lari ga misollar:


* ``__init__``: object ning initialisation methodi, object yaratilgan vaqtda chaqiriladi.
* ``__str__``: objectning string ko'rinishini qaytaradigna methodi, siz object ni stringga o'girish uchun ``str``funksiyasini chaqirganingizda ishlatiladi.
* ``__class__``: objectning classini (yoki type ini) o'zida saqlovchi attribute, objectda  ``type`` funksiyasini ishlatganingizda qaytariladigan narsa.
* ``__eq__``: bu object boshqasiga teng lignin aniqlaydigan method. bu object kattayoki teng, kichik yoki teng , kichik ,katta va boshqa taqqoslashlarni tekshiradigan methodlar ham bor. Bu method lar object larni taqqoslashda ishlatiladi, masalan biz ikki object tengligini tekshirish uchun tenglik operatori ``==`` ini ishlatganimizda bu method chaqiriladi.
* ``__add__`` bu objectning boshqa object ga qoshish imkonini beradigan method. Boshqa barcha arifmetik operatorlarga mos keluvchi methodlar mavhud. Barcha objectlar ham hamma arifmetik operator larni qabul qila olmaydi -- number larda hamma methodlar bo'ladi, lekin  boshqa objectlarda faqatgina bir qism operatorlar ga mos methodlar bo'lishi mumkin.
* ``__iter__``: object ustida iterator qaytaruvchi method -- biz buni string larda, list larda va boshqa iterable  larda ko'rishimiz mumkin. Bu Object ustida ``iter`` funksiyasini bajarganda ishlaydi.
* ``__len__``: object uzunligini hisoblaydigan method -- biz buni ketma-ket qatorlarda ko'rishimiz mumkin.Bu, object iustida ``len`` funksiyasini bajarganimizda ishlaydi.
* ``__dict__``: objectning barcha atribute larini o'z ichiga oladigan dictionary, attribute nomalri key lar bo'ladi.Bu objectning barcha attribute lari ni iterate(aylanib chiqish) qilganimizda foydali bo'ladi. ``__dict__`` method larni, class attributlarni yoki maxsus default attributlarni(``__class__`` ga o'xshagam) o'z ichiga olmaydi.

Mashq 5
-------


#. Ikkinchi mashqdagi ``Person`` class idan instance yarating. Bu instance da ``dir`` funksiyasini ishlating. Keyin  ``dir`` funksiyani class da ishlating.
    
    #. Instance ingizda ``__str__`` methodini chaqirganingizda nima bo'ladi? str funksiyaga instance parameter qilib berganda gi natija bilan hozirgi natija tengligini tekshiring.
    #. Instance ning type qanaqa?
    #. Class ning type i qanaqa?
    #. Parametriga kelgan objectning baracha yozilgan attributlarining nomlari va qiymatlarini print qiladigan funksiya yozing.
    
Magic method larni override qilish
==================================

We have already seen how to overload the ``__init__`` method so that we can customise it to initialise our class.  We can also overload other special methods.  For example, the purpose of the ``__str__`` method is to output a useful string representation of our object. but by default if we use the ``str`` function on a person object (which will call the ``__str__`` method), all that we will get is the class name and an ID. That's not very useful!  Let's write a custom ``__str__`` method which shows the values of all of the object's properties::
Biz ``__init__`` methodini oveload qilishni ko'rdik va biz buni class imizni initialise qilishda ishlatishimiz mumkin. Biz maxsus methodlarni ham overload qilishimiz mumkin. Masalan, ``__str__`` method ining vazifasi object ning string ko'rinishini foydali qilib ko'rsatish. Lekin default holatda , agar person object i ustida ``str `` function ni chaqirsak (``__str__`` method ni chaqiradi), biz oladigan narsa faqat class nomi bilan ID. Bu uncha foydali emas! Keling objectning barcha property larining qiymatlarini ko'rsatadigan ``__str__`` method yozamiz::

    import datetime

    class Person:
        def __init__(self, name, surname, birthdate, address, telephone, email):
            self.name = name
            self.surname = surname
            self.birthdate = birthdate

            self.address = address
            self.telephone = telephone
            self.email = email

        def __str__(self):
            return "%s %s, born %s\nAddress: %s\nTelephone: %s\nEmail:%s" % (self.name, self.surname, self.birthdate, self.address, self.telephone, self.email)

    jane = Person(
        "Jane",
        "Doe",
        datetime.date(1992, 3, 12), # year, month, day
        "No. 12 Short Street, Greenville",
        "555 456 0987",
        "jane.doe@example.com"
    )

    print(jane)


Shuni esdasaqlangki, biz birthdate objectini output string ga %s bilan berganimizda u o'zi avtomatik ravishda stringga aylanib boradi, biz buni o'zimimz qilishimiz shart emas( agar formatni o'zgartitishni xohlamasak).

Taqqoslash operatorlarini ham overload qilish foydali, toki  person objectlarimiz ustida taqqoslash operator larini ishlatishimiz mumkin. Default holatda, bizning person objectlarimiz faqat gina bir xil object bo'sagina teng hisoblanadi, va  siz  bir person objecti ikkinchisidan katta ekanligini test qila olmaysiz, sababi person objectlarining default holatdagi qatori mavjud emas.

Tassavvur qiling biz person objectlarining barcha attribute qiymatlari teng bo'lsa ular teng bo'lishini xohlaymiz, va biz ularni ism familiyalari orqali alfabet ga mos holatda saralamoqchimiz. Barcha magic taqqoslash methodlari bir biridan alohida, demak agar biz ularning barchasi ishlashini xohlasak ularning barchasini overload qilishimiz kerak -- lekin baxtimizga biz tenglik methodini va sodda navbatlash methodlaridan birini yozganimizdan keyin qolganlai oson bo'ladi. Bu methodlarning har biri ikkita parameter oladi -- ``self`` hozirgi (ayni) object uchun va ``other`` ikkichi object uchun::

    class Person:
        def __init__(self, name, surname):
            self.name = name
            self.surname = surname

        def __eq__(self, other): #  self == other ?
            return self.name == other.name and self.surname == other.surname

        def __gt__(self, other): # self > other ?
            if self.surname == other.surname:
                return self.name > other.name
            return self.surname > other.surname

        # endi boshqa barcha methodlarni boshidagi 2 ta method orqali ifodalashimiz mumkin.
        def __ne__(self, other): #  self != other ?
            return not self == other # bu self.__eq__(other) ni chaqiradi.

        def __le__(self, other): #  self <= other ?
            return not self > other # bu self.__gt__(other) ni chaqiradi.

        def __lt__(self, other): # self < other?
            return not (self > other or self == other)

        def __ge__(self, other): # bu self >= other?
            return not self < other

Shuni yodda tutingki, ``other`` boshqa bir person object bo'lishi kafolatlanmagan, va biz shunday bo'lishini tekshiradigan biron bir test qo'ymadik. Bizning methodimiz boshqa objectda ``name`` yoki ``surname`` attribute lari bo'lmasa ishlamaydi, lekin ular bo'lsa taqqoslash ishlaydi. Biz bir xil type dagi objectlar bo'ladi deb o'ylab bu taqqoslash methodlarini yozishimiz kerak.

Ba'zida agar ikkinchi object boshqa typeda bo;lsa osongina error chiqarib chiqib ketish mantiqan to'g'ri ko'rinadi, lekin ba'zida biz bir xil type da bo'lmagan 2 ta objectni taqqoslashimiz mumkin. Masalan, ``1`` va ``2.5``  ni taqqoslasj mantiqan to'g'ri  chunki ularning biri integer, yana biri float bo'lishiga qaramasdan,  ikkalasi ham number lar.

.. Note:: Python 2 da ham individual taqqoslash methodlaridan(*rich comparisons* lari deb nomlangan) oldin ``__cmp__``  ()method i bo'lgan. Bu method agar rich comparaision lar define qilinmagan bo'lsa ishlatiladi. Siz bumi rich comparision methodlariga mos ravishda overload qilishingiz kerak, bo'lmasa turli muammolarga duch kelasiz.

Mashq 6
-------

#. To'liq generic objectla yasash uchun class yozing: uning ``__init__``  methodi istalgan uzunlikdagi keyword parameterlani qabul qilishi kerak,  va ularni object nig attribute lariga sozlashi kerak, keys larni attribute nomlari sifatida. Bu class uchun ``__str__`` methodi yozing -- bu method qaytaradigan string: bu clas nomi va shu instance atttributelarining qiymatlarini bo'lsin.

Mashqlarning javoblari
======================

Mashq 1 Javobi
--------------

#.

    #. ``Person`` global scope da define qilingan class. Bu global variable.
    #. ``person`` ``Person`` class i instance i. Bu ham global variable.
    #. ``surname`` - ``__init__`` methodga yuborilgan parameter -- ``__init__`` method i scope idagi local variable.
    #. ``self``  class ning har bir methodiga yuborilgan parameter -- instance objectdan  method ``.`` operatori bilan chaqirilganda ``self`` orniga osha instance object yuboriladi.
    #. ``age`` - ``Person`` class ining methodi. Shu class ning scope ida local variable.
    #. ``age``  (funksiya ichida ishlatilgan variable)  ``age`` method i ichida ishlatilgan local variable.
    #. ``self.email``  alohida variable emas. Objectni ozida ushlab turgan varibaledan turib o'sha objectning method va attribute larini ``.`` operatori bilan chaqirishimiz mumkinlgining  misoli. Biz objectning methodlarining ichida osha object ga ``self`` variable i bilan bog'lanamiz. -- self define qilngan istalgan joyda biz ``self.email``, ``self.age()``, va h. k. larni ishlatishimiz mumkin.
    #. ``person.email`` tepadagi jovob ning xuddi o'zi. Global scopeda person instance imizga ``person`` nomli variable bilan bo'glanganmiz. ``person`` define qilingan hamma joyda , ``person.email``, ``person.age()``, va h.k. larni ishlatishimiz mumkin.

Mashq 2 Javobi
--------------

#. Mana misol uchun programma::

    import datetime

    class Person:

        def __init__(self, name, surname, birthdate, address, telephone, email):
            self.name = name
            self.surname = surname
            self.birthdate = birthdate

            self.address = address
            self.telephone = telephone
            self.email = email

            
            # Bu qa'tiy muhim emas, lekin bu attributelarni aniq tanishtiradi.
            self._age = None
            self._age_last_recalculated = None

            self._recalculate_age()

        def _recalculate_age(self):
            today = datetime.date.today()
            age = today.year - self.birthdate.year

            if today < datetime.date(today.year, self.birthdate.month, self.birthdate.day):
                age -= 1

            self._age = age
            self._age_last_recalculated = today

        def age(self):
            if (datetime.date.today() > self._age_last_recalculated):
                self._recalculate_age()

            return self._age

Mashq 3 Javobi
--------------

#. ``name`` har doim konstructoda sozlangan instance attribute, va har bir class instance turli xil name va qiymatga ega bo'lishi mumkin. ``surname`` har doim class attribute, va constructorda override qilib bo'lmaydi -- har bir instance ``Smith`` nomli surnamega ega bo'ladi. ``profession`` class attribute, lekin bu konstruktordagi instance attribute bilan override qilinishi mumkin. har bir instance constructorga  ``surname`` parametri bilan boshqa qiymat berilmasa ``smith``  qiymatli profession ga ega bo'ladi.

Mashq 4 Javobi
--------------


#. Mana misol uchun programma::

    class Numbers:
        MULTIPLIER = 3.5

        def __init__(self, x, y):
            self.x = x
            self.y = y

        def add(self):
            return self.x + self.y

        @classmethod
        def multiply(cls, a):
            return cls.MULTIPLIER * a

        @staticmethod
        def subtract(b, c):
            return b - c

        @property
        def value(self):
            return (self.x, self.y)

        @value.setter
        def value(self, xy_tuple):
            self.x, self.y = xy_tuple

        @value.deleter
        def value(self):
            del self.x
            del self.y

Mashq 5 Javobi
--------------

#.

    #. ``'<__main__.Person object at 0x7fcb233301d0>'`` ga o'xshagan narsa ko'rishingiz kerak.
    #. ``<class '__main__.Person'>`` -- ``__main__`` siz ishlatayotgan programma uchun Python ning nomi.
    #. ``<class 'type'>`` -- xohlagan class ``type`` tabiatanype i bor.
    #. Here is an example program::

            def print_object_attrs(any_object):
                for k, v in any_object.__dict__.items():
                    print("%s: %s" % (k, v))

Answer to exercise 6
--------------------

#. Mana misol uchun programma::

    class AnyClass:
        def __init__(self, **kwargs):
            for k, v in kwargs.items():
                setattr(self, k, v)

        def __str__(self):
            attrs = ["%s=%s" % (k, v) for (k, v) in self.__dict__.items()]
            classname = self.__class__.__name__
            return "%s: %s" % (classname, " ".join(attrs))
