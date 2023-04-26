# golang_sobes

## 1 Golang Key Concepts
Компилируемый, многопоточный, со статической типизацией 
язык программирования. 

Go является кроссплатформенным языком программирования. Это означает, что один и тот же код на Go может быть скомпилирован для разных операционных систем, таких как Windows, macOS и Linux, а также для разных архитектур, таких как x86, ARM и других. Кроме того, Go поддерживает кросскомпиляцию, что позволяет компилировать программы для разных платформ с помощью одного и того же компьютера.

Для компиляции программы на Go для определенной операционной системы, необходимо использовать соответствующий тег операционной системы при компиляции - GOOS.
При необходимости можно также указать тег архитектуры процессора (например, GOARCH=amd64 для 64-битных процессоров).
После компиляции программа будет скомпилирована для определенной операционной системы и архитектуры, и может быть запущена на соответствующей платформе.


### 1,1 Syntax
Синтаксис Go напоминает синтаксис языка C


### 1,2 Structs, empty struct
Структуры в Go также имеют ряд преимуществ по сравнению с классами в других языках программирования:

Простота: Структуры в Go более просты в использовании, чем классы в других языках, что делает их более доступными для начинающих программистов.

Эффективность: Структуры в Go имеют меньшую накладную в сравнении с классами в других языках, что делает их более эффективными в использовании.

Гибкость: Структуры в Go могут быть использованы для создания различных объектов с различными полями, что позволяет создавать комплексные структуры данных для различных задач.

Обычная структура в Go - это структура с определенными полями и методами, которые можно вызвать для работы с этой структурой. **Пустая структура** в Go - это структура без полей и методов, которая может быть использована, например каналов (channels) в Go. В каналах может быть использована как тип данных канала, когда важен только факт обмена сообщением между горутинами, а значение сообщения не имеет значения. Пустые структуры в Go занимают 0 байт памяти и могут быть использованы для указания наличия некоторого значения без необходимости выделения памяти под него.

### 1,3 Types
- Целочисленные типы данных: int8, int16, int32, int64, uint8, uint16, uint32, uint64, int, uint, uintptr.
- Вещественные типы данных: float32, float64.
- Комплексные числа: complex64, complex128.
- Булев тип данных: bool.
- Строковый тип данных: string.
- Тип данных "пустая структура": struct{}.
- Указатели: *T, где T - это любой тип данных.
- Функции: func.
- Интерфейсы: interface.
- Массивы: [n]T, где n - это размер массива, а T - это любой тип данных.
- Срезы: []T, где T - это любой тип данных.
- Карты: map[T1]T2, где T1 и T2 - это любые типы данных.
- Кроме встроенных типов данных, в Go также есть возможность определения пользовательских типов данных, таких как структуры и перечисления (enum).

Приставка u в типах **uint** обозначает "беззнаковый" (unsigned). Например, uint8 обозначает 8-битное беззнаковое целое число, которое может хранить значения от 0 до 255, а uint64 обозначает 64-битное беззнаковое целое число, которое может хранить значения от 0 до 18,446,744,073,709,551,615.

**rune** в Go - это псевдоним для типа int32. Он используется для представления Unicode-символов, так как каждый символ Unicode кодируется 32-битным числом.

В Go тип данных **byte** представляет собой беззнаковый 8-битный целочисленный тип, который может хранить значения от 0 до 255. byte и uint8 являются синонимами


### 1,4 Goroutines
Горутины — это легковесные потоки, которые реализуют конкурентное программирование в Go. Их называют легковесными потоками, потому что они управляются рантаймом языка, а не операционной системой. Стоимость переключения контекста и расход памяти намного ниже, чем у потоков ОС. Следовательно, для Go — не проблема поддерживать одновременно десятки тысяч горутин.

Переключение потоков требует восстановления регистров, таких как программные счетчики, указатели стека, регистры с плавающей запятой и т. д. Из-за этого стоимость обслуживания процесса или потока довольно высока. Кроме того, в случаях, когда данные совместно используются одноранговыми узлами, возникают дополнительные затраты на синхронизацию данных. Хотя накладные расходы на переключение между задачами максимально оптимизированы, постановка новых задач по-прежнему требует больше ресурсов. Иногда это сильно снижает производительность приложения, даже если потоки обозначены как легковесные.

Преимущество горутин в том, что они не зависят от базовой операционной системы, а скорее, существуют в виртуальном пространстве среды выполнения Go. В результате любая оптимизация горутины меньше зависит от платформы, на которой она работает. Они начинают работать с начальной емкости стека размером всего 2-4 КБ

Хотя **минимальный размер стека определен** как 2048 байтов, рантайм Go также не позволяет горутинам превышать **максимальный размер стека**; этот максимум зависит от архитектуры и составляет 1 ГБ для 64-разрядных систем и 250 МБ для 32-разрядных систем.

Если этот предел достигнут, будет выполнен вызов runtime.abort. Превысить этого размер очень просто с помощью рекурсивной функции;

Количество **логических потоков в Go равно количеству** доступных процессоров на машине. При запуске программы Go рантайм автоматически определяет количество доступных процессоров и назначает соответствующее количество логических потоков для выполнения программы. При этом количество логических потоков может изменяться в зависимости от нагрузки на систему и других факторов.

Кроме того, Go рантайм поддерживает работу с несколькими операционными потоками через мультиплексирование ввода/вывода (I/O multiplexing) и механизм блокировки/разблокировки горутин при выполнении сетевых операций и других операций, которые могут заблокировать поток выполнения. Это позволяет создавать эффективные и масштабируемые сетевые приложения в Go, не тратя ресурсы на создание и управление множеством операционных потоков.

### 1,5 Channels
В Go каналы (channels) используются для организации коммуникации и синхронизации между горутинами. Каналы представляют собой типизированные каналы связи, которые позволяют передавать значения между горутинами.

Создание канала происходит с помощью встроенной функции make, которой передается тип канала и его ёмкость (количество элементов, которое может содержать канал).

Не буферизированный канал представляет собой канал с емкостью 0, то есть он никогда не может содержать более одного элемента. Он гарантирует, что отправитель и получатель будут блокироваться на момент отправки и получения сообщения соответственно, пока другая сторона не будет готова для этого. Это делает не буферизированные каналы полезными для синхронизации горутин.

Буферизированный канал, с другой стороны, имеет емкость больше 0, что позволяет отправителю отправлять сообщения в канал без блокировки, если в канале еще есть свободное место. Однако, если канал полон, отправитель блокируется, пока другой поток не получит сообщение. Буферизированные каналы полезны, когда нужно временно сохранить данные или когда есть несколько отправителей и/или получателей, которые могут работать с асинхронной схемой.

В обоих случаях, при получении сообщения из канала, получатель блокируется, пока сообщение не будет отправлено в канал.

**Deadlock** - это ситуация, когда две или более горутины блокируют друг друга, ожидая, когда другая горутина освободит какой-то ресурс, например, канал или мьютекс. Таким образом, все горутины оказываются в состоянии ожидания и программа зависает.

**livelock** – тип взаимной блокировки, при котором несколько потоков выполняют бесполезную работу, попадая в зацикленность при попытке получения каких-либо ресурсов. При этом их состояния постоянно изменяются в зависимости друг от друга. Фактической ошибки не возникает, но КПД системы падает до 0

### 1,6 Public, private access
инкапсуляция реализуется с помощью концепции экспорта и неэкспорта идентификаторов. Идентификатор является экспортируемым, если его имя начинается с заглавной буквы, и неэкспортируемым, если его имя начинается со строчной буквы.

Инкапсулировать можно переменные, функции, структуры, поля и методы структур, интерфейсы и т.д. Принцип инкапсуляции позволяет создавать более безопасный и модульный код, так как скрывает внутреннюю реализацию объектов и предоставляет только необходимые интерфейсы для работы с ними.

### 1,7 Interfaces,empty interface
В Go интерфейс - это тип, который определяет набор методов, которые должен реализовывать любой тип, чтобы считаться реализующим данный интерфейс. Интерфейсы в Go позволяют абстрагироваться от конкретных типов данных и работать с ними на более абстрактном уровне.

**Утинная типизация (duck typing)** - это концепция когда если тип имеет методы, которые соответствуют сигнатуре методов, определенной в интерфейсе, то этот тип считается реализующим данный интерфейс. При этом само слово "реализация" не используется, вместо этого говорят о том, что тип "удовлетворяет интерфейсу".

Интерфейсы помогают уменьшить дублирование, то есть количество шаблонного кода.
Они облегчают использование в модульных тестах заглушек вместо реальных объектов.
Будучи архитектурным инструментом, интерфейсы помогают отвязывать части вашей кодовой базы.

**Пустой интерфейсный** тип не описывает методы. У него нет правил. И поэтому любой объект удовлетворяет пустому интерфейсу.

### 1,8 nil
nil в Go - это предопределенное значение, которое используется для указания на отсутствие значения или ссылки на объект. или неинициализированное состояние.

- Если указатель никуда не указывает, попытка разыменования указателя не сработает. Разыменование указателя nil приведет к сбою программы.
- Когда переменная объявляется как тип функции, ее значение по умолчанию будет nil. _(var fn func(a, b int) int_
; fmt.Println(fn == nil) // Выводит: true) 
- Срез, объявленный без композитного литерала или встроенной функции make, будет со значением nil. К счастью, ключевое слово range, а также встроенные функции len и append будут работать со срезами nil, как показано в следующем примере.
- Как и срезы, карты объявленные без композитного литерала или встроенной функции make, обладают значением nil. Карты можно прочитать даже когда у них значение nil, что показано в следующем листинге, хотя запись в карту nil вызовет сбой.

### Pointers
Указатель — это местоположение в памяти, где хранится значение. Значение указателя — это адрес переменной. 

В Go указатели объявляются с помощью символа * перед типом данных

Другой способ получить указатель — использовать встроенную функцию new
Функция new принимает аргументом тип, выделяет для него память и возвращает указатель на эту память.
Функция new создаёт неименованную переменную (безымянный объект) типа Type и инициализирует её нулевым значением типа Type и возвращает указатель типа *Type.

В некоторых языках программирования есть существенная разница между использованием new и &, и в них нужно удалять всё, что было создано с помощью new. Go не такой

& — оператор взятия адреса.

В Go аргументы функций передаются по значению по умолчанию, то есть копируются в новую переменную в функции. Это означает, что изменения, сделанные внутри функции на переданных аргументах, не влияют на исходные значения.

### 1,9 Error handling
Обработка ошибок в Go основана на использовании возвращаемых значений. Функции в Go могут возвращать несколько значений, где последний из них является ошибкой. Если в процессе выполнения функции произошла ошибка, она должна быть возвращена в качестве значения.

же в Go есть функция panic(), которая позволяет генерировать ошибки в случае серьезных проблем в программе. Однако, ее использование не рекомендуется в обычном коде, так как она может привести к аварийному завершению программы. Вместо этого, лучше использовать обработку ошибок через возвращаемые значения.
### 1,1 Panic, recover, defer
**С помощью defer** можно установить функцию, которая будет вызываться независимо от того, была ли ошибка в текущей функции. Например, если нужно освободить ресурсы, когда функция закончит работу, можно использовать defer для удобства и избежания утечек ресурсов.

**Defer** работает по принципу стека: каждый вызов defer добавляет функцию в стек, а когда функция завершается, все отложенные вызовы выполняются в обратном порядке, начиная с верхней функции в стеке.

**При вызове panic** выполнение текущей функции прекращается и управление передается обработчику паники. Если в текущей функции не было отложенных вызовов (defer), то управление переходит к вызывающей функции, и так далее, до тех пор, пока не будет найден обработчик паники или пока не будет достигнут корневой уровень стека, что приводит к завершению программы.

**recover** - это встроенная функция в Go, которая используется для восстановления управления в случае возникновения паники в программе. Она может быть использована только внутри функции, которая была вызвана с помощью defer. Если же одна из отложенных функций вызывает recover(), то программа продолжит свою работу с того места, где произошла паника, и recover() вернет значение, которое было передано в panic().

## 2 Golang Advanced
### 2,1 Select statement
- Оператор select позволяет go-процедуре находиться в ожидании нескольких операций передачи данных.

- select блокируется до тех пор, пока один из его блоков case не будет готов к запуску, а затем выполняет этот блок. Если сразу несколько блоков могут быть запущены, то выбирается произвольный.

Блок _default_ в select запускается, если никакой другой блок не готов.
Используйте default, чтобы посылать и получать данные без блокировок.

Вы можете использовать функцию _time.After()_ для ожидания определенного количества времени перед выполнением кода, даже если нет активных каналов в блокировке.
```golang
select {
case ch1 := <-chan1:
    fmt.Println("received from chan1:", ch1)
case ch2 := <-chan2:
    fmt.Println("received from chan2:", ch2)
case <-time.After(2 * time.Second):
    fmt.Println("timed out")
}

fmt.Println("continuing execution...")
```
Вы также можете использовать этот подход для выполнения какой-то задачи после определенного времени, даже если какие-то каналы все еще блокированы.

- Бесконечный цикл вместе с оператором select часто используется для ожидания сообщений из нескольких каналов одновременно. Он позволяет блокировать выполнение программы до тех пор, пока не появятся данные в одном из каналов, и затем обработать данные из этого канала. Если использовать select в цикле, то можно постоянно мониторить несколько каналов на наличие сообщений.

- for range можно читать с канала данные как из слайса.
for range и select используются для чтения из канала, но имеют некоторые различия.
for range читает из канала, пока он не будет закрыт. Это удобно использовать, когда вы знаете, что канал будет закрыт после передачи всех значений, и вы хотите прочитать все значения по порядку. 
Таким образом, for range удобно использовать для чтения из канала, когда вы знаете, что канал будет закрыт после передачи всех значений, а select удобно использовать для выбора из нескольких каналов, когда вы не знаете, какой из них будет готов к чтению.

Чтение из канала
1. Чтение из nil канала блокирует навсегда
2. Чтение из пустого канала блокирует до записи в этот канал или до его закрытия
3. Чтение из закрытого канала вернет zero value типа канала и false вторым значением
4. Чтение из однонаправленного канала (<-chan) приведет к ошибке компиляции

Запись и закрытие канала
1. Запись в заполненный или nil канал приведет к блокировки
2. Запись в закрытый канал вызывает panic
3. Закрытие nil канала вызывает panic
4. Закрытие закрытого канала вызывает panic

_Владелец канала — это горутина которая создает, пишет и закрывает канал._
 Создавать, записывать и закрывать канал лучше разрешать только владельцу

### 2,2 Timeouts
### 2,3 Mutexes
В Go Mutex (сокр. от mutual exclusion) - это объект синхронизации, который используется для блокировки доступа к общим ресурсам из нескольких горутин. Mutex гарантирует, что только одна горутина может получить доступ к критической секции кода в любой момент времени.

Mutex обычно используется в ситуациях, когда несколько горутин могут пытаться одновременно получить доступ к общему ресурсу, например, общему объекту или файлу. При таких обстоятельствах возникает необходимость синхронизации доступа к этим ресурсам, чтобы гарантировать правильную работу программы.

_В Go есть два типа Mutex: sync.Mutex и sync.RWMutex._ RWMutex нужен, когда у нас есть объект, который нельзя параллельно писать, но можно параллельно читать. Например, стандартный тип map.
Перед записью в защищаемый мьютексом объект делается .Lock(), а вызовы .Lock() и .RLock() в других горутинах будут ждать, пока вы не отпустите мьютекс через .Unlock().
Перед чтением защищаемого объекта делается .RLock() и только вызовы .Lock() в других горутинах блокируются, вызовы .RLock() спокойно проходят. Когда отпускаете мьютекс через .RUnlock(), ждущие вызовы .Lock() по-очереди могут забирать мьютекс на себя.
Таких образом обеспечивается параллельное чтение объекта несколькими горутинами, что улучшает производительность.


**Гонка (race condition)** - это состояние, когда несколько горутин пытаются одновременно обратиться к общему ресурсу (например, переменной) и изменить его значение. В такой ситуации порядок выполнения операций не определен, что может привести к непредсказуемому поведению программы.

В Go гонки возможны, так как горутины выполняются параллельно и могут одновременно обращаться к общим ресурсам.
### 2,4 Wait Group
Пример использования WaitGroup может быть, когда нужно запустить несколько горутин, которые должны завершить свою работу до продолжения выполнения основной программы. В этом случае, для каждой запущенной горутины мы добавляем 1 к счетчику WaitGroup. После того, как горутина завершила свою работу, она уменьшает счетчик на 1. После запуска всех горутин мы вызываем метод WaitGroup.Wait(), который ожидает, пока счетчик не станет равным 0, что означает, что все горутины завершили свою работу.

Синтаксис использования WaitGroup достаточно прост: сначала нужно создать переменную типа WaitGroup, затем запустить горутину и добавить ее в группу при помощи метода Add(), а в самой горутине вызвать метод Done(), когда она завершит работу. В конце программы нужно вызвать метод Wait() у переменной WaitGroup, чтобы программа дождалась завершения всех горутин в группе.

### 2,5 Slices, arrays, map
В Go срез (slice) представляет собой динамический массив фиксированной длины. Он является ссылкой на последовательность элементов массива и содержит три поля: указатель на первый элемент в массиве, длину среза и его емкость.

Синтаксис создания среза похож на создание массива, но без указания его размера в квадратных скобках. Например, a := []int{1, 2, 3} создаст срез из трех элементов типа int. Срезы также можно создавать с помощью встроенной функции make, которая принимает тип среза, его длину и, опционально, его емкость.

Одно из ключевых отличий между срезами и массивами в Go заключается в том, что срезы могут динамически изменять свою длину, в то время как массивы имеют фиксированную длину, которая определяется при их создании. Это означает, что при изменении длины среза, Go может перевыделить память для среза и скопировать значения элементов в новую область памяти, если это необходимо.

Кроме того, срезы являются ссылочными типами, поэтому передача среза в функцию означает передачу ссылки на массив, а не его копирование. Это позволяет создавать более эффективный код, когда нужно работать с большими массивами данных.

Срезы также могут быть созданы из других срезов или массивов, с помощью оператора «диапазон» (:) или функции append. Оператор «диапазон» позволяет выбрать определенный диапазон элементов из существующего среза, например, a[1:4] создаст новый срез, состоящий из элементов с индексами от 1 до 3 включительно. Функция append позволяет добавлять новые элементы в конец существующего среза.

--

**append возвращает новый срез**, который содержит все элементы старого среза и добавленные элементы. Если длина нового среза меньше емкости, то новая емкость будет такой же, как и старая. Если длина нового среза превышает емкость, то append выделяет новый массив и копирует туда старые значения и новые добавленные значения.

Однако, есть несколько подводных камней, связанных с использованием append. Например, если срез не имеет достаточной емкости, чтобы добавить новые элементы, append выделит новый массив и скопирует в него все элементы старого среза, а затем добавит новые элементы. Это может привести к неожиданно низкой производительности, если append вызывается много раз для больших срезов. Чтобы избежать этого, можно заранее выделить память для среза с помощью функции make.

### 2,6 Strings

Тип rune можно использовать, например, для работы со строками, которые содержат символы на разных языках или символы Unicode, такие как эмодзи. Например, если мы хотим перебрать все символы в строке, включая многобайтные символы, мы можем использовать цикл for range и тип rune

### 2,7 Dependency management
### 2,8 Context
### 3 Memory management
### 3,1 Stack (overflow)
В Go стек (stack) является частью памяти, где хранятся локальные переменные и контекст вызова функций. Каждый поток (goroutine) имеет свой собственный стек.

Стек работает по принципу LIFO (Last In First Out), то есть последний добавленный элемент в стек будет первым извлеченным. Когда функция вызывается, происходит расширение стека для хранения локальных переменных и других данных, и когда функция возвращается, стек снова уменьшается. Если размер стека превышает свою емкость, происходит переполнение стека (stack overflow), что может привести к ошибкам и аварийному завершению программы.

если используется рекурсия слишком глубоко, то это может также привести к переполнению стека. В этом случае, стек будет расти до тех пор, пока не достигнет максимально возможного размера, после чего программа завершится с ошибкой.


### 3,2 Heap
Heap (куча) - используется для хранения значений типов данных, которые могут быть выделены динамически, таких как slice, map и channel. Также heap используется для хранения объектов, созданных с помощью функций из стандартной библиотеки Go, таких как new() и make(). Одним из примеров использования кучи является хранение значений в slice, когда срез выходит за границы своей ёмкости, и происходит увеличение его размера с помощью append().

### 3,3 Garabage collection
Garbage collector (сборщик мусора) в Go - это часть среды выполнения, которая автоматически управляет памятью и удаляет объекты, которые больше не используются, чтобы предотвратить утечку памяти.

_В Go сборщик мусора использует алгоритм mark-and-sweep. Он работает следующим образом:_

1. Сборщик мусора начинает с корневого множества, которое включает все глобальные и стековые переменные.
2. Затем он просматривает все объекты, доступные из корневого множества, и помечает их как "живые".
3. Все не помеченные объекты считаются "мусором" и удаляются.
4. Затем сборщик мусора сжимает все "живые" объекты, чтобы свободное пространство было сгруппировано.
5. Наконец, сборщик мусора обновляет указатели на объекты, которые были перемещены в новое место. 

Этот процесс выполняется автоматически и в фоновом режиме, что облегчает жизнь разработчика и предотвращает многие ошибки, связанные с управлением памятью. Однако, при неправильном использовании памяти все равно можно получить утечки памяти, поэтому важно следить за своим кодом и убедиться, что все объекты, которые больше не нужны, правильно удаляются.

### 3,4 Memory leaks
Утечки памяти (или давление памяти) могут принимать разные формы. Обычно мы считаем их багами, но истинная причина возникновения может крыться ещё на стадии проектирования.

Важно построить систему таким образом, чтобы избежать преждевременных оптимизаций и позволить выполнять их позже по мере развития кода, а не перегружать его с самого начала. Некоторые распространённые примеры возникновения проблем с памятью:

слишком большое количество выделений памяти, неверное представление данных;
интенсивное использование рефлексии или строк;
использование глобальных переменных;
«осиротевшие», бесконечные горутины.
Самый простой способ создать утечку памяти в Go — определить глобальную переменную, массив, и добавить в него данные.

Golang предлагает инструмент с именем pprof. Он может помочь обнаружить проблемы с памятью. Он также может быть использован при обнаружении проблем в работе процессора.

pprof создаёт файл дампа, в который кладёт сэмпл кучи. Этот файл можно потом проанализировать/визуализировать, чтобы получить карту:

текущего выделения памяти;
общего (накопительного) выделения памяти.
У инструмента есть возможность сравнивать снимки, сделанные в разное время. Это может быть полезно при стрессовых сценариях для определения проблемных областей кода.


## 4 Data structures
### 4,1 List
### 4,2 Array
### 4,3 Hash Table
### 4,4 Queue
## 5 Network
### 5,1 TCP
### Cookies

В браузере куки представляются как набор пар "имя-значение", которые хранятся на стороне клиента в текстовом файле. Куки могут быть установлены на определенный домен и путь, и могут быть отправлены серверу вместе с каждым запросом клиента, связанным с этим доменом и путем. Сервер может использовать куки для идентификации клиента, отслеживания состояния сессии и предоставления персонализированного контента. Куки также могут быть защищены с помощью протокола шифрования HTTPS для обеспечения безопасности передачи данных между клиентом и сервером.

_В HTTP-куки могут быть заданы следующие параметры:_

- Имя и значение: это обязательные параметры, которые задают имя и значение куки. Имя не может содержать символы "равно" ("="), точку с запятой (";") или запятые (","). Значение должно быть заключено в кавычки, если содержит любой символ, кроме буквенно-цифровых символов.
- Domain: указывает домен, для которого применяется куки. Браузер отправляет куки только на серверы, которые принадлежат указанному домену. Если параметр не задан, куки будут отправляться на тот же домен, который установил их.
- Path: указывает URL-путь на сервере, для которого применяется куки. Браузер отправляет куки только на серверы, которые имеют указанный URL-путь. Если параметр не задан, куки будут отправляться на любой URL-путь на сервере.
- Expires/Max-Age: указывает время жизни куки. Параметр Expires задает конкретное время и дату, когда куки должен истечь, а параметр Max-Age задает продолжительность жизни куки в секундах. Если параметры не заданы, куки живут до закрытия браузера.
- Secure: указывает, что куки должны быть отправлены только через безопасное HTTPS-соединение.
- HttpOnly: указывает, что куки должны использоваться только для HTTP-запросов и не должны быть доступны для скриптов JavaScript. Это помогает защитить куки от атак на межсайтовое скриптование (XSS).
- SameSite: указывает, как браузер должен отправлять куки на серверы, которые не принадлежат исходному домену. Значения параметра могут быть "Strict", "Lax" или "None". "Strict" означает, что куки не будут отправляться на серверы, которые не принадлежат исходному домену, даже если это ссылка на другую страницу на этом же домене. "Lax" означает, что куки будут отправляться на серверы, которые не принадлежат исходному домену, если это ссылка на страницу на этом же домене. "None" означает, что куки могут быть отправлены на любой сервер. В параметре SameSite можно также указать время жизни куки с помощью параметров Expires или Max-Age.

### 5,2 HTTP (1.0, 1.1, 2.0)
HTTP (Hypertext Transfer Protocol) - это протокол передачи данных в Интернете, который используется для обмена информацией между клиентом и сервером. HTTP работает поверх протокола TCP (Transmission Control Protocol) и использует стандартные порты 80 и 443 (для HTTPS).

Сеть - это компьютерная система, которая позволяет компьютерам обмениваться данными между собой. HTTP (HyperText Transfer Protocol) - это протокол передачи данных, который используется для обмена информацией в Интернете.

Когда пользователь отправляет запрос через браузер, браузер создает HTTP-запрос и отправляет его на сервер. HTTP-запрос состоит из заголовка и тела. Заголовок содержит информацию о типе запроса, запрашиваемом ресурсе и других параметрах. Тело содержит данные, которые отправляются на сервер.

На данный момент существует две версии протокола HTTP:

- HTTP/1.1 - это наиболее распространенная версия, которая была представлена в 1999 году. HTTP/1.1 поддерживает многопоточность, что позволяет одновременно передавать несколько запросов и ответов между клиентом и сервером. Кроме того, HTTP/1.1 поддерживает такие функции, как кэширование, управление соединениями и виртуальные хосты.
- HTTP/2 - это новая версия протокола, которая была представлена в 2015 году. HTTP/2 поддерживает мультиплексирование, что позволяет одновременно передавать несколько запросов и ответов по одному соединению. Кроме того, HTTP/2 использует бинарный формат передачи данных, что улучшает производительность и снижает нагрузку на сервер.

Одним из основных отличий HTTP/2 от HTTP/1.1 является то, что в HTTP/2 используется одно соединение для всех запросов и ответов, что снижает задержку и улучшает производительность. Кроме того, в HTTP/2 использованы новые функции, такие как серверное push-уведомление и приоритезация потоков.
### 5,3 UDP
### 5,4 WebSockets (SSE)
## 6 Relational Databases
### 6,1 ACID
ACID - это аббревиатура, которая описывает свойства транзакций в базах данных:

- Атомарность (Atomicity) - транзакция должна быть выполнена как единое целое, все изменения должны быть либо выполнены, либо отменены.
- Согласованность (Consistency) - транзакция должна приводить базу данных от одного согласованного состояния к другому согласованному состоянию. Если данные не удовлетворяют некоторым ограничениям целостности, то транзакция не должна быть выполнена.
- Изолированность (Isolation) - транзакции должны выполняться в изоляции друг от друга. Другими словами, каждая транзакция должна видеть базу данных в том состоянии, в котором она была до ее начала, и не должна видеть изменений, внесенных другими транзакциями, пока эти изменения не будут подтверждены.
- Долговечность (Durability) - после подтверждения транзакции ее изменения должны быть сохранены в базе данных и не должны быть потеряны в случае сбоя системы.

Эти свойства обеспечивают надежность и целостность баз данных, что очень важно для бизнес-приложений, работающих с критически важными данными.

### 6,2 Transactions
### 6,3 Index
## 7 NoSQL
### 7,1 CAP theorem
### 7,2 Sharding
### 7,3 Replication
## 8 Operating System
### 8,1 Process
### 8,2 Thread
### 8,3 File
### 8,4 Syscall
## 9 Problem Solving
### 9,1 Users Rating Problem
### 9,2 Drag & Drop Problem
### 9,3 Ping Pong Problem
