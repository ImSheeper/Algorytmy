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
