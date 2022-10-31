## Домашнее задание по теме «JVM. Организация памяти, сборщики мусора, VisualVM»
Задача 1 (обязательная)

Описание кода с точки зрения происходящего в JVM:

    package org.example;

    public class JvmComprehension {
---- Компилятор создает файл с расширением class и помещает туда всю информацию о нашем приложении для JVM

    public static void main(String[] args) { 
---- В момент вызова метода создаётся фрейм (кадр) в стеке

        int i = 1;                      // 1*
---- переменная хранится в Stack Memory

        Object o = new Object();        // 2
---- переменная "о" хранится в Stack Memory и ссылается на new Object, который хранится в heap (куча)

        Integer ii = 2;                 // 3
---- переменная хранится в Stack Memory

        printAll(o, i, ii);             // 4
---- начинается выполнение байт-кода метода printAll.
Виртуальная машина снимает нужное количество переменных со стека и передает в метод.

        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
---- в методе сосредоточена логика программы, весь исполняемый байт-код

        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}

1) Класс JvmComprehension подгружается в систему загрузчиков классов ClassLoading: Application ->
   Platform -> Bootstrap (поиск класса, загрузил, если нашел) -> Platform (поиск класса, загрузил, если нашел) ->
   Application (поиск класса, загрузил, если нашел. Если не нашел, ошибка ClassNotFoundException)
    - загрузка происходит в область памяти - Metaspace (размер памяти по умолчанию не ограничен, 
      но можно настроить размер памяти),
      где хранится мета-информация:
   
      ● данные о классе (имя, методы, поля и др.)
   
      ● константы
2) Подготовка классов к выполнению:

   ● проверка, что код валиден

   ● подготовка примитивов в статических
   полях

   ● связывание ссылок на другие классы
3) Инициализация:
   выполняются инициализаторы static
   и инициализаторы полей static
4)     * //1, 2, 3, 4, 5, 6, 7 - Код выполняется строка за строкой, последовательность выполнения кода

5) Garbage Collection: сборка мусора
   Периодически собирает объекты из памяти
   (хипа), которые больше не используются