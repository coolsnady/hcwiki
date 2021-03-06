# Посібник з голосування за Hard Fork у тестовій мережі Testnet

Цей посібник востаннє оновлено для v1.0.1

Починаючи з версії v0.8.0 від 13 лютого 2017 р., у тестовій мережі Decred Testnet було впроваджено для тестування механізм голосування за hard fork. За оцінками, пробне голосування має розпочатися 25 лютого 2017 року близько 13.00 за CST (Central Standard Time) та триватиме 7 днів. Якщо Ви бажаєте взяти в ньому участь, нижче можна знайти довідники для Paymetheus і додатків командного рядка

---

## Введення

Процес голосування за впровадження консенсусних змін, які б призвели до hard fork, складається з двох етапів. Примітка: наступні інтервальні блоки вказані для тестової мережі, для основної мережі mainnet вони будуть мати інший вигляд.

Перший крок - досягти порогу оновлення в мережі. Після випуску hard fork кода більшість вузлів (nodes) у мережі, що беруть участь у PoW / PoS, мають бути оновлені для того, щоб голосування могло розпочатися. Для Proof-of-Work мінімум 75% 100 останніх блоків повинні мати найновішу версію блока. Для Proof-of-Stake 75% голосів, поданих у рамках статичного інтервалу у 2016 блоків року, повинні мати для голосування найостаннішу версію

Другий крок цього процесу - це фактичне голосування. Попередній інтервал у 2016 блоків знаходиться в межах більшого інтервалу у 5040 блоків, і мережа має зачекати закінчення цього більш тривалого інтервалу у 5040 блоків. Через різну тривалість інтервалів *може* знадобитися до 5040 додаткових блоків перед початком вікна для голосування. Після цього виникає статичний інтервал у 5040 блоків під час подання голосів, і якщо 75% голосів, отриманих у рамках цього інтервала, кажуть "так" запропонованим змінам, ці зміни повністю реалізуються після одного інтервалу додаткового блока (щоб надати вузлам, що залишилися, час, необхідний для оновлення, та уникнути їх видалення із блокчейну із-зі форка). Нижче наведено спрощену схему для пояснення кожного інтервалу блока в тому порядку, в якому вони хронологічно з`являються.

Опис інтервалу | Тип інтервалу | Кількість блоків
---------------------|-------------|---------------
Мінімум 75% блоків повинні мати найостаннішу версію | Проглядання | 100
Мінімум 75% голосів повинні мати найостаннішу версію | Статичний | 2016
Інтервал після виконання вимог оновлення | Статичний | До 5040
Фактичний інтервал голосування - 75% голосів мають бути "так" для проходження | Статичний | 5040
Інтервал перед реалізацією, якщо голосування відбулося | Статичний | 5040

Якщо пропозиція не досягне 10%-го порогу голосів "ні" або "так", стейкхолдери зможуть знову проголосувати за нього протягом наступного інтервалу голосування, доки цей поріг не буде досягнуто або не закінчиться термін дії пропозиції

Нижче наведено інструкції для участі у демоверсії голосування у Testnet через стейкпул за допомогою Paymetheus та / або програмами командного рядка `dcrd`, `dcrwallet`, та `dcrctl`. Довідник з програм командного рядка використовує конфігураційні файли для передачі параметрів до додатку під час запуску. У якості альтернативи під час запуску додатку можна використовувати прапори, але вони не будуть розглядатися у цьому огляді

## Paymetheus

> Крок 1: Завантажте та встановіть Paymetheus

If you haven't already updated your Decred binaries to v2.0.0, visit the [Installation Guide](/getting-started/user-guides/paymetheus.md) and follow the directions for the Windows Installer.

> Крок 2: Запустіть Decred Testnet

У меню "Пуск" відкрийте `Decred Testnet`. Це запустить `Paymetheus`, відкриється нове вікно командного рядка та запуститься `dcrd.exe`. Якщо testnet daemon запущено вперше, деякий час займе синхронізація з blockchain testnet

In the `Paymetheus` window, you'll be greated by a "Connect to dcrd" dialog. Keep the defaults and press the continue button. The next view will have two buttons, "Create a new wallet" and "Restore wallet from seed." For this guide, it will be assumed you do not already have a seed you wish to restore.

Після натискання кнопки "Створити новий гаманець" Вам буде повідомлено інформацію щодо seed нового гаманця. Запишіть свій seed, розташуйте його у безпечному місці і ніколи ні з ким не діліться. Вам також доведеться повторно ввести його після того, як Ви натиснули кнопку "Продовжити" (CONTINUE).

Після того, як Ви надрукували seed, наступним відображуваним елементом буде "Зашифрувати гаманець" (Encrypt Wallet). Введіть приватний пароль, як описано в інструкціях. Натисніть "Зашифрувати" (ENCRYPT). Після цього Paymetheus почне створювати Ваш гаманець. Як тільки його буде створено, він відкриє сторінку огляду Вашого гаманця

> Крок 3: Заеєструйтеся на веб-сайті Stakepool

Очікуючи, поки синхронізується Ваш вузол / гаманець, зайдіть н [https://teststakepool.decred.org](https://teststakepool.decred.org) і зареєструйте новий акаунт.

> Крок 4: Придбайте монети Testnet

Далі Вам знадобиться придбати монети Testnet для покупки тікетів Testnet. Існує офіційний "кран" (faucet) Testnet, розташований за адресою [https://faucet.decred.org](https://faucet.decred.org). Щоб отримати нову адресу від Paymetheus, перейдіть на вкладку "Запит платежу" ("Request payment")в меню навігації. Натискання кнопки "Створити адресу" ("GENERATE ADDRESS") створить адресу, яка повинна починатися з "Ts". Скопіюйте та вставте цю адресу до "крану", і Ви отримаєте свої монети.

> Крок 5: Придбайте тікети Testnet

Клікніть на вкладку "Купити тікети" в меню навігації Paymetheus. На цій сторінці Ви побачите 7 полів для заповнення. Усі значення за умовчанням можуть використовуватися для придбання тікетів, **крім** "Конфігурація стейкпула" ("Stake pool preference"). Натисніть кнопку "Керування пулами" ("Manage pools"). Вам потрібно буде ввести ключ API свого акаунту в стейкпулі testnet. Для цього просто відвідайте [https://teststakepool.decred.org/settings](https://teststakepool.decred.org/settings) - Ваш API Token буде першим елементом на сторінці. Введіть його в поле API Key у Paymetheus і натисніть "Зберегти" (Save). Ваш 1-of-2 multi-sig скрипт буде автоматично згенеровано, і Ви можете натиснути "Закрити" (Close).

Потім виберіть testpoolpool.decred.org з випадного списку конфигурацій стейкпула і натисніть кнопку "Купити" (Purchase), щоб почати купувати тікети! Примітка: складність тікета дорівнює вартості тікета (cost per ticket), тому переконайтеся, що у Вас достатньо testnet монет, щоб придбати щонайменше один.

> Крок 6: Встановіть votebits Ваших тікетів через стейкпул

При використанні пула, будь-які тікети, які Ви купуєте, делегують своє право на голосування цьому стейкпулу. За умовчанням пул буде голосувати Вашими тікетами на свій розсуд. Звичайно, Ви можете забажати змінити, як само повинні голосувати Ваши тікети.

Ви можете встановити votebits своїх тікетів через інтерфейс тікетів стейкпула. Нижче наведено скріншот сторінки [https://teststakepool.decred.org/tickets](https://teststakepool.decred.org/tickets) У нижній частині секції "Live / Immature" цієї сторінки Ви побачите параметри votebit. Ви можете редагувати votebits одразу * усіх * Ваших тікетів одночасно через інтерфейс стейкпула. Тікети, показані нижче, встановлено на "Так" для питання "Чи дійсний попередній блок?" ("Previous Block Valid?") і "Так" для "Збільшити розмір блока з 1,0 МБ до 1,25 МБ" ("Increase Block Size from 1.0 MiB to 1.25MB"), в результаті чого значення Votebit дорівнює 5.

<img src="/img/testnet-voting_votebit-setting.jpg">

Для отримання деякої основної інформації про votebits відвідайте розділ "Пояснення Votebits" [An Explanation of Votebits](#an-explanation-of-votebits).

## Інструкції для програм командного рядка

> Крок 1: Завантажте та встановіть Decred

If you haven't already updated your Decred binaries to v2.0.0, visit the [Installation Guide](/getting-started/user-guides/cli-installation.md) and follow the directions for your operating system.

> Крок 2: Створіть конфігураційні файли

Якщо Ви вже знайомі з файлами `.conf` перейдіть до кроку 3.

Будь ласка, перегляньте наш розділ "Введення до конфігураційних файлів" [Configuration Files Introduction](/getting-started/startup-basics.md#configuration-files) та або створіть нові конфігураційні файли, або скопіюйте типові конфігураційні файли у каталоги, вказані для Вашої операційної системи.

> Крок 3: Відредагуйте конфігураційні файли для запуску Testnet

Для запуску `dcrd`, `dcrwallet`, та `dcrctl` у тестовій мережі просто додайте `testnet=true` або `testnet=1` до всіх трьох конфігураційних файлів. Якщо Ви використовуєте один із типових конфігураційних файлів, Ви можете просто знайти рядок, де написано `;testnet=1` ((перший параметр у розділі "Налаштування мережі" (Network Settings) та видалити крапку.

Це потрібно зробити для всіх трьох конфігураційних файлів.

> Крок 4. Створіть новий гаманець Testnet

Якщо Ви ніколи раніше не запускали гаманець Testnet, Вам потрібно буде створити новий. За допомогою файлу `dcrwallet.conf` встановіться у testnet **(див. Крок 3)**, запустіть `dcrwallet` з прапором `--create`.

Для тих, хто не знає, як це зробити, дотримуйтесь інструкцій згідно із операційною системою нижче:

**Windows**: <br />
1. Використовуючи Command Prompt або File Explorer, перейдіть до каталогу виконуваного файлу `dcrwallet` <br />

2. Якщо Ви використовуєте File Explorer, виберіть опцію "Відкрити командний рядок" ("Open command prompt") в спадному меню "Файл" <br />

3. Введіть команду `dcrwallet.exe --create`

**macOS**: <br />
1. Використовуючи Terminal або Finder, перейдіть до каталогу виконуваного файлу `dcrwallet` <br />

2. Якщо Ви використовуєте Finder, Ви можете відкрити новий Terminal у розташуванні папки, клацнувши правою кнопкою миші папку та вибравши у спадному меню пункт Services > New Terminal at Window<br />

3. Введіть команду `./dcrwallet --create`.

**Linux**: <br />
1. Будь-яким способом перейдіть до каталогу виконуваного файлу `dcrwallet` <br />

2. Введіть команду `./dcrwallet --create`

Вас проведе через звичайні підказки зі створення нового гаманця. Дотримуйтесь інструкцій на екрані. Вам буде потрібно створити приватний пароль (Ви будете використовувати його пізніше, щоб розблокувати свій гаманець під час створення транзакцій). Додатковий рівень шифрування є абсолютно необов'язковим. Ваше seed можна використовувати для відновлення Вашого гаманця на будь-якому комп'ютері, використовуючи dcrwallet. Запишіть свій seed, збережіть його у безпечному місці і ніколи нікому не віддавайте. Після закінчення процесу гаманець закриється.

> Крок 5: Запустіть dcrd на Testnet

Запустіть виконуваний файл `dcrd` з параметром `testnet=1` або `testnet=true` в конфігураційному файлі, щоб запустити Ваш вузол на testnet. Ваш вузол почне синхронізуватися з рештою мережі. Синхронізація триватиме деякий час.

> Крок 6: Запустіть dcrwallet на Testnet

Запустіть виконуваний файл `dcrwallet` з параметром `testnet=1` або `testnet=true`в конфігураційному файлі, щоб запустити Ваш гаманець на testnet. Ваш гаманець підключиться до Вашого вузла та почне синхронізувати свої адреси. Синхронізація може тривати деякий час.

> Крок 7: Зареєструйтеся на веб-сайті стейкпула

Очікуючи, поки Ваш вузол / гаманець синхронізації, відвідайте [https://teststakepool.decred.org](https://teststakepool.decred.org) зареєструйте новий акаунт. Перейдіть до кроку 8.

> Крок 8: Зачекайте, поки вузол / гаманець Testnet синхронізується

Зробіть перерву, це може зайняти деякий час.

> Крок 9. Створіть адресу відкритого ключа (Public Key Address) для використання стейкпула

Першим кроком до користування стейкпулом є генерація нової адреси відкритого ключа, який буде використовуватися для генерування 1-of-2 multisignature скрипта. Дотримуйтесь інструкцій на [https://teststakepool.decred.org/address](https://teststakepool.decred.org/address) щоб створити та зберегти цю адресу. Якщо Ви створили акаунт в стейкпулі основної мережі mainnet, це той самий.

> Крок 10: Імпортуйте свій P2SH Multi-Sig скрипт з стейкпула

Далі Вам потрібно імпортувати скрипт, який дозволить Вам делегувати пулу право на голосування. Після завершення попереднього кроку цей скрипт повинен бути доступний на сторінці [https://teststakepool.decred.org/tickets](https://teststakepool.decred.org/tickets). Знову дотримуйтеся вказівок щодо імпорту скрипта. Якщо Ви створили акаунт в стейкпулі основної мережі mainnet, це той самий.

> Крок 11: Придбайте монети Testnet

Далі Вам буде потрібно придбати монети Testnet для покупки тікетів Testnet. Існує офіційний "кран" (faucet) Testnet, розташований за адресою [https://faucet.decred.org](https://faucet.decred.org). Введіть адресу Testnet (можна отримати, виконавши команду `getnewaddress` - приклади для кожної операційної системи нижче)

    Windows: dcrctl.exe --wallet getnewaddress
    macOS/Linux: ./dcrctl --wallet getnewaddress

> Крок 12: Купіть тікети Testnet

[https://teststakepool.decred.org/tickets](https://teststakepool.decred.org/tickets) описує три варіанти купівлі тікетів. Найкращий варіант - скористатись покупкою в ручному режимі (manual purchasing), щоб Ви могли купувати тікети, коли вони Вам потрібні.

Задайте команду `dcrctl --wallet getstakeinfo` щоб побачити поточну складність. Це поточна вартість тікета (ticket price). Відкорегуйте команду purchaseticket, яка відображається на сторінці тікетів у стейкпулі, щоб врахувати цю поточну вартість тікета.

> Крок 13: Зачекайте початку голосування

По-перше, щонайменше 75% ВСІХ голосів, відданих у останніх 2016 блоках, повинні бути з вузлів, на яких працює найостанніша версія програмного забезпечення Decred. Потім перед початком голосування повинен пройти інтервал у 5040 блоків. Перевірте останній статус всього процесу голосування [https://hardforkdemo.decred.org](https://hardforkdemo.decred.org).

> Крок 14: Встановіть Votebits Ваших тікетів через стейкпул

При використанні пула, будь-які тікети, які Ви купуєте, делегують своє право на голосування цьому стейкпулу. За умовчанням пул буде голосувати Вашими тікетами на свій розсуд. Звичайно, Ви можете забажати змінити, як само повинні голосувати Ваши тікети.

Ви можете встановити votebits своїх тікетів через інтерфейс тікетів стейкпула. Нижче наведено скріншот сторінки [https://teststakepool.decred.org/tickets](https://teststakepool.decred.org/tickets) У нижній частині секції "Live / Immature" цієї сторінки Ви побачите параметри votebit. Ви можете редагувати votebits одразу * усіх * Ваших тікетів одночасно через інтерфейс стейкпула. Тікети, показані нижче, встановлено на "Так" для питання "Чи дійсний попередній блок?" ("Previous Block Valid?") і "Так" для "Збільшити розмір блока з 1,0 МБ до 1,25 МБ" ("Increase Block Size from 1.0 MiB to 1.25MB"), в результаті чого значення Votebit дорівнює 5.

<img src="/img/testnet-voting_votebit-setting.jpg">

## Пояснення Votebits

Нижче наведено скріншот усіх значущих votebit-значень для голосування версії 4:

<img alt="Graph explaining the votebit values of vote version 4." src="/img/testnet-voting_vote-version-4.jpg">

Цей скріншот досить самоочевидний. У інтерфейсі пула, якщо користувач вибрав "Так" для збільшення розміру блоку та "Так" для підтвердження валідності попереднього блока, votebits їхнього тікета встановлені на "5".

Нижче наведено скріншот, який показує, де можна знайти Votebits та Vote Version у діючій транзакції голосування за допомогою block explorer за адресою [https://testnet.decred.org](https://testnet.decred.org). Цей голос було віддано із значенням Votebit, що дорівнює 5, як видно із другого output транзакції.

<img src="/img/testnet-voting_vote-version-and-votebits.jpg">

Ви можете легко перевірити свої голосування, натиснувши на "Хеш транзакції тікета" (Ticket Transaction Hash) на будь-якому з Ваших тікетів у розділі "Проголосовані тікети" ("Voted Tickets") на сторінці [https://teststakepool.decred.org/tickets](https://teststakepool.decred.org/tickets) (ПРИМІТКА. Ваш голос * може * відображатись як V0 [Version 0] через помилку в коді стейкпула - вона зараз досліджується і може навіть вже бути вирішена до моменту публікації цього посібника.)

## Сайт демо-версії Hard Fork

Для відображення статусу впровадження нової процедури голосування було створено, [https://hardforkdemo.decred.org](https://hardforkdemo.decred.org) Кожний крок візуалізується діаграмами та досить простими поясненнями.
