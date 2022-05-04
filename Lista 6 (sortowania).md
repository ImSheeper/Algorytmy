# main.cpp
```cpp
#include "Header.h"
#include <iostream>
#include <chrono>
#include <vector>
#include <algorithm>
#include <fstream>

using namespace std;

int main() {

    srand(time(NULL));

    int n;
    int mapa[] = {
            30000,
            50000,
            100000,
            150000,
            200000,
            500000,
            1000000,
            2000000,
            5000000,
            10000000
    };

    cout << "Wybierz rozmiar tablicy" << endl;
    cout << "1. 30.000" << endl;
    cout << "2. 50.000" << endl;
    cout << "3. 100.000" << endl;
    cout << "4. 150.000" << endl;
    cout << "5. 200.000" << endl;
    cout << "6. 500.000" << endl;
    cout << "7. 1.000.000" << endl;
    cout << "8. 2.000.000" << endl;
    cout << "9. 5.000.000" << endl;
    cout << "10. 10.000.000" << endl;
    cout << "11. Wprowadz recznie" << endl;
    cin >> n;

    if(n == 11) {
        cout << "Podaj liczbe" << endl;
        cin >> n;
        goto m;
    }

    n = mapa[n - 1];

    m:int* tab = new int[n];

    for (int i = 0; i < n; i++) {
        tab[i] = rand() % 100;
    }

    int sort;
    char tn;

    do {

        cout << "Wybierz sortowanie" << endl;
        cout << "1. Sortowanie babelkowe" << endl;
        cout << "2. Sortowanie przez wybor" << endl;
        cout << "3. Sortowanie przez wstawianie" << endl;
        cout << "4. Sortowanie przez scalanie" << endl;
        cout << "5. Sortowanie przez zliczanie" << endl;
        cout << "6. Sortowanie przez kopcowanie" << endl;
        cout << "7. Sortowanie kubelkowe" << endl;
        cout << "8. Sortowanie szybkie" << endl;
        cout << "9. Pomiar czasu wszystkich sortowan" << endl;
        cin >> sort;

        if (sort == 1) bubble(tab, n);
        if (sort == 2) selection(tab, n);
        if (sort == 3) insertion(tab, n);

        if (sort == 4) {
            auto start = chrono::system_clock::now();
            MergeSort(tab, 0, n - 1, n);
            auto end = chrono::system_clock::now();

            chrono::duration<double> time = end - start;

            cout << endl << "Czas operacji: " << time.count() << endl;

            ofstream plik;
            plik.open("Scalanie.txt");
            plik << "Nazwa sortowania: Przez scalanie" << endl;
            plik << "Czas operacji: " << time.count() << endl;
            plik.close();
        }

        if (sort == 5) countSort(tab, n - 1);
        if (sort == 6) heap(tab, n);
        //if (sort == 7) bucket(tab, n);

        if (sort == 8) {
            auto start = chrono::system_clock::now();
            quick(tab, 0, n - 1);
            auto end = chrono::system_clock::now();

            chrono::duration<double> time = end - start;

            cout << endl << "Czas operacji: " << time.count() << endl;

            ofstream plik;
            plik.open("quick.txt");
            plik << "Nazwa sortowania: Szybkie" << endl;
            plik << "Czas operacji: " << time.count() << endl;
            plik.close();
        }

        if (sort == 9) {
            cout << "Babelkowe: ";
            bubble(tab, n);
            cout << endl;

            cout << "Przez wybor: ";
            selection(tab, n);
            cout << endl;

            cout << "Przez wstawianie: ";
            insertion(tab, n);
            cout << endl;

            cout << "Przez scalanie: ";
            auto start = chrono::system_clock::now();
            MergeSort(tab, 0, n - 1, n);
            auto end = chrono::system_clock::now();

            chrono::duration<double> time = end - start;

            cout << endl << "Czas operacji: " << time.count() << endl << endl;

            ofstream plik;
            plik.open("Scalanie.txt");
            plik << "Nazwa sortowania: Przez scalanie" << endl;
            plik << "Czas operacji: " << time.count() << endl;
            plik.close();

            cout << "Przez zliczanie: ";
            countSort(tab, n - 1);
            cout << endl;

            cout << "Przez kopcowanie: ";
            heap(tab, n);
            cout << endl;

            //bucket(tab, n);

            cout << "Szybkie: ";
            start = chrono::system_clock::now();
            quick(tab, 0, n - 1);
            end = chrono::system_clock::now();

            time = end - start;

            cout << endl << "Czas operacji: " << time.count() << endl;

            plik.open("quick.txt");
            plik << "Nazwa sortowania: Szybkie" << endl;
            plik << "Czas operacji: " << time.count() << endl;
            plik.close();
        }

        cout << "Powtorzyc? t/n" << endl;
        cin >> tn;
    }while(tn != 'n');

    delete[] tab;

    return 0;
}
```

# Sortowania.cpp
```cpp
#include "Header.h"
#include <iostream>
#include <chrono>
#include <vector>
#include <algorithm>
#include <fstream>

using namespace std;

//Jakby kogos naszla ochota na wyswietlanie zawartosci tablicy
//zalecana cierpliwosc
void show(int* tab, int n) {

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            printf("%4d", tab[i]);
        }
    }
}

void bubble(int* tab, int n) {

    auto start = chrono::system_clock::now();

    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (tab[j] > tab[j + 1]) swap(tab[j], tab[j + 1]);
        }
    }

    auto end = chrono::system_clock::now();

    chrono::duration<double> time = end - start;

    cout << endl << "Czas operacji: " << time.count() << endl;

    ofstream plik;
    plik.open("babelek.txt");
    plik << "Nazwa sortowania: Babelkowe" << endl;
    plik << "Czas operacji: " << time.count() << endl;
    plik.close();
}

void selection(int* tab, int n) {

    auto start = chrono::system_clock::now();

    int min;

    for (int i = 0; i < n - 1; i++)
    {
        min = i;
        for (int j = i + 1; j < n; j++) {
            if (tab[j] < tab[min]) min = j;
        }
        swap(tab[min], tab[i]);
    }

    auto end = chrono::system_clock::now();

    chrono::duration<double> time = end - start;

    cout << endl << "Czas operacji: " << time.count() << endl;

    ofstream plik;
    plik.open("wybor.txt");
    plik << "Nazwa sortowania: Przez wybor" << endl;
    plik << "Czas operacji: " << time.count() << endl;
    plik.close();
}

void insertion(int* tab, int n) {

    int i, k, j;

    auto start = chrono::system_clock::now();

    for (i = 1; i < n; i++) {
        k = tab[i];
        j = i - 1;

        while (j >= 0 && tab[j] > k) {
            tab[j + 1] = tab[j];
            j = j - 1;
        }

        tab[j + 1] = k;
    }

    auto end = chrono::system_clock::now();

    chrono::duration<double> time = end - start;

    cout << endl << "Czas operacji: " << time.count() << endl;

    ofstream plik;
    plik.open("wstawianie.txt");
    plik << "Nazwa sortowania: Przez wstawianie" << endl;
    plik << "Czas operacji: " << time.count() << endl;
    plik.close();
}

void Merge(int *tab, int low, int high, int mid) {

    int i, j, k, temp[high-low+1];
    i = low;
    k = 0;
    j = mid + 1;

    while (i <= mid && j <= high) {
        if (tab[i] < tab[j]) {
            temp[k] = tab[i];
            k++;
            i++;
        }

        else {
            temp[k] = tab[j];
            k++;
            j++;
        }
    }

    while (i <= mid) {
        temp[k] = tab[i];
        k++;
        i++;
    }

    while (j <= high) {
        temp[k] = tab[j];
        k++;
        j++;
    }

    for (i = low; i <= high; i++) {
        tab[i] = temp[i-low];
    }

}

void MergeSort(int *tab, int low, int high, int n) {

    int mid;
    if (low < high) {
        mid=(low+high)/2;

        MergeSort(tab, low, mid, n);
        MergeSort(tab, mid + 1, high, n);

        Merge(tab, low, high, mid);
    }
}

int getMax(int* t, int n) {
    int naj = t[0];

    for(int i = 1; i < n; i++) {
        if(t[i] > naj) naj = t[i];
    }

    return naj;
}

void countSort(int* t, int n) {

    auto start = chrono::system_clock::now();

    int maxx = getMax(t, n) + 1;
    int* c = new int[maxx];

    for(int i = 0; i < maxx; i++) {
        c[i] = 0;
    }

    for(int i = 0; i < n; i++) {
        c[t[i]]++;
    }

    int ktory = 0;

    for(int liczba = 0; liczba < maxx; liczba++) {
        for(int ileJej = 0; ileJej < c[liczba]; ileJej++) {
            t[ktory] = liczba;
            ktory++;
        }
    }

    delete[] c;

    auto end = chrono::system_clock::now();

    chrono::duration<double> time = end - start;

    cout << endl << "Czas operacji: " << time.count() << endl;

    ofstream plik;
    plik.open("Zliczanie.txt");
    plik << "Nazwa sortowania: Przez zliczanie" << endl;
    plik << "Czas operacji: " << time.count() << endl;
    plik.close();
}

void heapify(int* tab, int n, int i) {
    int largest = i;
    int l = 2 * i + 1;
    int r = 2 * i + 2;

    if (l < n && tab[l] > tab[largest]) largest = l;

    if (r < n && tab[r] > tab[largest]) largest = r;

    if (largest != i) {
        swap(tab[i], tab[largest]);

        heapify(tab, n, largest);
    }
}

void heap(int* tab, int n) {

    auto start = chrono::system_clock::now();

    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(tab, n, i);

    for (int i = n - 1; i > 0; i--) {
        swap(tab[0], tab[i]);

        heapify(tab, i, 0);
    }

    auto end = chrono::system_clock::now();

    chrono::duration<double> time = end - start;

    cout << endl << "Czas operacji: " << time.count() << endl;

    ofstream plik;
    plik.open("kopcowanie.txt");
    plik << "Nazwa sortowania: Przez kopcowanie" << endl;
    plik << "Czas operacji: " << time.count() << endl;
    plik.close();
}

int n = 300;
int low = 1;
int maks = 10000;

int getMaxx(int* t, int n) {
    int naj = t[0];

    for(int i = 1; i < n; i++) {
        if(t[i] > naj) naj = t[i];
    }

    return naj;
}

int getMaxx(vector<int> t) {
    int naj = t[0];

    for(int i = 1; i < t.size(); i++) {
        if(t[i] > naj) naj = t[i];
    }

    return naj;
}

int getMin(vector<int> t) {
    int naj = t[0];

    for(int i = 1; i < t.size(); i++) {
        if(t[i] < naj) naj = t[i];
    }

    return naj;
}

int getMin(int* t, int n) {
    int naj = t[0];

    for(int i = 1; i < n; i++) {
        if(t[i] < naj) naj = t[i];
    }

    return naj;
}

void zliczanie(int* t, int n) {
    int maxx = getMaxx(t, n) + 1;
    int* c = new int[maxx];

    for(int i = 0; i < maxx; i++) {
        c[i] = 0;
    }

    for(int i = 0; i < n; i++) {
        c[t[i]]++;
    }

    int ktory = 0;

    for(int liczba = 0; liczba < maxx; liczba++) {
        for(int ileJej = 0; ileJej < c[liczba]; ileJej++) {
            t[ktory] = liczba;
            ktory++;
        }
    }

    delete[] c;
}

void zliczanie(vector<int>& t, bool wys = false) {
    int n = t.size();
    int maxx = getMaxx(t) + 1;
    vector<int> c;

    for(int i = 0; i < maxx; i++) {
        c.push_back(0);
    }

    for(int i = 0; i < n; i++) {
        c[t[i]]++;
    }

    int ktory = 0;

    for(int liczba = 0; liczba < maxx; liczba++) {
        for(int ileJej = 0; ileJej < c[liczba]; ileJej++) {
            t[ktory] = liczba;
            ktory++;
        }
    }

}

void kubelek(int* t, int n) {
    auto start = chrono::system_clock::now();

    int mx = getMaxx(t, n);
    int mn = getMin(t, n);

    int ileKubelkow = 10;
    vector<int> kubelki[ileKubelkow];

    int r = mx - mn;
    float p = (float)r / (float)ileKubelkow + 0.02;

    float prog = mn + p;
    int indexKubelka = 0;
    for(int i = 0; i < n; i++) {
        prog = mn + p;
        indexKubelka = 0;
        while(t[i] > prog) {
            prog += p;
            indexKubelka++;
        }
        (kubelki[indexKubelka]).push_back(t[i]);
    }

    for(int i = 0; i < ileKubelkow; i++) {
        zliczanie(kubelki[i]);
    }

    int indexTab = 0;
    for(int i = 0; i < ileKubelkow; i++) {
        for(auto liczba : kubelki[i]) {
            t[indexTab] = liczba;
            indexTab++;
        }
    }

    auto end = chrono::system_clock::now();

    chrono::duration<double> time = end - start;

    cout << endl << "Czas operacji: " << time.count() << endl;

    ofstream plik;
    plik.open("kubelkowe.txt");
    plik << "Nazwa sortowania: Kubelkowe" << endl;
    plik << "Czas operacji: " << time.count() << endl;
    plik.close();
}

//Nie wiem jak mierzyc czas, wiec funkcja z tego poleciala do main'a
void quick(int *tab, int lewy, int prawy) {

    if(prawy <= lewy) return;

    int i = lewy - 1, j = prawy + 1,
    pivot = tab[(lewy+prawy)/2];

    while(1) {
        while(pivot>tab[++i]);

        while(pivot<tab[--j]);

        if( i <= j) swap(tab[i],tab[j]);

        else break;
    }

    if(j > lewy) quick(tab, lewy, j);
    if(i < prawy) quick(tab, i, prawy);
}
```
# Header.h
```cpp
#pragma once

void bubble(int* tab, int n);
void selection(int* tab, int n);
void insertion(int* tab, int n);
void Merge(int *tab, int low, int high, int mid);
void MergeSort(int *tab, int low, int high, int n);
int getMax(int array[], int size);
void countSort(int *array, int size);
void heapify(int* tab, int n, int i);
void heap(int* tab, int n);
void bucket(float *tab, int n);
void quick(int *tab, int lewy, int prawy);
```
