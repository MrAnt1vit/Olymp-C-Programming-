# V Set. Map. And other. Hints.

> Некоторые задачи, возможно, были разобраны ранее.
> 

---

### Задача А

![image.png](V%20Set%20Map%20And%20other%201182e807d74f806fbc37d1b0e093e556/image.png)

```cpp
#include <iostream>
#include <set>

using namespace std;

void solve() {
    set<int> s;
    int n;
    while (cin >> n) {
        s.insert(n);
    }
    cout << s.size() << '\n';
}

int main() {
    solve();
    return 0;
}
```

- Если мы добавим все элементы в сет, то повторные просто не добавятся, а значит его размер будет в точности равен числу уникальных элементов.

---

### Задача B

![image.png](V%20Set%20Map%20And%20other%20Hints%2011a2e807d74f80278bd5c2b6d850a349/image.png)

- Будем идти последоватльно по числам, добавляя их в сет.
- Соответственно на момент обработки очередного числа в сете буду только числа до него, а значит можно легко узнать, был элемент ранее или нет

```cpp
#include <iostream>
#include <set>
using namespace std;

void solve() {
    set<int> s;
    int x;
    while (cin >> x) {
        if (s.contains(x)) cout << "YES\n";
        else cout << "NO\n";
        s.insert(x);
    }
}

int main() {
    solve();
    return 0;
}
```

---

### Задача C

![image.png](V%20Set%20Map%20And%20other%201182e807d74f806fbc37d1b0e093e556/image%201.png)

```cpp
#include <iostream>
#include <set>
#include <vector>
#include <map>

using namespace std;

void solve() {
    map<string, int> cnt;

    string str;
    while (cin >> str) {
        cnt[str]++;
    }

    string res=cnt.begin()->first;
//    string res="";
    for (auto it: cnt) {
        if (it.second > cnt[res]) res=it.first;
    }
    cout << res;
}

int main() {
    solve();
    return 0;
}
```

- Учтем, что перебирая ключи в парах (или сами пары), то они будут идти в порядке возрастания.
- Также рекомендую вспомнить что такое for(auto … : …) {}

---

### Задача D

![image.png](V%20Set%20Map%20And%20other%20Hints%2011a2e807d74f80278bd5c2b6d850a349/image%201.png)

- Задача слабо отличается от предыдущей, просто здесь нужно во время подсчета выводить номер, сколько раз оно уже встретилось, а не сделать что-либо уже после подсчета

```cpp
#include <iostream>
#include <map>
 
using namespace std;
 
void solve() {
    map<string, int> mp;
    string x;
    while (cin >> x) {
        cout << mp[x] << ' ';
        mp[x]++;
    }
}
 
int main() {
    solve();
    return 0;
}
```

---

### Задача E

![image.png](V%20Set%20Map%20And%20other%20Hints%2011a2e807d74f80278bd5c2b6d850a349/image%202.png)

- В действительности смысл задачи вывести чаты в обратном порядке, пропуская те чаты, которые уже были выведены.
- Собственно давайте это сделаем
- *Можно для наглядности использовать stack, но я буду писать вектор

```cpp
#include <iostream>
#include <set>
#include <vector>
#include <algorithm>
using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<string> vec(n);
    for (int i = 0; i < n; i++) {
        cin >> vec[i];
    }

    set<string> s;
    reverse(vec.begin(), vec.end());
    for (auto& i: vec) {
        if (!s.contains(i)) {
            cout << i << '\n';
        }
        s.insert(i);
    }
}

int main() {
    solve();
    return 0;
}
```

---

### Задача G

![image.png](V%20Set%20Map%20And%20other%20Hints%2011a2e807d74f80278bd5c2b6d850a349/image%203.png)

- Задача разбиралась на занятии, по сути надо построить обратный словарь.
- Сделаем map, в котором ключами будут слова из **итогового** словаря, а значением будет вектор из слов, которые ему соответствуют.
- Важно правильно считать данные. Для этого воспользуемся тем, что после каждого слова идет запятая, а после последнего в строке - нет.

```cpp
#include <iostream>
#include <set>
#include <vector>
#include <map>
#include <algorithm>
 
using namespace std;
 
void solve() {
    int n;
    cin >> n;
    string key, value;
    map<string, vector<string>> mp;
 
    // 3
    // apple - malum, pomum, popula
    // fruit - baca, bacca, popum
    // punishment - malum, multa
 
    // 7
    // baca - fruit
    // bacca - fruit
    // malum - apple, punishment
    // multa - punishment
    // pomum - apple
    // popula - apple
    // popum - fruit
    for (int i = 0; i < n; i++) {
        cin >> key;
        cin >> value; // "-"
 
        cin >> value;
        while (value.back() == ',') {
            value.pop_back();
            mp[value].push_back(key);
            cin >> value;
        }
        mp[value].push_back(key);
    }
 
    cout << mp.size() << '\n';
    for (auto i: mp) {
        // i = pair<string, vector<string>>
        cout << i.first << " - ";
        vector<string>& values = i.second;
        sort(values.begin(), values.end());
        for (int j = 0; j < values.size() - 1; j++) {
            cout << values[j] << ", ";
        }
        cout << values.back() << '\n';
    }
}
 
int main() {
    solve();
    return 0;
}
```

---

### Задача I

![image.png](V%20Set%20Map%20And%20other%20Hints%2011a2e807d74f80278bd5c2b6d850a349/image%204.png)

- Буквально сделем симметрическую разность. Сделаем два множества, затем пройдем по одному из них, выбирая из них те, что не встречаются во втором.
- Повторим операцию для второго множества и выведем результат.
- Асимптотика O((n + m) log (n + m))

```cpp
#include <iostream>
#include <set>
#include <vector>
#include <algorithm>
using namespace std;

void solve() {
    int n, m, x;
    cin >> n;
    set<int> s1, s2;
    for (int i = 0; i < n; i++) {
        cin >> x;
        s1.insert(x);
    }
    cin >> m;
    for (int i = 0; i < m; i++) {
        cin >> x;
        s2.insert(x);
    }
    set<int> res;
    for (auto i: s1) {
        if (!s2.contains(i))
            res.insert(i);
    }
    for (auto i: s2) {
        if (!s1.contains(i))
            res.insert(i);
    }
    cout << res.size() << '\n';
    for (auto i: res) {
        cout << i << ' ';
    }
}

int main() {
    solve();
    return 0;
}
```

---

### Задача J

![image.png](V%20Set%20Map%20And%20other%20Hints%2011a2e807d74f80278bd5c2b6d850a349/image%205.png)

- Найдем элементы, которые находятся сразу в двух множествах, а потом выкинем их из каждого по отдельности.

```cpp
#include <iostream>
#include <set>
#include <vector>
#include <algorithm>
using namespace std;

void solve() {
    int n, m, x;
    cin >> n >> m;
    set<int> s1, s2;
    for (int i = 0; i < n; i++) {
        cin >> x;
        s1.insert(x);
    }
    for (int i = 0; i < m; i++) {
        cin >> x;
        s2.insert(x);
    }
    set<int> both;
    for (auto i : s1) {
        if (s2.contains(i)) {
            both.insert(i);
        }
    }
    for (auto i : both) {
        s1.erase(i);
        s2.erase(i);
    }

    cout << both.size() << '\n';
    for (auto i : both)
        cout << i << ' ';
    cout << '\n' << s1.size() << '\n';
    for (auto i : s1)
        cout << i << ' ';
    cout << '\n' << s2.size() << '\n';
    for (auto i: s2)
        cout << i << ' ';
}

int main() {
    solve();
    return 0;
}
```

---

### Ending

$© \text { Created by Anton Vitiuk 10.2024}$

[telegram](https://t.me/MrAnt1vit) [vk](https://vk.com/mrant1vit)