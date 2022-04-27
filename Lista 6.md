```cpp
#include <iostream>
#include <vector>
#include <sstream>
#include <fstream>
#include <string>
#include <cctype>
#include <algorithm>

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

bool next(lista* nowy, lista* aktualny) {

    int a = aktualny->nazwisko.size() > nowy->nazwisko.size() ? nowy->nazwisko.size() : aktualny->nazwisko.size();

    for (int i = 0; i < a; i++) {

        if (tolower(aktualny->nazwisko[i]) > tolower(nowy->nazwisko[i]))
            return false;
    }

    return true;
}

void dodaj(lista* nowy, lista* pocz, lista* koniec, bool spr) {

    lista* t = pocz;

    if (spr) {

        while (t->nast)
        {
            if (t->imie == nowy->imie && t->nazwisko == nowy->nazwisko && t->wiek == nowy->wiek && t->PESEL == nowy->PESEL) {
                cout << "dany element istnieje na liscie" << endl;
                return;
            }

            t = t->nast;
        }
    }

    if (pocz->imie == "") {
        *pocz = *nowy;
        *koniec = *nowy;
    }

    else {
        lista* temp = pocz;

        while (temp->nast && next(nowy, temp)) {
            temp = temp->nast;
        }

        if (!temp->pop && next(temp, nowy)) {
            lista* poczv2 = pocz->nast;

            lista* pierwszy = new lista;

            *pierwszy = *pocz;
            pierwszy->pop = nowy;
            pierwszy->nast = poczv2;

            nowy->nast = pierwszy;
            *pocz = *nowy;
        }

        else if (!temp->nast && next(nowy, temp)) {
            nowy->nast = temp->nast;
            temp->nast = nowy;
            nowy->pop = temp;
            *koniec = *nowy;
        }

        else {
            if (!temp->pop) {
                nowy->pop = temp;
                nowy->nast = temp->nast;
                temp->nast = nowy;
                temp->nast->pop = nowy;
                return;
            }

            nowy->pop = temp->pop;
            nowy->nast = temp;

            lista* el = temp->nast;
            lista* pierwszy = new lista;

            *pierwszy = *temp;
            pierwszy->pop = nowy;
            pierwszy->nast = el;

            nowy->nast = pierwszy;

            temp = temp->pop;
            *temp->nast = *nowy;
        }
    }
}

void wpiszdane(lista* pocz, lista* koniec, bool spr = false) {

    lista* nowy = new lista;
    cout << "podaj imie" << endl;
    cin >> nowy->imie;
    cout << "podaj nazwisko" << endl;
    cin >> nowy->nazwisko;
    cout << "podaj PESEL" << endl;
    cin >> nowy->PESEL;
    cout << "podaj wiek" << endl;
    cin >> nowy->wiek;
    dodaj(nowy, pocz, koniec, spr);
}

void sortlista(lista* temp, lista* koniec, lista* pocz) {

    if (temp->pop == nullptr && temp->nast == nullptr) {
        lista* nowy = new lista;
        *pocz = *nowy;
        *koniec = *nowy;
        return;
    }

    if (temp->pop != nullptr) temp->pop->nast = temp->nast;

    if (temp->nast != nullptr) temp->nast->pop = temp->pop;

    if (!temp->nast) {

        *koniec = *temp->pop;
        koniec->nast = nullptr;
    }

    if (!temp->pop) *pocz = *temp->nast;
}

void usun(lista* pocz, lista* koniec, string s_wybor, int wybor, bool all = false)
{

    lista* temp = pocz;
    while (temp) {

        transform(s_wybor.begin(), s_wybor.end(), s_wybor.begin(), ::tolower);

        switch (wybor) {
        case 1:
            if (temp->imie == s_wybor) {
                sortlista(temp, koniec, pocz);
                if (!all)
                    return;
            }
            break;

        case 2:
            if (temp->nazwisko == s_wybor) {
                sortlista(temp, koniec, pocz);
                if (!all)
                    return;
            }
            break;

        case 3:
            if (temp->PESEL == s_wybor) {
                sortlista(temp, koniec, pocz);
                if (!all)
                    return;
            }
            break;

        case 4:
            if (temp->wiek == stoi(s_wybor)) {
                sortlista(temp, koniec, pocz);
                if (!all)
                    return;
            }
            break;

        }

        temp = temp->nast;
    }
}

bool spr(lista* pocz) {

    lista* temp = pocz;
    if (!pocz->imie.empty()) return true;
    return false;
}

void usunwybor(lista* pocz, lista* koniec, bool all = false) {

    cout << "po czym chcesz usunac\n";
    cout << "1.imie\n";
    cout << "2.nazwisko\n";
    cout << "3.PESEL\n";
    cout << "4.wiek\n";

    int a;
    cin >> a;

    string b;

    cout << "podaj fraze\n";
    cin >> b;

    switch (a) {
    case 1:
        usun(pocz, koniec, b, 1, all);
        break;
    case 2:
        usun(pocz, koniec, b, 2, all);
        break;
    case 3:
        usun(pocz, koniec, b, 3, all);
        break;
    case 4:
        usun(pocz, koniec, b, 4, all);
        break;
    }
}

void usun(lista* pocz, lista* koniec, int index) {

    lista* temp = pocz;

    for (int i = 0; i < index; ++i) {
        temp = temp->nast;
    }

    sortlista(temp, pocz, koniec);
}

void show(lista* temp) {

    for (int i = 1; temp; i++) {
        cout << i << ". " << "imie: " << temp->imie << " nazwisko: " << temp->nazwisko << " PESEL: " << temp->PESEL << endl;
        temp = temp->nast;
    }

}

void edit(lista* pocz, lista* koniec) {

    show(pocz);
    cout << "ktory numer chcesz edytowac?" << endl;

    int a;
    cin >> a;

    usun(pocz, koniec, a - 1);
    wpiszdane(pocz, koniec);
}

void funzapis(lista* pocz, string nazwisko = "", bool pelnoletni = false) {

    lista* temp = pocz;
    vector<lista> array;

    while (temp) {
        array.push_back(*temp);
        temp = temp->nast;
    }

    string save;

    if (!nazwisko.empty()) {

        int zmiany = 0;

        do {
            zmiany = 0;

            for (int i = 0; i < array.size(); ++i) {

                transform(array[i].nazwisko.begin(), array[i].nazwisko.end(), array[i].nazwisko.begin(), ::tolower);
                transform(nazwisko.begin(), nazwisko.end(), nazwisko.begin(), ::tolower);

                if (array[i].nazwisko != nazwisko) {
                    array.erase(array.begin() + i);
                    zmiany++;
                }
            }
        } while (zmiany != 0);

        if (array.size() == 0) {
            cout << "nie ma osob o podanym nazwisku" << endl;
        }
    }

    if (pelnoletni) {
        int zmiany = 0;

        do {
            zmiany = 0;
            for (int i = 0; i < array.size(); ++i) {

                if (array[i].wiek < 18) {
                    array.erase(array.begin() + i);
                    zmiany++;
                }
            }
        } while (zmiany != 0);

        if (array.size() == 0) {
            cout << "nie ma osob pelnoletnich" << endl;
        }
    }

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

void zapis(lista* pocz)
{
    cout << "wybierz:" << endl;
    cout << "1.wszystkie" << endl;
    cout << "2.o wybranym nazwisku" << endl;
    cout << "3.pelnoletnie" << endl;

    int a;
    cin >> a;
    string nazwisko;

    switch (a) {
    case 1:
        funzapis(pocz);
        break;
    case 2:
        cout << "podaj nazwisko" << endl;
        cin >> nazwisko;
        funzapis(pocz, nazwisko);
        break;
    case 3:
        funzapis(pocz, "", true);
        break;
    }
}

void odczyt() {
    //inicjacja zmiennej odpowiadającej za odczyt danych
    fstream odczyt;
    //otwieranie pliku
    odczyt.open("dane.txt");
    //sprawdzanie czy dalo się otworzyć plik
    if (!odczyt.good()) {
        cout << "nie mozna uzyskac dostepu do pliku\n";
        return;
    }

    string linia;

    do {
        //pobieranie linia po linii i wyświetlanie
        getline(odczyt, linia);
        cout << linia << endl;
    } while (linia != "");
    //zamykanie pliku
    odczyt.close();
}

int main() {

    lista* pocz = new lista;
    lista* koniec = new lista;

    while (true) {

        cout << "1.dodaj element\n";
        cout << "2.dodaj element z wskazaniem czy dany element istnieje juz na liscie \n";
        cout << "3.usun jeden element o podanym kluczu\n";
        cout << "4.usun wszystkie elementy z listy na podstawie klucza\n";
        cout << "5.wyswietl\n";
        cout << "6.edytuj wybrany element listy\n";
        cout << "7.zapisz do pliku\n";
        cout << "8.wyjdz\n";
        cout << "9.wyswietl co znajduje sie w pliku\n";
        cout << "wybierz: ";

        int a;
        cin >> a;

        string data;
        switch (a) {
        case 1:
            wpiszdane(pocz, koniec);
            break;
        case 2:
            wpiszdane(pocz, koniec, true);
            break;
        case 3:
            if (spr(pocz))
                usunwybor(pocz, koniec);
            else
                cout << "nie ma uzytkownika\n";
            break;
        case 4:
            if (spr(pocz))
                usunwybor(pocz, koniec, true);
            else
                cout << "nie ma uzytkownika\n";
            break;
        case 5:
            show(pocz);
            break;
        case 6:
            if (spr(pocz))
                edit(pocz, koniec);
            else
                cout << "nie ma uzytkownika\n";
            break;
        case 7:
            zapis(pocz);
            break;
        case 8:
            delete(koniec);
            delete(pocz);
            return EXIT_SUCCESS;
        case 9:
            odczyt();
            break;
        default:
            cout << "wybrales zly nr" << endl;
            break;
        }
    }
}
```
