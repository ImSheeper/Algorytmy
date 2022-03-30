```cpp
#include <iostream>

using namespace std;

struct osoba {
    string imie;
    string nazwisko;
    string pesel;
    int wiek;

    osoba *next;
    osoba();
};

osoba::osoba() {
    next = 0;
}

struct lista {

    osoba *pocz;

    void dodaj();
    void wyswietl();
    void usun(int nr);

    lista();
};

lista::lista() {
    pocz = 0;
}

void lista::dodaj() {

    osoba *nowa = new osoba;

    cout << "Imie" << endl;
    cin >> nowa -> imie;
    cout << "Nazwisko" << endl;
    cin >> nowa -> nazwisko;
    cout << "PESEL" << endl;
    cin >> nowa -> pesel;
    cout << "wiek" << endl;
    cin >> nowa -> wiek;

    if(pocz == 0) pocz = nowa;

    else {
        osoba *temp = pocz;
        while(temp -> next) temp = temp -> next;

        temp -> next = nowa;
        nowa -> next = 0;
    }
}

void lista::wyswietl() {

    osoba *temp = pocz;
    int id = 1;

    while(temp) {
        cout << "Id: " <<  id << endl;
        cout << "Imie: " <<  temp -> imie << endl;
        cout << "nazwisko: " << temp -> nazwisko << endl;
        cout << "PESEL: " << temp -> pesel << endl;
        cout << "wiek: " << temp -> wiek << endl << endl;

        temp = temp -> next;
        id++;
    }
}

void lista::usun(int nr) {

    if(nr == 1) {
        osoba *temp = pocz;
        pocz = temp -> next;
        delete temp;
    }
    else if(nr >= 2) {
        int j = 1;

        osoba *temp = pocz;

        while(temp) {
            if(j + 1 == nr) break;
            temp = temp -> next;
            j++;
        }

        if(temp -> next -> next == 0) {
            delete temp -> next;
            temp -> next = 0;
        }
        else {
            osoba *usuwana = temp -> next;
            temp -> next = temp -> next -> next;
            delete usuwana;
        }
    }
}

int main() {

    int wyb, id;
    lista *baza = new lista;

    do {

        cout << "Menu" << endl;
        cout << "1.Dodaj osobe" << endl;
        cout << "2.Usun element" << endl;
        cout << "3.Wyswietl stos" << endl;
        cout << "4.Plik" << endl;
        cout << "5.Wyjscie" << endl;
        cin >> wyb;

        switch (wyb) {
            case 1: {
                baza -> dodaj();
                break;
            }

            case 2: {
                cout << "Podaj id do usuniecia" << endl;
                cin >> id;
                baza -> usun(id);
                break;
            }

            case 3: {
                baza -> wyswietl();
            }
        }
    }while(wyb != 5);

    return 0;
}
```
