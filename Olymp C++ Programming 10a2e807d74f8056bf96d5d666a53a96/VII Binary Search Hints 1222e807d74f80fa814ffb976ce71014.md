# VII Binary Search. Hints.

---

### C. Приближенный двоичный поиск

![image.png](VII%20Binary%20Search%20Hints%201222e807d74f80fa814ffb976ce71014/image.png)

> С помощью алгоритма бинарного поиска найдём наибольшее число в массиве, не превосходящее данное. Затем, если индекс найденного числа — не последний, то проверим, какую разницу с данным числом имеет следующее число в массиве, и выведем нужное
> 

```cpp
#include <iostream>
#include <vector>

using namespace std;

int bin_search(vector<int>& vec, int left, int right, int num) {
    if (right - left <= 1) {
        return left;
    }

    int mid = (left + right) / 2;
    if (vec[mid] > num) {
        return bin_search(vec, left, mid, num);
    }
    return bin_search(vec, mid, right, num);
}

void solve() {
    int n;
    int k;
    cin >> n >> k;

    vector<int> first(n);
    for (int i = 0; i < n; ++i) {
        cin >> first[i];
    }

    for (int i = 0; i < k; ++i) {
        int tmp;
        cin >> tmp;
        int index = bin_search(first, 0, n, tmp);

        if (index == n - 1) {
            cout << first[index] << '\n';
            continue;
        }

        cout << (tmp - first[index] <= first[index + 1] - tmp ?
                 first[index] : first[index + 1]) << '\n';
    }
}

int main() {
    solve();

    return 0;
}
```

- В данном коде приведён пример реализации рекурсивного бинарного поиска. Вместо него можно написать более привычный вариант. Просто замените функцию $bin\_search$:

```cpp
int bin_search(vector<int>& vec, int size, int num) {
    int left = 0;
    int right = size;

    while (right - left > 1) {
        int mid = (left + right) / 2;
        if (vec[mid] > num) {
            right = mid;
        } else {
            left = mid;
        }
    }
    
    return left;
}
```

---

### G. Площадь

![image.png](VII%20Binary%20Search%20Hints%201222e807d74f80fa814ffb976ce71014/image%201.png)

> С помощью бинарного поиска найдём наибольшую возможную ширину дорожки. Для этого создадим функцию $count$, которая просчитает в зависимости от рассматриваемой нами ширины дорожки необходимое количество плитки. В бинарном поиске будем вызывать эту функцию (функция $count$ монотонно возрастает)
> 

```cpp
#include <iostream>

using namespace std;

long long count(long long num, pair<long long, long long> size) {
    return size.first * size.second -
           (size.first - 2 * num) * (size.second - 2 * num);
}

int bin_search(int left, int right, long long num, pair<int, int> size) {
    if (right - left <= 1) {
        return left;
    }

    int mid = (left + right) / 2;
    if (count(mid, size) > num) {
        return bin_search(left, mid, num, size);
    }
    return bin_search(mid, right, num, size);
}

void solve() {
    int n;
    int m;
    long long t;
    cin >> n >> m >> t;

    cout << bin_search(0, (min(n, m) + 1) / 2, t, {n, m});
}

int main() {
    solve();

    return 0;
}
```

- Бинарный поиск аналогично [задаче](VII%20Binary%20Search%20Hints%201222e807d74f80fa814ffb976ce71014.md) $C$ выполнен рекурсивно
- Также замечу, что чтобы не мучаться с кастами из $int$ в $long\ long$ в функции $count$ (иначе при умножении $n$ и $m$ возможно переполнение) просто сделаем так, чтобы функция изначально принимала $long\ long$ (произойдёт неявное преобразование типов). Также можно передавать пару по ссылке (для небольшой экономии памяти, но тогда ее нужно будет создать перед вызовом функции (это связано с работой языка, не вникайте сильно) и сделать ее от двух $long\ long$. *C++ не может сделать ссылку на пару $int$’ов от пары $long\ long$’ов.

```cpp
int bin_search(int left, int right, long long num, pair<long long, long long>& size);

void solve() {
    int n;
    int m;
    long long t;
    cin >> n >> m >> t;
    pair<long long, long long> pr(n, m);

    cout << bin_search(0, (min(n, m) + 1) / 2, t, pr);
    // cout << bin_search(0, (min(n, m) + 1) / 2, t, {n, m}); - CE
}
```

---

### I. Коровы — в стойла

![image.png](VII%20Binary%20Search%20Hints%201222e807d74f80fa814ffb976ce71014/image%202.png)

> С помощью бинарного поиска найдём подходящее нам расстояние между коровами. Для этого создадим функцию $count$, которая просчитает в зависимости от рассматриваемого расстояния определит, сколько коров мы сможем разместить с условием, что между ними будет расстояние не меньше рассматриваемого. В бинарном поиске будем вызывать эту функцию (функция $count$ монотонно невозрастающая)
> 

```cpp
#include <iostream>
#include <vector>

using namespace std;

int count(vector<int>& vec, int dist, int len) {
    int amount = 1;

    int last = 0;
    for (int i = 1; i < len; ++i) {
        if (vec[i] - vec[last] >= dist) {
            last = i;
            ++amount;
        }
    }

    return amount;
}

int bin_search(int left, int right, long long num, vector<int>& vec, int len) {
    if (right - left <= 1) {
        return left;
    }

    int mid = (left + right) / 2;
    if (count(vec, mid, len) < num) {
        return bin_search(left, mid, num, vec, len);
    }
    return bin_search(mid, right, num, vec, len);
}

void solve() {
    int n;
    int k;
    cin >> n >> k;

    vector<int> vec(n);
    for (int i = 0; i < n; ++i) {
        cin >> vec[i];
    }

    cout << bin_search(1, 1e9, k, vec, n);
}

int main() {
    solve();

    return 0;
}
```

---

### J. Вырубка леса (без решения)

![image.png](VII%20Binary%20Search%20Hints%201222e807d74f80fa814ffb976ce71014/image%203.png)

> Для начала сожмём это огромное условие, чтобы стало проще решать — выкинем всё ненужное.
По сути задача требует от нас найти некое минимальное $\alpha$, такое что
> 

$$
(\alpha A- \lfloor\frac{\alpha}{K}\rfloor A) + (\alpha B- \lfloor\frac{\alpha}{M}\rfloor B)=A(\alpha-\lfloor\frac{\alpha}{K}\rfloor)+B(\alpha-\lfloor\frac{\alpha}{M}\rfloor)\geq X
$$

> Поймём теперь, откуда взялась эта формула (а самое главное — что за треш такой я написал**😅**). Разберём только левую часть, переход в равенстве очевиден. Рассмотрим первую скобку — вторая получается аналогично. $\alpha A$ — количество вырубленных деревьев Дмитрием за $\alpha$ дней. $\lfloor\frac{\alpha}{K}\rfloor$ — количество дней, в которые Дмитрий не работал, поэтому домножив эту величину на $A$ и вычев это из $\alpha A$, мы получим количество деревьев, вырубленных Дмитрием за $\alpha$ дней. Также заметим, что функция, вычисляющая данное выражение — монотонно возрастает, поэтому мы можем применить алгоритм бинарного поиска по ответу
> 

---

### K. Про таблицу умножения (без решения)

![image.png](VII%20Binary%20Search%20Hints%201222e807d74f80fa814ffb976ce71014/image%204.png)

> Для начала поймём, на каком месте по величине в таблице будет некоторое заданное $1\leq\alpha\leq nm$. Вам может прийти в голову, что в данной задаче аналогично предыдущим существует какой-то способ найти позицию $\alpha$ за $O(1)$. Но на самом деле нам ничего не мешает сделать это за $O(n)$ (понять нам это может ограничение $n, m\leq 5\cdot10^5$ — из этого следует, что по времени вполне пройдёт решение за $O((n) \log (nm))$).
> 

$$
\newcommand\rfrac[2]{{}_{#1}\!\backslash^{#2}}
\begin{array}{ c|c|c|c|c|c|c|c|c| c | c | c | c | c |}
 \rfrac{n}{m}&{1}&2&3&4&5&6&7&8&9&10&11&12&13&\cdots \\
\hline
  1&\red1&\red2&\red3&\red4&\red5&\red6&\red7&\red8&\red9&\red{10}&\red{11}&\bold{12}&13&\cdots \\
\hline
  2&\red2&\red4&\red6&\red8&\red{10}&\bold{12}&14&16&18&20&22&24&\cdots \\
\hline
  3&\red3&\red6&\red9&\bold{12}&15&18&21&24&27&30&33&\cdots \\
\hline
  4&\red4&\red8&\bold{12}&16&20&24&28&32&36&40&\cdots \\
\hline
  5&\red5&\red{10}&15&20&25&30&35&40&45&\cdots \\
\hline
  6&\red6&\bold{12}&18&24&30&36&42&48&\cdots \\
\hline
  7&\red7&14&21&28&35&42&49&\cdots\\
\hline
  8&\red8&16&24&32&40&48&\cdots\\
\hline
  \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots &  \ddots
\end{array}
$$

> Для примера рассмотрим $n=8,m=12,\alpha=12$. Красным выделены числа меньшие $\alpha$, жирным выделены числа равные $\alpha$. Что мы можем заметить? Рассмотрим некоторое число $1\leq\phi\leq n$. В строчке $\phi$ количество красных чисел равно $min(\lfloor\frac{\alpha}{\phi}\rfloor, m)$*. В случае, если $\alpha$
⋮$\phi$, то количество красных будет на 1 меньше. Таким образом количество красных чисел, то есть чисел меньших $\alpha$, будет $\sum_{\phi=1}^{n}(min(\lfloor\frac{\alpha}{\phi}\rfloor,m)-(\alpha\bmod\phi==0))$, ну а количество жирно выделенных чисел — $\sum_{\phi=1}^{n}(\alpha\bmod\phi==0)$. То есть мы для любого $\alpha$ за $O(n)$ можем определить диапазон, в котором лежит наше $\alpha$. Заметим также, что данная функция монотонно неубывающая$\implies$воспользуемся алгоритмом бинарного поиска по ответу
> 

* Мы используем $min$, так как иначе мы можем рассмотреть числа большие $m$ (в примере, если бы мы проходились по $m$, то в первом же столбце количество красных чисел по формуле было бы $11$, но на самом деле их всего $8$. Кстати, проходиться по $n$ или $m$ — не имеет абсолютно никакой разницы, то есть во всех формулах $n$ и $m$ можно поменять местами)

---

### Ending

$© \text { Created by Simon Khasanov 10.2024}$

[telegram](https://t.me/the_semen1) [vk](https://vk.com/1thesemen)