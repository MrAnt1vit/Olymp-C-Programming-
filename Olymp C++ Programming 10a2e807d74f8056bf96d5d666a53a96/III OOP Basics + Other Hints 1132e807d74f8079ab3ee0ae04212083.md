# III OOP Basics + Other. Hints.

---

# Контест: Продолжение + ООП

### A. Номер самого большого числа

![image.png](III%20OOP%20Basics%20+%20Other%20Hints%201132e807d74f8079ab3ee0ae04212083/image.png)

```cpp
#include <iostream>
#include <vector>

using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    int ind = 0;
    for (int i = 0; i < n; i++) {
        if (a[i] >= a[ind]) ind = i;
    }
    cout << ind + 1;
}

int main() {
    solve();
    return 0;
}
```

- Запоминаем индекс и двигаем его, если находим нужный элемент.
- *Для самостоятельного разбора можете написать код, которые еще и работает за O(1) памяти.

---

### B. Между офисами

![image.png](III%20OOP%20Basics%20+%20Other%20Hints%201132e807d74f8079ab3ee0ae04212083/image%201.png)

![image.png](III%20OOP%20Basics%20+%20Other%20Hints%201132e807d74f8079ab3ee0ae04212083/image%202.png)

- Можно просто посчитать буквально то, что требуется в задаче, но такой код я не буду показывать (можете в лс спросить).
- Здесь можно заметить, что по сути в задаче есть 4 случая:
    - Первый символ S, последний символ S. Тогда из каждой поездки в F он когда то вернулся. А значит число полетов в одну сторону такое же, как число полетов в другую.
    - Первый символ F, последний F. В целом аналогично, поэтому тут тоже ответ NO.
    - Первый S, последний F. Также можно понять, что мы один раз прилетели в F, а дальше всегда возвращались. Значит полетов SF было на 1 больше, чем FS.
    - Первый F, последний S. Аналогично, полетов FS было на 1 больше, чем SF.
- Также для экономии памяти (за O(1)) считаем только первый и последний символы.

```cpp
#include <iostream>
using namespace std;

void solve() {
    char first, last;
    int n;
    cin >> n;
    cin >> first;
    last = first;
    for (int i = 1; i < n; i++) cin >> last;
    if (first == 'S' && last == 'F') cout << "YES";
    else cout << "NO";
}

int main() {
    solve();
    return 0;
}
```

---

### C. Плохие цены

![image.png](III%20OOP%20Basics%20+%20Other%20Hints%201132e807d74f8079ab3ee0ae04212083/image%203.png)

- Вспомним про наши любимые мультитесты. =)
- Выделим самое главное из условия: элемент плохой, если после него есть элемент меньше него.
- Тогда можем идти с конца массива и поддерживать актуальный минмиум (другими словами минимум на суффиксе).
- Заведем счетчик для ответа. Тогда если элемент меньше минмиума, то мы просто обновляем минимум, а если нет, то наш ответ увеличивается на 1.

```cpp
#include <iostream>
#include <vector>
using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<int> a(n);
    for (int i = 0; i < n; i++)
        cin >> a[i];

    int mn = a[n - 1], res = 0;
    for (int i = n - 1; i >= 0; i--) {
        if (a[i] <= mn) mn = a[i];
        else res++;
    }
    cout << res << '\n';
}

int main() {
    int t;
    cin >> t;
    while (t--)
        solve();
    return 0;
}
```

---

### D. Маленькая пони и пещера кристаллов

- Просто задача на реализацию. Надо грамотно расписать циклы и не запутаться в инексах.

```cpp
#include <iostream>
using namespace std;

void solve() {
    int n;
    cin >> n;
    for (int i = 0, j = 1; i < n / 2; i++, j++) {
        for (int k = 0; k < n / 2 - j + 1; k++) cout << '*';
        for (int k = 1; k < 2 * j; k++) cout << 'D';
        for (int k = 0; k < n / 2 - j + 1; k++) cout << '*';
        cout << '\n';
    }
    for (int i = 0; i < n; i++) cout << 'D';
    cout << '\n';
    for (int i = 0, j = n / 2; i < n / 2; i++, j--) {
        for (int k = 0; k < n / 2 - j + 1; k++) cout << '*';
        for (int k = 1; k < 2 * j; k++) cout << 'D';
        for (int k = 0; k < n / 2 - j + 1; k++) cout << '*';
        cout << '\n';
    }
}

int main() {
    solve();
    return 0;
}
```

---

### E. Соседние замены

![image.png](III%20OOP%20Basics%20+%20Other%20Hints%201132e807d74f8079ab3ee0ae04212083/image%204.png)

![image.png](III%20OOP%20Basics%20+%20Other%20Hints%201132e807d74f8079ab3ee0ae04212083/image%205.png)

- Рассмотрим каждое число по отдельности.
    - Если оно четное, то на некотором шаге просто уменьшится на 1
    - Если же оно нечетное, то оно сначала увеличится на 1, а потом сразу же уменьшится обратно.
- Получаем, что нечетные числа останутся без изменений, в четные уменьшатся на один.

---

### F. Любовный треугольник

![image.png](III%20OOP%20Basics%20+%20Other%20Hints%201132e807d74f8079ab3ee0ae04212083/image%206.png)

- Уменьшим элементы на 1, чтобы решать задачу в 0-индексации
- Тогда мы должны вывести YES, если есть некоторый индекс, что a[a[i]]=i. Значит можно просто их перебрать и проверить это условия.

---

### G. Вася и шахматы

![image.png](III%20OOP%20Basics%20+%20Other%20Hints%201132e807d74f8079ab3ee0ae04212083/image%207.png)

![image.png](III%20OOP%20Basics%20+%20Other%20Hints%201132e807d74f8079ab3ee0ae04212083/image%208.png)

- Попробуйте делать ходы симметрично (выберите какого-нибудь игрока и попробуйте за него так поиграть).
- Допустим, если n - нечетное, то попробуйте играть за черных таким образом и попробуйте найти закономерность.

---

### H. I Wanna Be the Guy

![image.png](III%20OOP%20Basics%20+%20Other%20Hints%201132e807d74f8079ab3ee0ae04212083/image%209.png)

- Надо проверить, что среди чисел все встречаются хотя бы 1 раз.
- Для этого можно просто сделать вектор размера n и отметить там все элемнты, которые есть во входных данных.

---

# Контест: Стек, Дэк, Очередь.

### A. Проверка работ

![image.png](III%20OOP%20Basics%20+%20Other%20Hints%201132e807d74f8079ab3ee0ae04212083/image%2010.png)

- По сути стопка тетрадей это и есть дек. Поэтому по сути надо просто сделать то, что написано в условии, а потом вспомнить что у дека ест индексация.

```cpp
#include <iostream>
#include <deque>
using namespace std;

void solve() {
    int n;
    cin >> n;
    deque<string> d;
    for (int i = 0 ; i < n; i++) {
        string name, type;
        cin >> name >> type;
        if (type == "top") d.push_front(name);
        else d.push_back(name);
    }
    int q;
    cin >> q;
    for (int i = 0 ; i < q; i++) {
        int num;
        cin >> num;
        cout << d[num - 1] << '\n';
    }
}

int main() {
    solve();
    return 0;
}
```

---

### B. Игра в пьяницу

![image.png](III%20OOP%20Basics%20+%20Other%20Hints%201132e807d74f8079ab3ee0ae04212083/image%2011.png)

- По сути надо просто написать то, что написано в условии, используя две очереди)

---

# Ending

$© \text { Created by Anton Vitiuk 10.2024}$

[telegram](https://t.me/MrAnt1vit) [vk](https://vk.com/mrant1vit)