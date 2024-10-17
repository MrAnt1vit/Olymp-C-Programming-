# IV Stack. Deque. Queue. Hints.

---

# Stack. Deque. Queue.

### B. Игра в пьяницу

![image.png](IV%20Stack%20Deque%20Queue%20Hints%201182e807d74f80b28264ed6e47d4f5bb/image.png)

- По сути надо просто реализовать то, что написано в условии =)

```cpp
#include <iostream>
#include <queue>

using namespace std;

void solve() {
    queue<int> a, b;
    int x;
    for (int i = 0; i < 5; i++) {
        cin >> x;
        a.push(x);
    }
    for (int i = 0; i < 5; i++) {
        cin >> x;
        b.push(x);
    }

    for (int i = 0; i < 1000000; i++) {
        if (a.front() == 0 && b.front() == 9) {
            a.push(a.front());
            a.push(b.front());
        } else if (a.front() == 9 && b.front() == 0) {
            b.push(a.front());
            b.push(b.front());
        } else if (a.front() > b.front()) {
            a.push(a.front());
            a.push(b.front());
        } else {
            b.push(a.front());
            b.push(b.front());
        }
        a.pop();
        b.pop();
        if (a.empty()) {
            cout << "second " << i + 1;
            return;
        } else if (b.empty()) {
            cout << "first " << i + 1;
            return;
        }
    }
    cout << "botva";
}

int main() {
    solve();
    return 0;
}
```

---

### C. Сортировка вагонов

![image.png](IV%20Stack%20Deque%20Queue%20Hints%201182e807d74f80b28264ed6e47d4f5bb/image%201.png)

![image.png](IV%20Stack%20Deque%20Queue%20Hints%201182e807d74f80b28264ed6e47d4f5bb/image%202.png)

- Идея в том, что рассматрвиая вагоны последовательно, будем перевозить либо верхний вагон из тупика (если он “следюущий”), либо класть в тупик очередной вагон из последовательности.
- Можете доказать, что если в таком порядке не получится отсортированная последовательность, то это в принципе невозможно.
- Есть похожий способ перевозки вагонов, который также гарантирует, что либо мы смогли отсортировать, либо это в принципе невозможно.

```cpp
#include <iostream>
#include <stack>
#include <vector>

using namespace std;

void solve() {
    stack<int> way;
    vector<int> res;
    int n;
    cin >> n;
    vector<pair<int, int>> steps;

    int now = 1;
    for (int i = 0; i < n; i++) {
        while (!way.empty() && way.top() == now) {
            res.push_back(way.top());
            way.pop();
            steps.push_back({2, 1});
            now++;
        }
        int x;
        cin >> x;
        way.push(x);
        steps.push_back({1, 1});
    }
    while (!way.empty() && way.top() == now) {
        res.push_back(way.top());
        way.pop();
        steps.push_back({2, 1});
        now++;
    }

    if (now != n + 1) {
        cout << 0;
        return;
    }
    for (auto i: steps) {
        cout << i.first << ' ' << i.second << '\n';
    }
}

int main() {
    solve();
    return 0;
}
```

---

### D. Валера и дек

![image.png](IV%20Stack%20Deque%20Queue%20Hints%201182e807d74f80b28264ed6e47d4f5bb/image%203.png)

- Заметим, что выполнить все m операций у нас не получится (даже просто один раз выполнить все операции и запомнить ответы не выйдет), т.к. число слишком большое.
- Но при этом, можно понять, что после какого-то момента на первом месте будет максимальный элемент, а остальные будут двигаться “по кругу”.
- Значит можно выполнить n операций вручную (и запомнить для них ответ), а для последующих выводить соответственно максимум и некоторый элемент (индекс которого можно просто посчитать, по сути это $(m_j - n)\ \%\ (n - 1)$).

---

### I. Очередь в магазине

![image.png](IV%20Stack%20Deque%20Queue%20Hints%201182e807d74f80b28264ed6e47d4f5bb/image%204.png)

- Подробный разбор есть в конце лекции, здесь только небольшой хинт.
- В целом сделать просто операции, описанные в задаче можно при помощи обычной очереди, но это работает долго. (O(n^2)).
- Это можно обойти, используя некоторый “счетчик добвляемой агрессии”, а в самих людях хранить увеличение или уменьшение его соответсвтенно.

```cpp
#include <iostream>
#include <queue>

using namespace std;

void solve() {
    int n;
    cin >> n;
    deque<pair<long long, long long>> q;

    long long add = 0;

    for (int i = 0; i < n; i++) {
        int type;
        cin >> type;
        if (type == 1) {
            int x;
            cin >> x;
            q.push_back({x, 0});
        }
        if (type == 2) {
            int x, y;
            cin >> x >> y;
            q[0].first += x;
            q[0].second += y;      // -> y
            q.back().second -= y;  // -> -y
        }
        if (type == 3) {
            cout << q.front().first + add << '\n';
            add += q.front().second;
            q.pop_front();
        }
    }
}

int main() {
    solve();
    return 0;
}
```

---

# Set + Map.

### B. Встречалось ли число раньше

![image.png](IV%20Stack%20Deque%20Queue%20Hints%201182e807d74f80b28264ed6e47d4f5bb/image%205.png)

- По сути задача на реализацию и использование сета.

```cpp
#include <iostream>
#include <set>

using namespace std;

void solve() {
    set<int> s;
    int x;
    while (cin >> x) {
        if (s.contains(x))
            cout << "YES\n";
        else
            cout << "NO\n";
        s.insert(x);
    }
}

int main() {
    solve();
    return 0;
}
```

---

### D. Номер появления слова

![image.png](IV%20Stack%20Deque%20Queue%20Hints%201182e807d74f80b28264ed6e47d4f5bb/image%206.png)

- Аналогично задача на реализацию и использование map’a.

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

### G. Англо-латинский словарь

![image.png](IV%20Stack%20Deque%20Queue%20Hints%201182e807d74f80b28264ed6e47d4f5bb/image%207.png)

- Подробно разбиралось на занятии.
- По сути надо по словарю построить обратный.
- Рекомендую запустить у себя в дебаге и подробно пройти каждую строчку.

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

# Ending

$© \text { Created by Anton Vitiuk 10.2024}$

[telegram](https://t.me/MrAnt1vit) [vk](https://vk.com/mrant1vit)