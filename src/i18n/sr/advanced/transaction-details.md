# Детаљи трансакције 

---

Децред трансакције су трансфери ДЦР-а који постоје унутар блокова. Трансакције се састоје пре свега од инпута и аутпута, мада имају и неколико других поља података. 


## Формат трансакције 

Поље        | Опис                                                                                    | Величина
---          | ---                                                                                            | ---
Верзија      | Трансакцијска верзија. Овај број се користи за означавање начина на који се трансакција треба тумачити  | 4 bytes
Број улаза  | Број улаза у трансакцији која је кодирана као цијели број променљивих дужности                   | 1-9 bytes
Улази       | Серијализирана листа свих улаза трансакције                                                | Променљива
Излазни број | Број излаза у трансакцији која је кодирана као цијели број променљиве дужине                  | 1-9 bytes
Излази      | Серијализирана листа свих излаза трансакције                                               | Променљива
Време закључавања    | Време када се трансакција може потрошити. (Обично нула, у ком случају нема ефекта)       | 4 bytes
Екпири       | Висина блока на којој трансакција истиче и више није важећа                       | 4 bytes


### Улази
Улази проводе претходно израђене излазе. Постоје две врсте трансакција: сведок и не-сведок.


#### Улази који нису сведоци
Унос података о трансакцијама без реферисања односи се на непотрошени излаз и редни број. Позивање на непотрошени излаз се зове "излазна точка". Аутпут има три поља:

Поље            | Опис                                                                                                                           | Величина
---              | ---                                                                                                                                   | ---
Трансакција хеш | Хеш трансакције која садржи резултат који се троши                                                                     | 32 bytes
Излазни индекс     | Индекс производње који се троши                                                                                                   | 4 bytes
Дрво             | Који се дрво користи потрошња. То је потребно јер постоји више од једног дрвета које се користе за лоцирање трансакција у блока. | 1 byte

Осим излаза, унесни уноси садрже редни број. Овај број има више историјског значаја од релевантне употребе; Међутим, његова најважнија сврха је омогућити "закључавање" плаћања, тако да се не могу откупити до одређеног времена.


#### Инпут трансакције сведока
Анкета о трансакцији сведока садржи податке потребне да би се доказало да се излаз може потрошити. Улази сведока садрже четири поља података:

Поље            | Опис
---              | ---
Вредност            | Количина кованица за коју се преноси излаз.
Висина блока     | Висина блока која садржи трансакцију у којој се налази излаз који се троши.
Блок индекс      | Индекс трансакције у коме се налази исход који се троши.
Писмо потписа | Скрипт који задовољава захтеве сценарија у исходу који се троши.


### Излази
Излази су трансфери ДЦР-а који се могу потрошити помоћу улаза. Излази увек имају три поља података:

Поље             | Опис                                                                                     | Величина
---               | ---                                                                                             | ---
Вредност             | Количина ДЦР се шаље излазом.                                                     | 8 bytes
Верзија           | Верзија излаза. Овај број се користи за означавање начина на који се оутпут треба тумачити. | 2 bytes
Скрипт јавног кључа | Скрипт који мора бити задовољен да проведе излаз                                           | Променљива

---

## Сериализација 
Горњи формат није формат који се трансакције шаљу и примају. Приликом слања или примања трансакција, они се могу серијализовати на неколико начина. Начин на који би трансакција требало десеријализирати може се одредити из његове верзије. Верзије трансакције заузимају четири бајта, али те четири бајта заправо користе за складиштење две одвојене вредности. Прва два бајта верзије трансакције означавају његов број актуелног броја. Друга два бајта означавају формат за серијацију.


### Формати за серијализацију
При серијализацији постоје два главна дела трансакције: његов "префикс" и његови подаци о сведочењу.
Префикс трансакције се састоји од:

* Улаз (без података о свједоцима)
* Излази
* Време закључавања
* Истиче

Подаци сведока трансакције укључују само његове податке. Укључена поља података његових улаза овисе о специфичном формату серије. Овај формат може бити један од следећих:

* **0 (пуну серијализацију)** - Префикс трансакције се налази непосредно пре његовог сведочења.
* **1 (без сведока)** - Префикс трансакције је једини податак који је присутан.
* **2 (Само сведок)** - Подаци сведока трансакције су једини подаци који су присутни. За сваки улаз, ово укључује његову вриједност, висину блока, индекс блока и потпис.
* **3 (Потписивање сведока)** - Подаци о сведочењу трансакције су једини подаци који су присутни и серијализовани су за потребе потписивања. За сваки улаз, ово укључује само његов потпис.
* **4 (Потписивање сведока са вредношћу)** - Подаци о сведочењу трансакције су једини подаци који су присутни и редиговани су за потребе потписивања. За разлику од сведока који потписује, овај формат укључује вредност сваког уноса пре потписивања.
