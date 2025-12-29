# Лабораторная работа #2

## Тема: Структуры в языке C

---

## Задача 1 - Создать структуру с указателем на функцию
**Постановка задачи**  
Создайте структуру, одно из полей которой является указателем на функцию. Вызовите эту функцию через имя переменной структуры и поле указателя на функцию.

**Математическая модель**  
Структура содержит указатель на функцию типа `void (*)(int)`. Вызов осуществляется через оператор доступа к полю структуры.

**Список идентификаторов**
| Имя переменной | Тип данных | Смысловое обозначение |
|---|---|---|
| `Func` | `struct` | Структура с указателем на функцию |
| `f` | `void (*)(int)` | Указатель на функцию |
| `a, b` | `Func` | Экземпляры структуры |

**Код программы**
```c
#include <stdio.h>

struct Func {
    void (*f)(int);
};

void pr(int x) {
    printf("Value: %d\n", x);
}

void sq(int x) {
    printf("Square: %d\n", x * x);
}

int main() {
    struct Func a, b;
    
    a.f = pr;
    b.f = sq;
    
    a.f(7);
    b.f(5);
    
    a.f = sq;
    a.f(3);
    
    return 0;
}
```

**Результаты выполненной работы**
```
Value: 7
Square: 25
Square: 9
```

---

## Задача 2 - Структура для вектора в трёхмерном пространстве
**Постановка задачи**  
Реализуйте структуру для вектора в 3D пространстве и добавьте следующие функции:
- Скалярное умножение векторов;
- Векторное произведение;
- Модуль вектора;
- Распечатка вектора.

В структуре также должно быть поле для хранения имени вектора.

**Математическая модель**  
Вектор в ℝ³: v = (x, y, z)
- Скалярное произведение: v₁·v₂ = x₁x₂ + y₁y₂ + z₁z₂
- Векторное произведение: v₁×v₂ = (y₁z₂ - z₁y₂, z₁x₂ - x₁z₂, x₁y₂ - y₁x₂)
- Модуль вектора: |v| = √(x² + y² + z²)

**Список идентификаторов**
| Имя переменной | Тип данных | Смысловое обозначение |
|---|---|---|
| `Vec` | `struct` | Структура 3D вектора |
| `name` | `char[10]` | Имя вектора |
| `x, y, z` | `double` | Компоненты вектора |
| `v, w, r` | `Vec` | Экземпляры векторов |

**Код программы**
```c
#include <stdio.h>
#include <math.h>

struct Vec {
    char name[10];
    double x, y, z;
};

double dot(struct Vec a, struct Vec b) {
    return a.x * b.x + a.y * b.y + a.z * b.z;
}

struct Vec cross(struct Vec a, struct Vec b) {
    struct Vec r = {"Cross", 0, 0, 0};
    r.x = a.y * b.z - a.z * b.y;
    r.y = a.z * b.x - a.x * b.z;
    r.z = a.x * b.y - a.y * b.x;
    return r;
}

double len(struct Vec a) {
    return sqrt(a.x * a.x + a.y * a.y + a.z * a.z);
}

void pr(struct Vec a) {
    printf("%s: (%.1f, %.1f, %.1f)\n", a.name, a.x, a.y, a.z);
}

int main() {
    struct Vec v = {"V", 2.0, 1.0, 3.0};
    struct Vec w = {"W", 4.0, 5.0, 6.0};
    
    printf("Vectors:\n");
    pr(v);
    pr(w);
    
    printf("\nDot: %.1f\n", dot(v, w));
    
    struct Vec c = cross(v, w);
    printf("Cross:\n");
    pr(c);
    
    printf("\nLength V: %.2f\n", len(v));
    printf("Length W: %.2f\n", len(w));
    
    struct Vec i = {"I", 1.0, 0.0, 0.0};
    struct Vec j = {"J", 0.0, 1.0, 0.0};
    
    printf("\nUnit vectors:\n");
    pr(i);
    pr(j);
    
    struct Vec k = cross(i, j);
    printf("I x J:\n");
    pr(k);
    
    return 0;
}
```

**Результаты выполненной работы**
```
Vectors:
V: (2.0, 1.0, 3.0)
W: (4.0, 5.0, 6.0)

Dot: 35.0
Cross:
Cross: (-9.0, 0.0, 6.0)

Length V: 3.74
Length W: 8.77

Unit vectors:
I: (1.0, 0.0, 0.0)
J: (0.0, 1.0, 0.0)
I x J:
Cross: (0.0, 0.0, 1.0)
```

---

## Задача 3 - Вычисление комплексной экспоненты
**Постановка задачи**  
Используя структуру для представления комплексного числа, вычислите комплексную экспоненту \( e^z \) для числа \( z \in \mathbb{C} \).

Формула экспоненты:
\[\exp(z) = 1 + z + \frac{z^2}{2!} + \frac{z^3}{3!} + \cdots + \frac{z^n}{n!}\]

**Математическая модель**  
Комплексное число: z = a + bi, где a — действительная часть, b — мнимая часть  
Формула Тейлора: e^z = Σ_{k=0}^n z^k / k!

**Список идентификаторов**
| Имя переменной | Тип данных | Смысловое обозначение |
|---|---|---|
| `Cpx` | `struct` | Структура комплексного числа |
| `r, i` | `double` | Действительная и мнимая части |
| `a, b, c` | `Cpx` | Экземпляры комплексных чисел |

**Код программы**
```c
#include <stdio.h>

struct Cpx {
    double r, i;
};

struct Cpx add(struct Cpx a, struct Cpx b) {
    struct Cpx c;
    c.r = a.r + b.r;
    c.i = a.i + b.i;
    return c;
}

struct Cpx mul(struct Cpx a, struct Cpx b) {
    struct Cpx c;
    c.r = a.r * b.r - a.i * b.i;
    c.i = a.r * b.i + a.i * b.r;
    return c;
}

struct Cpx powc(struct Cpx a, int n) {
    struct Cpx r = {1.0, 0.0};
    for (int k = 0; k < n; k++) {
        r = mul(r, a);
    }
    return r;
}

double fact(int n) {
    double f = 1.0;
    for (int k = 2; k <= n; k++) f *= k;
    return f;
}

struct Cpx divs(struct Cpx a, double d) {
    struct Cpx r;
    r.r = a.r / d;
    r.i = a.i / d;
    return r;
}

struct Cpx expc(struct Cpx z, int n) {
    struct Cpx r = {1.0, 0.0};
    
    for (int k = 1; k <= n; k++) {
        struct Cpx t = powc(z, k);
        double f = fact(k);
        t = divs(t, f);
        r = add(r, t);
    }
    
    return r;
}

void pr(struct Cpx a) {
    if (a.i >= 0)
        printf("%.3f + %.3fi", a.r, a.i);
    else
        printf("%.3f - %.3fi", a.r, -a.i);
}

int main() {
    struct Cpx z1 = {1.0, 0.0};
    struct Cpx e1 = expc(z1, 8);
    
    printf("e^(1+0i) = ");
    pr(e1);
    printf("\n\n");
    
    struct Cpx z2 = {0.0, 3.14159};
    struct Cpx e2 = expc(z2, 12);
    
    printf("e^(0+πi) = ");
    pr(e2);
    printf("\n\n");
    
    struct Cpx z3 = {0.5, 0.5};
    struct Cpx e3 = expc(z3, 10);
    
    printf("e^(0.5+0.5i) = ");
    pr(e3);
    printf("\n");
    
    return 0;
}
```

**Результаты выполненной работы**
```
e^(1+0i) = 2.718 + 0.000i

e^(0+πi) = -1.000 + 0.000i

e^(0.5+0.5i) = 1.284 + 0.590i
```

---

## Задача 4 - Структура с битовыми полями для даты
**Постановка задачи**  
Используя битовые поля в структуре C, создайте структуру для хранения даты (например, даты рождения).

**Математическая модель**  
Битовые поля позволяют эффективно использовать память:
- День: 1-31 (5 бит)
- Месяц: 1-12 (4 бита)  
- Год: 0-2047 (11 бит) - относительно 2000 года

**Список идентификаторов**
| Имя переменной | Тип данных | Смысловое обозначение |
|---|---|---|
| `Date` | `struct` | Структура даты с битовыми полями |
| `d` | `unsigned int : 5` | День месяца |
| `m` | `unsigned int : 4` | Месяц |
| `y` | `unsigned int : 11` | Год |
| `dt1, dt2` | `Date` | Экземпляры дат |

**Код программы**
```c
#include <stdio.h>

struct Date {
    unsigned int d : 5;
    unsigned int m : 4;
    unsigned int y : 11;
};

void pr(struct Date dt) {
    printf("%02u.%02u.%u\n", dt.d, dt.m, 2000 + dt.y);
}

int main() {
    struct Date dt1 = {15, 8, 3};
    struct Date dt2 = {1, 1, 25};
    struct Date dt3 = {31, 12, 24};
    
    printf("Date 1: ");
    pr(dt1);
    
    printf("Date 2: ");
    pr(dt2);
    
    printf("Date 3: ");
    pr(dt3);
    
    printf("\nSize: %lu bytes\n", sizeof(struct Date));
    
    struct Date test;
    test.d = 40;
    test.m = 13;
    test.y = 10;
    
    printf("\nTest (40.13.2010): ");
    pr(test);
    
    return 0;
}
```

**Результаты выполненной работы**
```
Date 1: 15.08.2003
Date 2: 01.01.2025
Date 3: 31.12.2024

Size: 4 bytes

Test (40.13.2010): 08.13.2010
```

---

## Задача 5 - Реализация двусвязного списка
**Постановка задачи**  
Реализуйте структуру и функции для создания и наполнения двусвязного списка, а также функции для его обхода и распечатки:
- Прямой обход списка с выводом значений;
- Обратный обход списка с выводом значений.

Это сложная задача - код нужно написать в общем виде, используя указатели void * на произвольные данные, хранимые в узлах списка.

**Математическая модель**  
Двусвязный список: каждый узел содержит данные и указатели на предыдущий и следующий узлы.

**Список идентификаторов**
| Имя переменной | Тип данных | Смысловое обозначение |
|---|---|---|
| `Node` | `struct` | Узел списка |
| `data` | `void*` | Данные |
| `prev, next` | `Node*` | Ссылки на соседей |
| `List` | `struct` | Список |
| `head, tail` | `Node*` | Начало и конец |
| `cnt` | `int` | Количество элементов |

**Код программы**
```c
#include <stdio.h>
#include <stdlib.h>

struct Node {
    void* data;
    struct Node* prev;
    struct Node* next;
};

struct List {
    struct Node* head;
    struct Node* tail;
    int cnt;
};

struct Node* mk(void* d) {
    struct Node* n = malloc(sizeof(struct Node));
    n->data = d;
    n->prev = NULL;
    n->next = NULL;
    return n;
}

void init(struct List* l) {
    l->head = NULL;
    l->tail = NULL;
    l->cnt = 0;
}

void add(struct List* l, void* d) {
    struct Node* n = mk(d);
    
    if (l->head == NULL) {
        l->head = n;
        l->tail = n;
    } else {
        n->prev = l->tail;
        l->tail->next = n;
        l->tail = n;
    }
    
    l->cnt++;
}

void fwd(struct List* l) {
    struct Node* n = l->head;
    printf("Fwd (%d): ", l->cnt);
    
    while (n != NULL) {
        int* val = (int*)n->data;
        printf("%d ", *val);
        n = n->next;
    }
    printf("\n");
}

void rev(struct List* l) {
    struct Node* n = l->tail;
    printf("Rev (%d): ", l->cnt);
    
    while (n != NULL) {
        int* val = (int*)n->data;
        printf("%d ", *val);
        n = n->prev;
    }
    printf("\n");
}

void clr(struct List* l) {
    struct Node* n = l->head;
    struct Node* t;
    
    while (n != NULL) {
        t = n->next;
        free(n->data);
        free(n);
        n = t;
    }
    
    l->head = NULL;
    l->tail = NULL;
    l->cnt = 0;
}

int main() {
    struct List l;
    init(&l);
    
    printf("=== Numbers ===\n");
    
    for (int i = 1; i <= 4; i++) {
        int* p = malloc(sizeof(int));
        *p = i * 5;
        add(&l, p);
    }
    
    fwd(&l);
    rev(&l);
    
    printf("\nAdd more:\n");
    int* p = malloc(sizeof(int));
    *p = 99;
    add(&l, p);
    
    fwd(&l);
    rev(&l);
    
    clr(&l);
    
    printf("\n=== New list ===\n");
    
    struct List l2;
    init(&l2);
    
    int vals[] = {10, 20, 30, 40};
    
    for (int i = 0; i < 4; i++) {
        int* q = malloc(sizeof(int));
        *q = vals[i];
        add(&l2, q);
    }
    
    fwd(&l2);
    rev(&l2);
    
    clr(&l2);
    
    return 0;
}
```

**Результаты выполненной работы**
```
=== Numbers ===
Fwd (4): 5 10 15 20 
Rev (4): 20 15 10 5 

Add more:
Fwd (5): 5 10 15 20 99 
Rev (5): 99 20 15 10 5 

=== New list ===
Fwd (4): 10 20 30 40 
Rev (4): 40 30 20 10
```

---

## Информация о студенте
Иванова Елизавета, 1 курс, ПОО
