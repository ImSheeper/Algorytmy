# Zadanie 1 
```c++
#include <iostream>

using namespace std;

int sil(int n) {

    if(n == 0) return 1;
    else return n * sil(n - 1);

}

int main()
{
    int x;

    m:cout << "Podaj n" << endl;
    cin >> x;

    if (x > 10 || x < 0) {
        cout << "Bledna liczba" << endl;
        goto m;
    }

    cout << "silnia z " << x << " wynosi " << sil(x) << endl;

    return 0;
}
```
# Zadanie 2 (NIE DZIAŁA)
```c++
#include <iostream>

using namespace std;

void rys(int n) {

    int x = n;

    if(n > 0) {
        if(x > 0) {
            cout << "*";
            rys(n - 1);
        }
        cout << endl;
    }
}

int main() {

    int n;

    cout << "Podaj n" << endl;
    cin >> n;

    rys(n);

    return 0;
}
```
# Zadanie 3
```c++
#include <iostream>

using namespace std;

int fib(int n, int x = 0, int y = 1) {

    if(n == 0) return 0;

    int suma = x + y;
    cout << suma << ", ";
    fib(n - 1, suma, x);
}

int main() {

    int n;

    cout << "Podaj n" << endl;
    cin >> n;

    fib(n);

    return 0;
}
```

# Zadanie 4
```cpp
#include <iostream>
#include <ctime>

using namespace std;

int ele(int tab[], int max = 0, int i = 0) {

    if(i == 50) return max;
    if(max < tab[i]) {
        max = tab[i];
    }
    ele(tab, max, i+1);
}

int main() {

    int tab[50];

    srand(time(NULL));

    for(int i = 0; i < 50; i++) {
        tab[i] = rand() % 101;
        cout << tab[i] << ", ";
    }

    cout << endl << "Najwieksza liczba to: " << ele(tab);

    return 0;
}
```
# Zadanie 5
```cpp
#include <iostream>

using namespace std;

int euk(int a, int b) {

    if(a == b) return a;

    if(a > b) a -= b;
    else b -= a;

    euk(a, b);
}

int main() {

    int a, b;

    cout << "Podaj a: " << endl;
    cin >> a;
    cout << "Podaj b: " << endl;
    cin >> b;

    cout << euk(a, b);

    return 0;
}
```
# Zadanie 6
