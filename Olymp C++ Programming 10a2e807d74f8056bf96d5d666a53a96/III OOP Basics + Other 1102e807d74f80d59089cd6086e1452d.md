# III OOP Basics + Other

---

# Больше функций

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    int a = 5;
    int b = 6;

    cout << max(a, b) << '\n'; // 6
    cout << min(a, b) << '\n'; // 5

    max(1ll, a); // CE

    a = -6;
    cout << abs(a) << ' ' << abs(b) << '\n'; // 6 6

    swap(a, b); // a = 6, b = -6

    a = 0;
    cout << (a == 0 ? "YES" : "NO") << '\n'; // YES
    cout << (b == 0 ? "YES" : "NO") << '\n'; // NO

    // similar to:
    a == 0 ? cout << "YES\n" : cout << "NO\n"; // YES
    b == 0 ? cout << "YES\n" : cout << "NO\n"; // NO

    // similar to:
    if (a == 0)
        cout << "YES" << '\n';
    else
        cout << "NO" << '\n';
}
```

> max(a, b);
> 

Возвращает максимум из двух переменных.

> min(a, b);
> 

Возвращает минимум из двух переменных.

> max(1ll, b);
> 

Не могут сравнить элементы разных типов данных, поэтому иногда, например, стоит прописать (int) или другой тип.

*Строго говоря они лежат в <algorithm>, но иногда компилируется и без них

> abs(a); // 5 → 5; -5 → 5
> 

Получаем число по модулю.

> swap(a, b);
> 

Используется для смены значений переменных. (а переходит в б, и наоборот)

*Плюс swap в том, что многие типы обмениваются за единицу (например, векторы).

> a == 0 ? “YES” : “NO”;
> 

Тернарный оператор. Проще и короче if … else … . Возвращает/выполняет первое, если условие истинно и правое, если ложно.

---

# Структуры…

### Основное и конструкторы по умолчанию.

```cpp
#include <iostream>
#include <vector>

using namespace std;

struct Point {
    int x;
    int y;
};

int main() {
    Point a;
    Point b{1, 2};
    Point c(3, 4);
    Point d{1};
    Point e = {0, 1};
    
    cout << b.x << ' ' << b.y << '\n';
    b.x = b.y = 2;
    
    
    Point* ptr = &a;
    cout << ptr->x;
    return 0;
}
```

> struct Point {
    int x;
    int y;
};
> 

Обьявили самую простую структуру и два поля для нее. 

> Point a;
Point b{1, 2};
Point c(3, 4);
Point d{1};
Point e = {0, 1};
> 

Разные способы создания стркутуры.
Если не указывать аргументы, то они будут заданы по умолчанию. 
Аргументы можно указывать в круглых скобках или фигурных.
Если указать только 1 аргумент, то остальные будут заданы по умоллчанию.

> cout << b.x << ' ' << b.y << '\n';
b.x = b.y = 2;
> 

Так можно обращаться к полям структуры. Например, их можно вывести или изменить.

> Point* ptr = &a;
cout << ptr->x;
> 

Можно создать указатель на собственную структуру.
Через стрелочку можно также получать поля структуры.

---

### Конструкторы

Часто мы хотим указать поля некоторым нестандартным образом.

```cpp
#include <iostream>
#include <vector>

using namespace std;

struct Point {
    int x;
    int y;

    Point(int x, int y) {
        this->x = x;
        this->y = y;
    }

    Point(int x) {
        this->x = x;
        y = -x; // can use this->y, but not needed
    }

    Point() {
        x = y = 0;
    }

    // ?? but why not
    Point (long long x) {
        this->x = -x;
        y = x;
    }
};

int main() {
    Point a(1);
    cout << a.x << ' ' << a.y << '\n'; // 1 -1

    Point b;
    cout << b.x << ' ' << b.y << '\n'; // 0 0

    Point c(1ll);
    cout << c.x << ' ' << c.y << '\n'; // -1 1
    return 0;
}
```

> Point(int x, int y) {
    this->x = x;
    this->y = y;
}
> 

Конструктор от двух аргументов. Конкретно этот делает тоже самое, что и стандартный.

> this->x;
> 

Так как переменная x “затмевает” внутренний x, а получить доступ к полю хочется. Для этого можно использовать this. 

> Point() {
    x = y = 0;
}
> 

Конструктор без полей “занулит” все поля.

> Point(int x) {
    this->x = x;
    y = -x; // can use this->y, but not needed
}
> 

Конструктор от одного аргумента, для примера.

> Point (long long x) {
    this->x = -x;
    y = x;
}
> 

Нестандартный конструктор от другого типа. Просто для примера, что не обязательно везде писать int’ы)

---

### Методы

```cpp
#include <iostream>
#include <vector>
#include <cmath>

using namespace std;

struct Point {
    int x;
    int y;

    Point() {
        x = y = 0;
    }

    Point(int x) {
        this->x = x;
        y = 0;
    }

    Point(int x, int y) {
        this->x = x;
        this->y = y;
    }

    double dist() {
        return sqrt(x * x + y * y);
    }
    
    void add(int k) {
        x += k;
        y += k;
    }

    bool operator<(Point& other) {
        return this->dist() < other.dist();
    }
};

Point operator+(Point& a, Point& b) {
    Point res = a;
    res.x += b.x;
    res.y += b.y;
    return res;
}

int main() {
    Point a(1, 2);
    
    cout << a.dist() << '\n';
    a.add(2); // a = {3, 4};
    cout << a.x << ' ' << a.y << '\n'; // 3 4
    
    Point b(3, 4);
    Point c = a + b;
    cout << c.x << ' ' << c.y << '\n'; // 6 8
    return 0;
}
```

> double dist() {
    return sqrt(x * x + y * y);
}

a.dist(); → 2.23607
> 

Пример метода у структуры. Можем вызывать его и получить, например, расстояние до центра плоскости (в этом примере).

> void add(int k) {
    x += k;
    y += k;
}
> 

Метод с аргументом. Здесь, например, можно прибавить к координатам одно и тоже число.

> bool operator<(Point& other) {
    return this->dist() < other.dist();
}
> 

Операторы. Это пример оператора сравнения. Сравнивает две точки и одна меньше другой, если расстояние до центра меньше.

> Point operator+(Point& a, Point& b) {
    Point res = a;
    res.x += b.x;
    res.y += b.y;
    return res;
}
> 

Оператор +. Теперь мы можем сложить две точки. (его можно прописать и внутри структуры, но так приоритетнее, также работает и с оператором <).

---

### Сортировка и компараторы

```cpp
#include <iostream>
#include <vector>
#include <cmath>
#include <algorithm>

using namespace std;

struct Point {
    int x;
    int y;
    
    Point() {}
    
    Point(int x, int y) {
        this->x = x;
        this->y = y;
    }

    double dist() {
        return sqrt(x * x + y * y);
    }
    
    // 1st variant
    bool operator<(Point& other) {
        return this->dist() < other.dist();
    }
};

// 2nd variant
bool cmp(Point& p1, Point& p2) {
    return p1.dist() < p2.dist();
}

bool cmp1(int x, int y) {
    return x > y;
}

int main() {
    vector<int> v = {1, 2, 3, 4, 5, 6};
    sort(v.begin(), v.end(), cmp1);
    // reverse sort
    
    int n = 5;
    vector<Point> a(n);
    for (int i = 0; i < n; i++) {
        a[i].x = 5 - i;
        a[i].y = 6 - i;
    }
    sort(a.begin(), a.end());
    // sort using operator<
    sort(a.begin(), a.end(), cmp);
    // sort using comparator
    return 0;
}
```

> bool cmp1(int x, int y) {
    return x > y;
}

vector<int> v = {1, 2, 3, 4, 5, 6};
sort(v.begin(), v.end(), cmp1);
> 

Компаратор. Принимает два аргумента и возвращает true, если в итоговой сортировке первый аргумент должен стоять левее (false если они равны или больше, то есть либо не важно, либо правее).
Это пример сортировке в обратном порядке.

> sort(a.begin(), a.end());
> 

Сортировка по умолчанию сравнивает элементы через оператор <, если он есть (иначе - CE).

> bool cmp(Point& p1, Point& p2) {
    return p1.dist() < p2.dist();
}

sort(a.begin(), a.end(), cmp);
> 

Можно передать сввой компаратор (как в случае с числами).

---

# Асимптотика

### Базовое понимание

- Когда мы пишем задачи, у нас есть ограничение по времени и во время решения задач нам надо уметь понимать, будет ли работать код в заданых лимитах.
- Зная, что в C++ за секунду рабоатет примерно $3*10^8$ операций, можно в целом понять сколько их будет в худшем случае.
- Но считать их напрямую очень муторно и в какой-то степени глупо. Для этого можно пользоваться асимтотиками.
- Например, если в задаче есть только ввод/вывод цикла, то код отработает за O(n) (за “линию”). Значит выполнит порядка n операций. Ведь нам важно что мы выполняем n операций “примерно”, а то что в реальности их n + 10 не так критично.

### Семантика O(…)

- Считая асимтотику, мы игнорируем константы, то есть если выполняется фиксированно, допустим, 10 операций, то это все равно O(1).
- Соответственно, O(2 + 8) = O(1); O(2n) = O(n)
Аналогично, если у нас, например n + 2, то асимтотически это тоже самое, что n.

### “Смежная” асимптотика

- В задаче бывает больше одной переменной, тогда имеет смысл асмптотик, например,
O(n + m), O(nm) и тд
- Когда в крайних случаях код долго выполняется при одной переменной или при другой.
- Также такое бывает, когда код зависит от значений массива/вектора, который вводится.

### Небольшая таблица

| Асимтотика | Некоторые операции | Пример | Пояснение |
| --- | --- | --- | --- |
| O(1) | O(2) = O(1 + 3) = O(const) = O(1) | Задачи на сложеие/вычитание/формулу | Важно, что констанста может быть разной, но часто в таких задачх это не важно |
| O(log n) | O(log_3(n)) = O(log_2(n)) = O(log n) | Бинарный поиск, СНМ (потом будет) | Часто log n возникает в составе других асимптотик, а не сам по себе |
| O(sqrt(n)) | O (sqrt(n) + 2 + log n) =
O (sqrt n) | Проверка числа на простоту | Часто в задачах на математику (еще есть декомпозиции, но это потом…) |
| O(n) | O(2n) = O(n+1) = O(n) | Максимум в векторе, ввод массива, поиск максумума | Существенная часть алгоритмов работает за “линию” |
| O(n log n) | O(n) * O(log n) = O(n log n) | Быстрая сортировка, Дерево отрезков и др. | Большая часть алгоритмов и задач решается с такой асимптотикой (прям много) |
| O (n^2) | O(n^2 + 5n + 10) = O(n^2) | Сортировка вставками/выбором | Такое встречается чаще в динамическом программировании, но часто есть решение быстрее |

### Примеры

- O(1)

```cpp
#include <iostream>
using namespace std;

int main() {
    int end;
    cin >> end;
    cout << (end + 4) / 5;
    return 0;
}
```

- O(log n)

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    cin >> n;
    int i = 1, k = 0;
    while (i * 2 <= n) {
        i *= 2;
        k++;
    }
    cout << k;
    return 0;
}
```

- O(n log n)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> a(n);
    int s = 0;
    for (int i = 0; i < n; i++)
        cin >> a[i];
    sort(a.begin(), a.end());
    for (int i = 0; i < n; i++)
        cout << a[i] << ' ';
    return 0;
}
```

- O(sqrt(n))

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    cin >> n;
    bool fl = false;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) fl = true;
    }
    cout << (fl ? "YES" : "NO");
    return 0;
}
```

- O(n)

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> a(n);
    int s = 0;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
        s += a[i];
    }
    cout << s;
    return 0;
}
```

- O(n^2)

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n; cin >> n;
    vector<int> a(n);
    long long s = 0;
    for (int i = 0; i < n; i++)
        cin >> a[i];
    for (int i = 0; i < n; i++)
        for (int j = i + 1; j < n; j++)
            s += a[i] * a[j];
    cout << s;
    return 0;
}
```

---

# Ending

$© \text { Created by Anton Vitiuk 10.2024}$

[telegram](https://t.me/MrAnt1vit) [vk](https://vk.com/mrant1vit)