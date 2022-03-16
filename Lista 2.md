# Zadanie 1
```cpp
#include <iostream>
#include <time.h>

using namespace std;

int main() {

    int tab[10];
    int c;

    srand(time(NULL));

    for (int i = 0; i < 10; i++) {
        tab[i] = rand() % 100 + 1;
        cout << tab[i] << ", ";
        c = *( & (tab[0]) + i);
        int &ref = c;
        cout << "Element: " << ref << ", Adres tego elementu to: " << (&(tab[0]) + i) << endl;
    }

    return 0;
}
```
# Zadanie 2
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
# Zadanie 3
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
# Zadanie 4
```cpp
#include <iostream>
#include <ctime>
#include <vector>

using namespace std;

bool spr(int index, vector<int> indexy) {

    for(int i=0; i<indexy.size(); i++) {
        if(index == indexy[i])
            return false;
    }

    return true;
}

int main() {

    srand(time(NULL));

    int tab[100];
    int* wsk = new int[100];
    vector<int> indexy;
    int idx;

    for (int i = 0; i < 100; i++) {
        tab[i] = rand() % 100;
    }

    for (int k = 0; k < 100; k++) {
        int* a = new int;
        *a = 101;
        for (int j = 0; j < 100; j++) {
            if(*a > tab[j] && spr(j,indexy)) {
                a = reinterpret_cast<int *>(tab[j]);
                idx = j;
            }
        }

        indexy.push_back(idx);
        wsk[k] = reinterpret_cast<int>(a);
        delete a;
    }

    tab[5] = 999;

    for (int l = 0; l < 100; l++) {
        cout << tab[l] << ", ";
    }

    cout << endl;

    for (int l = 0; l < 100; l++) {
        cout << wsk[l] << ", ";
    }

    delete wsk;

    return 0;
}
```
# Zadanie 5
```cpp
#include<iostream>
#include<cstdlib>
#include<iomanip>
#include<algorithm>
#include <vector>

using namespace std;

struct tosoba {
    int id;
    char imie[20];
    char nazwisko[20];
    int wiek;
    bool operator < (const tosoba &x)const {
        return nazwisko>x.nazwisko;
    }
};

vector<tosoba> sort(vector<tosoba> tab) {

    int size = tab.size();
    tosoba array[size];

    for (int i = 0; i < size; i++) {
        array[i] = tab[i];
    }

    sort(array,array+size);
    tab.clear();

    for (int j = 0; j < size; j++) {
        tab.push_back(array[j]);
    }

    return tab;
}

tosoba add(int idx) {
    tosoba person;
    person.id = idx;

    cout << "podaj imie" << endl;
    cin >> person.imie;

    cout << "podaj nazwisko" << endl;
    cin >> person.nazwisko;

    cout << "podaj wiek" << endl;
    cin >> person.wiek;

    return person;
}

int showname(vector<tosoba> tab, string nazwisko) {

    for (int i = 0; i < tab.size(); i++) {
        if(tab[i].nazwisko == nazwisko) {
            return i;
        }
    }
    return -1;
}

void show(vector<tosoba> tab) {

    for (int i = 0; i < tab.size(); i++) {
        cout << "imie: " << tab[i].imie << endl << "nazwisko: " <<tab[i].nazwisko << endl << "wiek: " << tab[i].wiek << endl;
    }
}

int main() {

    vector<tosoba> tab;
    int a;

    tosoba person;
    int index;
    string name;
    for (;;) {
        cout<<"1.dodaj\n2.szukaj wedlug nazwiska\n3.sortuj wedlug nazwiska\n4.wyswietl\n5.usun\n6.wyjscie"<<endl;
        cin >> a;

        switch (a) {
            case 1:
                person = add(tab.size());
                tab.push_back(person);
                break;

            case 2:
                cout <<"podaj nazwisko" << endl;
                cin >> name;
                index = showname(tab,name);
                if(index == -1) {
                    cout << "nie ma takiej osoby" << endl;
                    break;
                }
                cout << tab[index].imie << endl;
                cout << tab[index].nazwisko << endl;
                cout << tab[index].wiek << endl;
                break;

            case 3:
                tab = sort(tab);
                break;

            case 4:
                show(tab);
                break;

            case 5:
                cout << "podaj index osoby do usnuniecia" << endl;
                cin >> a;
                tab.erase(tab.begin()+a+1);
                break;

            case 6:
                return EXIT_SUCCESS;
        }
    }
}
```
