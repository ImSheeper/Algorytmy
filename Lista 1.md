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

# Zadanie 5

# Zadanie 6
