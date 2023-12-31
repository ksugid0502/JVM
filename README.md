# JVM
```
public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
```

1. В стеке в фрейме main создается примитив int i со значением 1  
2. Загружается класс Object (сначала ищется в кэше ClassLoader-ов, если не находитсся, то выполняется загрузка),  
   далее выполняется инициализация объекта o, выделяется память в куче и ссылка на объект кладется в стек в фрейм main  
3. Аналогично загружается класс Integer, создается объект ii в куче и ссылка на объект кладется в стек в фрейм main  
4. При вызове метода printAll создается новый фрейм printAll в стеке,  
   далее в этот фрейм кладутся ссылка o, примитив i, ссылка ii  
5. Загружается класс Integer, создается объект ii в куче и ссылка на объект кладется в стек в фрейм printAll  
6. При вызове метода System.out.println загружается класс System, в процессе загрузки инициализируются все статические поля и методы,
   в том числе out, далее создается новый фрейм в стеке, куда кладется ссылка на передаваемую строку, после исполнения метода фрейм удаляется  
7. После выхода из метода printAll удаляется соответствующий фрейм и следовательно теряется ссылка на объект uselessVar,
   он становится недостижимым объектом и сборщик мусора при работе удалит этот объект из кучи,
   далее загрузка класса System уже не происходит, т.к. он уже был загружен, вызывается метод println, создается новый фрейм,
   в который кладется ссылка на передаваемую строку "finished", после исполнения метода фрейм удаляется
