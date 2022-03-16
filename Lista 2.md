# Zadanie 1
```cpp
#include <iostream>
#include <ctime>

using namespace std;

int main() {

    srand(time(0));

    int N = 5;
    int* a = new int;
    double* d = new double;
    float* f = new float;
    int *tab = new int[N];

    *a = rand() % 10;
    cout << *a << endl;

    *d = rand() % 10;
    cout << *d << endl;

    *f = rand() % 10;
    cout << *f << endl;

    for (int i = 0; i < N; i++) {
        tab[i] = rand()%10;
        std::cout << tab[i] << ", ";
    }

    delete(a);
    delete(d);
    delete(f);
    delete(tab);

    return 0;
}
```
# Zadanie 2
```cpp
#include <iostream>
#include <ctime>

using namespace std;

typedef struct {
    int id;
    char imie[20];
    char nazwisko[20];
    int wiek;
} tosoba;

void func(tosoba *tab, int n) {

    for (int i = 0; i < n; i++) {
        tab[i].id = i;
        sprintf(tab[i].imie,"%s %i","sdsada",i);
        sprintf(tab[i].nazwisko,"%s %i","dafsdasd",i);
        tab[i].wiek = rand()%50;
        cout << tab[i].imie << ", "<< tab[i].nazwisko << ", " << tab[i].wiek << endl;
    }
}

int main() {

    srand(time(0));

    int n;
    cout << "podaj N" << endl;
    cin >> n;

    auto *tab = new tosoba[n];
    func(tab,n);
    delete(tab);

    return 0;
}
```
# Zadanie 3

# Zadanie 4

# Zadanie 5
