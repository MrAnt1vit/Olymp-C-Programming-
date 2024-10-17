# II Advanced C++. Hints.

---

# Разбор задач + Хинты

### А. Восстановние трех чисел

![image.png](II%20Advanced%20C++%20Hints%201102e807d74f805eb573d031efe1bf03/image.png)

- Так как числа натуральные, то сумма всех трех чисел, очевидно, будет больше суммы любых двух. Поэтому мы однозначно можем определить какое  число является максимумом.
- А зная какое число максимум можно узнать и сами числа. Если есть сумма трех и сумма двух чисел, то можно найти одно из них. Разве что мы не сможем узнать порядок, но в задаче он нам и не нужен по сути.

```cpp
#include <iostream>

using namespace std;

int main() {
    int a, b, c, d;
    cin >> a >> b >> c >> d;
    if (a < b) swap(a, b);
    // now a >= b
    
    if (a < c) swap(a, c);
    // now a >= c && a >= b
    
    if (a < d) swap(a, d);
    // now a >= d && a >= c && a >= b
    
    cout << a - b << ' ' << a - c << ' ' << a - d << '\n';
    return 0;
}
```

- Чтобы не перебирать все случаи максимума, можно просто поменять числа так, чтобы в a лежал максимум.

---

### B. Катя и сборы

![image.png](II%20Advanced%20C++%20Hints%201102e807d74f805eb573d031efe1bf03/image%201.png)

- Кода не будет ы
- Во-первых обратите внимание, что у вас в реальность n+1 футболка и m+1 джинсы.
- Дальше поймем, что если у нас х футболок и у джинс, то всего вариантов у нас xy. Так как для каждой футоблки есть у вариантов джинс. Тогда всего вариантов y + y + … + y (x раз). То есть в точности xy.

---

### C. Трусливые ладьи

![image.png](II%20Advanced%20C++%20Hints%201102e807d74f805eb573d031efe1bf03/image%202.png)

- Попробуйте нарисовать разные ситауции и заметьте, что в реальности расположение ладей нас не очень интересует.
- Получается чтобы решить задачу нам надо знать лишь числа n и m.

---

### D. Игра с браслетиками

![image.png](II%20Advanced%20C++%20Hints%201102e807d74f805eb573d031efe1bf03/image%203.png)

- Посмотрим на некоторые первые числа:
    - K  = 1 … 3. Очевидно, ответ 1 т.к. можно выиграть за 1 ход.
    - K = 4. После любого хода можно выиграть за 2го игрока, значит ответ 2.
    - K = 5 … 7. Либо можно порисовать и заметить, что ответ везде 1, либо можно это доказать: можно оставить 4 штуки, после которых как бы ни походил второй игрок мы побеждаем.
    - K = 8. Можно понять, что ответ 2 из похожих соображений.
- Опять же, можно понять, что для всех K кратных 4 ответ 2 и 1 для остальных
    - В частности, если K не делится на 4 то можно оставить число кратное 4. После хода соперника число однозначно не будет делится на 4. Тогда мы вновь оставим кратное 4.
    - Таким образом всегда будем оставлять число кратное 4, а значит в итоге останется 0.

---

### E. Делаем срезы

![image.png](II%20Advanced%20C++%20Hints%201102e807d74f805eb573d031efe1bf03/image%204.png)

```cpp
#include <iostream>

using namespace std;

int main(){
    string str;
    cin >> str;
    int n = str.size();
    
    // Третий символ
    cout << str[2] << '\n';
    
    // Второй с конца символ
    cout << str[n - 2] << '\n';
    
    // Первые 5 символов
    for (int i = 0; i < 5; i++)
        cout << str[i];
    cout << '\n';
    
    // Все символы, кроме двух последних
    for (int i = 0; i < n - 2; i++) 
        cout << str[i];
    cout << '\n';
    
    // Все четные символы
    for (int i = 0; i < n; i += 2)
        cout << str[i];
    cout << '\n';
    
    // Все нечетные символы
    for (int i = 1; i < n; i += 2)
        cout << str[i];
    cout << '\n';
    
    // Все символы в обратном порядке
    for (int i = n - 1; i >= 0; i--)
        cout << str[i];
    cout << '\n';
    
    // Все символы в обратном порядке через одного
    for (int i = n - 1; i >= 0; i -= 2)
        cout << str[i];
    cout << '\n';
    
    // Размер строки
    cout << n << '\n';
}
```

---

### F. Задача Бахгольда

![image.png](II%20Advanced%20C++%20Hints%201102e807d74f805eb573d031efe1bf03/image%205.png)

- Единственный хинт: очевидно, минимальное простое число 2, значит любое четное число можно представить в виде n/2 двоек. (6 = 2+2+2, 16 = 8 двоек и тд… )

---

### Оставшиеся задачи

- G. МаксМинА. Задача выглядит тяжело и непонятно, но попробуйте разобраться и разобрать примеры) Само по себе решение очень простое.
- H. Котлеты. Код тоже не сложный, но осознать его не просто. Самое сложное - понять второй пример + выяснить, как из него следует общее решение.
- Остальные, можно сказать, со звездочкой. Их я могу обсудить и подсказать в телеге (даже если у вас нет идей, но вы хотите решить)

---

# Разбор решений

### Сокращаем код

```cpp
//
// Created by Anton Vitiuk on 27.09.2024.
//

#include <iostream>

using namespace std;

int main() {
    int a, b, c, w, x, y, z;
    cin >> x >> y >> z >> w;
    if (w > x && w > y && w > z) {
        a = w - x;
        b = w - y;
        c = w - z;
    }
    else if (y > x && y > z && y > w) {
        a = y - x;
        b = y - w;
        c = y - z;
    }
    else if (z > x && z > y && z > w) {
        a = z - x;
        b = z - w;
        c = z - y;
    }
    else {
        a = x - y;
        b = x - w;
        c = x - z;
    }
    cout << a << " " << b << " " << c;
    return 0;
}
```

```cpp
//
// Created by Anton Vitiuk on 27.09.2024.
//

#include <iostream>

using namespace std;

int main() {
    int a, b, c, w, x, y, z;
    cin >> x >> y >> z >> w;
    if (w < x) swap(x, w);
    if (w < y) swap(y, w);
    if (w < z) swap(z, w);
    
    cout << w - x << " " << w - y;
    cout << " " << w - z;
    return 0;
}
```

- Мы видим, что в реальности мы выводим из максимума остальные числа, поэтому можно сделать в разы короче

---

### Поговорим о кодстайле

```cpp
#include <iostream>
using namespace std;

int main()
{
    int k,m,n;
    cin>>k>>m>>n;
    m++;
    n++;
    if (k <= n*m) {
        cout<<"YES"<<endl;
    } else { cout<<"NO"<<endl;}
}

```

```cpp
#include <iostream>

using namespace std;

int main() {
    int k, m, n;
    cin >> k >> m >> n;
    m++;
    n++;
    if (k <= n * m)
        cout << "YES" << '\n';
    else 
        cout << "NO" << '\n';
}
```

- Элементарно писать код по кодстайлу и с хорошим визуалом не сложно, но это сильно поможет в будущем не нахватать проблем, да и вообще понятнее самим будет.

---

### Формулы лучше условий

```cpp
#include <iostream>
#include <cmath>

using namespace std;
int main() {
    int x;
    cin >> x;
    // 1st
    x % 5 == 0 ? cout << x/5 : cout << x/5+1;
    // 2nd
    cout << ceil(x / 5.0);
    return 0;
}

```

```cpp
#include <iostream>
#include <cmath>

using namespace std;
int main() {
    int x;
    cin >> x;
    cout << (x + 4) / 5;
    return 0;
}
```

- Разбор формулы есть в прошлых листках + на лекции

---

### Упрощаем сложный код

```cpp
#include <iostream>
#include <cmath>
#include <string>

using namespace std;

int main() {
    int n;
    cin >> n;
    cout << n / 2 << endl;
    if (n == 3) {
        cout << 3;
        return 0;
    }
    else {
        cout << 2;
    }
    if (n % 2 == 0) {
        for (int i = 1; i < n / 2; i++) {
            cout << " " << 2;
        }
    }
    else {
        for (int i = 1; i < (n - 3) / 2; i++) {
            cout << " " << 2;
        }
        cout << " " << 3;
    }
    return 0;
}
```

```cpp
#include <iostream>

using namespace std;

int main() {
    int n;
    cin >> n;
    cout << n / 2 << '\n';
    
    if (n % 2) {
        n -= 3;
        cout << 3 << ' ';
    }
    for (int i = 0; i < n; i+=2)
        cout << 2 << ' ';
    return 0;
}
```

- Подробный разбор был на занятии, не трудно убедится (даже без задачи), что эти коды выполняют одно и тоже.

---

### Немного про асиптотику

```cpp
#include <iostream>
using namespace std;
int main() {
    int end;
    cin >> end;

    int k = 0;
    while (end > 0)
    {
        if (end >= 5)
        {
            end -= 5;
        }
        else
        {
            int maxk = min(end, 5);
            end -= maxk;
        }
        k++;
    }

    cout << k << endl;

    return 0;
}
```

```cpp
#include <iostream>
using namespace std;
int main() {
    int end;
    cin >> end;

    cout << (end + 4) / 5 << '\n';

    return 0;
}
```

- Код работал за O(n), а теперь работает за O(1).
- Разбор подробнее в конце лекции.

---

$© \text { Created by Anton Vitiuk 09.2024}$

[telegram](https://t.me/MrAnt1vit) [vk](https://vk.com/mrant1vit)