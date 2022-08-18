# VisualVMDescription
Участок №1

20:33:28: Executing task 'JvmExperience.main()'...
> Task :compileJava UP-TO-DATE
> Task :processResources NO-SOURCE
> Task :classes UP-TO-DATE

> Task :JvmExperience.main()
>Please open 'ru.netology.JvmExperience' in VisualVm

Наблюдаем запуск подсистемы загрузки классов, найдена точка входа, погрузились системные классы, в стеке сформирован фрейм содержащий ссылку на Объект типа String в Heap, значение объекта = "Please open 'ru.netology.JvmExperience' in VisualVm" ,  сформирован новый фрейм, со значение 30_000 и ссылкой на обект Thread.

#Участок №2
>20:33:59.309693900: loading io.vertx
>20:33:59.675768: loaded 529 classes
>20:34:02.684848500: loading io.netty
>20:34:03.520632600: loaded 2117 classes

После паузы в 30_000 миллисекунды, наблюдаем ряд новых фреймов в стеке, с небольшим увеличением количества новых объектов в Heap, ClassLoader подгрузил новые зависимости – ссылки на объекты в классах, о чем свидетельствует увеличение MetaSpace Size, Used MetaSpace, и Heap

#Участок №3
>20:34:06.535967600: loading org.springframework
>20:34:06.802040: loaded 869 classes
>20:34:06.802040: loaded 869 classes
>20:34:09.804508700: now see heap

На графике отчётливо видно сокращение числа объектов в Heap, предполагаю, что Garbage Collector избавился от объектов без связей – не нужных объектов, высвободив память, при этом наблюдается увеличение интенсивности загрузки новых классов, о чем свидетельствует восходящее направление графиков MetaSpace и Classes

#Участок №4
20:34:09.804508700: creating 5000000 objects
>20:34:10.159976: created
>20:34:13.163546100: creating 5000000 objects
>20:34:13.482711800: created
>20:34:16.589307400: creating 5000000 objects
>20:34:16.986265200: created

На данном участке наблюдается всплеск вновь созданных объектов в Heap, HeapMaxSize увеличивается примерно в двое больше, чем UsedHeap, графики изменения Heap и MetaSpace согласованы, и остаются на одном уровне, что означает количество подгружаемых классов не меняется, - новые классы не подгружаются

#Участок 5
>BUILD SUCCESSFUL in 52s
>2 actionable tasks: 1 executed, 1 up-to-date
>20:34:20: Task execution finished 'JvmExperience.main()'.

На этом участке видно, что изменений значений в Stack Heap и MetaSpace не наблюдается, можно сделать вывод что программа завершена. Garbage Collector освободил всю занимаемую память.
