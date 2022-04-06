# Zadanie 1
```cpp
#include <iostream>
#include <string>
#include <sstream>
#include <vector>
#include <fstream>

using namespace std;

struct lista {
    string imie;
    string nazwisko;
    string PESEL;
    int wiek;
    lista* pop;
    lista* nast;
    lista();
};

lista::lista() {
    pop = nullptr;
    nast = nullptr;
}

void dodaj(lista* pocz, lista* koniec) {

    lista* nowy = new lista;

    cout << "podaj imie" << endl;
    cin >> nowy->imie;
    cout << "podaj nazwisko" << endl;
    cin >> nowy->nazwisko;
    cout << "podaj PESEL" << endl;
    cin >> nowy->PESEL;
    cout << "podaj wiek" << endl;
    cin >> nowy->wiek;

    if (pocz->imie == "") {
        *pocz = *nowy;
        *koniec = *nowy;
    }

    else {
        lista* temp = pocz;
        while (temp->nast) {
            temp = temp->nast;
        }
        nowy->pop = temp;
        temp->nast = nowy;
        *koniec = *temp->nast;
    }

}

void usun(lista* pocz, lista* koniec, string nazwisko, bool all = false) {

    lista* temp = pocz;

    while (temp) {
        if (temp->nazwisko == nazwisko) {
            if (temp->pop != nullptr)
                temp->pop->nast = temp->nast;
            if (temp->nast != nullptr)
                temp->nast->pop = temp->pop;
            if (!temp->nast)
                *koniec = *temp->pop;
            if (!temp->pop)
                *pocz = *temp->nast;
            if (!all)
                break;
        }
        temp = temp->nast;
    }
}

void wyszukaj(lista* pocz) {

    cout << "Po czym chcesz wyszukac\n1.imie,\n2.nazwisko\n3.PESEL\n4.wiek" << endl;
    int a;
    cin >> a;
    bool imie = false, nazwisko = false, PESEL = false, wiek = false;
    switch (a) {
    case 1:
        imie = true;
        break;
    case 2:
        nazwisko = true;
        break;
    case 3:
        PESEL = true;
        break;
    case 4:
        wiek = true;
        break;
    default:
        cout << "wybrales zly nr" << endl;
        return;
    }

    cout << "wpisz fraze" << endl;
    string fraza;
    cin >> fraza;
    lista* temp = pocz;

    for (int i = 0; temp; i++) {
        if (imie) {
            if (temp->imie == fraza) {
                cout << i << ". " << "imie: " << temp->imie << " nazwisko: " << temp->nazwisko << " PESEL: " << temp->PESEL << endl;
            }
        }

        if (nazwisko) {
            if (temp->nazwisko == fraza){
                cout << i << ". " << "imie: " << temp->imie << " nazwisko: " << temp->nazwisko << " PESEL: " << temp->PESEL << endl;
            }
        }

        if (PESEL) {
            if (temp->PESEL == fraza) {
                cout << i << ". " << "imie: " << temp->imie << " nazwisko: " << temp->nazwisko << " PESEL: " << temp->PESEL << endl;
            }
        }

        if (wiek) {
            stringstream ss;
            ss << temp->wiek;
            if (ss.str() == fraza) {
                cout << i << ". " << "imie: " << temp->imie << " nazwisko: " << temp->nazwisko << " PESEL: " << temp->PESEL << endl;
            }
        }
        temp = temp->nast;
    }
}

void show(lista* pocz) {

    lista* temp = pocz;
    for (int i = 1; temp; i++) {
        cout << i << ". " << "imie: " << temp->imie << " nazwisko: " << temp->nazwisko << " PESEL: " << temp->PESEL << endl;
        temp = temp->nast;
    }
}

void edit(lista* pocz) {

    show(pocz);
    cout << "ktory numer chcesz edytowac?" << endl;
    int a;
    cin >> a;
    lista* temp = pocz;

    for (int i = 0; i < a - 1; ++i) {
        temp = temp->nast;
    }

    cout << "podaj imie" << endl;
    cin >> temp->imie;
    cout << "podaj nazwisko" << endl;
    cin >> temp->nazwisko;
    cout << "podaj PESEL" << endl;
    cin >> temp->PESEL;
    cout << "podaj wiek" << endl;
    cin >> temp->wiek;
}
void save(lista* pocz) {

    lista* temp = pocz;
    vector<lista> array;
    while (temp) {
        array.push_back(*temp);
        temp = temp->nast;
    }

    string save;
    for (int j = 0; j < array.size(); j++) {
        save += array[j].imie;
        save += " ";
        save += array[j].nazwisko;
        save += " ";
        save += array[j].PESEL;
        save += " ";
        stringstream ss;
        ss << array[j].wiek;
        save += ss.str();
        save += "\n";
    }

    ofstream zapis("dane.txt");
    zapis << save;
    zapis.close();
}

int main() {

    lista* pocz = new lista;
    lista* koniec = new lista;

    while (true) {
        cout << "wybierz:\n";
        cout << "1.dodaj element\n";
        cout << "2.usun z listy 1 element o podanym nazwisku \n";
        cout << "3.usun wszystkie elementy o podanym nazwisku\n";
        cout << "4.wyszukaj elementy z listy na podstawie klucza\n";
        cout << "5.wyswietl\n";
        cout << "6.edytuj wybrany element listy\n";
        cout << "7.zapisz do pliku\n";
        cout << "8.wyjdz\n";
        cout << "wybierz: ";
        int a;
        cin >> a;
        string data;
        switch (a) {
        case 1:
            dodaj(pocz, koniec);
            break;
        case 2:
            cout << "podaj nazwisko" << endl;
            cin >> data;
            usun(pocz, koniec, data);
            break;
        case 3:
            cout << "podaj nazwisko" << endl;
            cin >> data;
            usun(pocz, koniec, data, true);
            break;
        case 4:
            wyszukaj(pocz);
            break;
        case 5:
            show(pocz);
            break;
        case 6:
            edit(pocz);
            break;
        case 7:
            save(pocz);
            break;
        case 8:
            delete(koniec);
            delete(pocz);
            return EXIT_SUCCESS;
        default:
            cout << "wybrales zly nr" << endl;
            break;
        }
    }
}
```
