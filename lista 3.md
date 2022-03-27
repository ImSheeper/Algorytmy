```cpp
#include <iostream>

using namespace std;

typedef struct lista
{
    string imie;
    string nazwisko;
    string PESEL;
    int wiek;
    lista *pop;
} lista;

lista* pocz = nullptr;

lista* ostatni();

void wypiszOsoby() {
    lista* obecny = pocz;
    if(!obecny) return;
    do{
        //wypisywanie
        cout << "Imie: " <<obecny->imie << endl;
        cout << "Nazwisko: " <<obecny->nazwisko << endl;
        cout << "Pesel: " <<obecny->PESEL << endl;
        cout << "Wiek: " <<obecny->wiek << endl << endl;
        obecny = obecny->pop;
    } while(obecny);
}

void dodajOsobe(string imie, string nazwisko, string pesel, int wiek) {
    if(pocz) {
        lista* obecny = ostatni();
        obecny->pop = new lista{imie, nazwisko, pesel, wiek, nullptr};
        //cout << "Dodalem osobe nie na poczatek" << endl;
    } else {
        pocz = new lista{imie, nazwisko, pesel, wiek, nullptr};
        //cout << "Dodalem osobe na poczatek" << endl;
    }
}

lista* ostatni() {
    lista* obecny = pocz;
    while(obecny->pop) {
        obecny = obecny->pop;
    }
    return obecny;
}

lista* przedostatni() {
    lista* przed = pocz;
    if(!przed->pop) return przed;
    while(przed->pop->pop) {
        przed = przed->pop;
    }
    return przed;
}



void usunOsobe() {
    if(!pocz) return;

    if(!pocz->pop) {
        delete pocz;
        pocz = nullptr;
        return;
    }

    lista* przedost = przedostatni();
    delete przedost->pop;
    przedost->pop = nullptr;
}

void usunWszystkich() {
    while(pocz) {
        usunOsobe();
    }
}

int main()
{


    dodajOsobe("Kamil", "Ciebien", "456", 56);
    dodajOsobe("David", "Ali", "654", 100);
    dodajOsobe("Mateusz", "Kanarski", "123", 10);
    dodajOsobe("Adam", "Sandler", "12356", 110);
    usunOsobe();

    wypiszOsoby();




    usunWszystkich();
    return 0;
}
```
# moje podejście, nie działa
```
#include <iostream>

using namespace std;

struct lista {
    string imie;
    string nazwisko;
    string pesel;
    int wiek;
    lista *pop;
};

void dodaj(lista *wskaznik) {

    cout << "Imie" << endl;
    cin >> wskaznik -> imie;
    cout << "Nazwisko" << endl;
    cin >> wskaznik -> nazwisko;
    cout << "PESEL" << endl;
    cin >> wskaznik -> pesel;
    cout << "wiek" << endl;
    cin >> wskaznik -> wiek;

}

void wyswietl(lista *wskaznik) {

    cout << "Imie: " << wskaznik -> imie << endl;
    cout << "nazwisko: " << wskaznik -> nazwisko << endl;
    cout << "PESEL: " << wskaznik -> pesel << endl;
    cout << "wiek: " << wskaznik -> wiek << endl << endl;
}

int main() {

    int wyb;

    do {
        lista osoba;
        lista *wskaznik = &osoba;

        cout << "Menu" << endl;
        cout << "1.Dodaj osobe" << endl;
        cout << "2.Usun element" << endl;
        cout << "3.Wyswietl stos" << endl;
        cout << "4.Plik" << endl;
        cout << "5.Wyjscie" << endl;
        cin >> wyb;

        switch (wyb) {
            case 1: {
                dodaj(wskaznik);
                break;
            }

            case 2: {
                break;
            }

            case 3: {
                wyswietl(wskaznik);
            }
        }
    }while(wyb != 5);

    return 0;
}
```
