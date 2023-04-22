#include <iostream>
#include <cstring>
using namespace std;
class Angajat{
private:
    int id;
    char*nume;
    char*pozitia;
    double salariu;
    int varsta;
public:
    Angajat():id(0),nume(nullptr),pozitia(nullptr),salariu(0),varsta(0){
        nume=new char[1];
        nume[0]='\0';
        pozitia=new char[1];
        pozitia[0]='\0';
    }
    Angajat(int id, const char* nume, const char* pozitia, double salariu, int varsta):id(id),salariu(salariu),varsta(varsta){
        this->nume = new char[strlen(nume)+1];
        strcpy(this->nume, nume);
        this->pozitia = new char[strlen(pozitia)+1];
        strcpy(this->pozitia, pozitia);
    }
    Angajat(const Angajat& A):id(A.id),salariu(A.salariu),varsta(A.varsta){
        this->nume=new char[strlen(A.nume)+1];
        strcpy(this->nume, A.nume);
        this->pozitia=new char[strlen(A.pozitia)+1];
        strcpy(this->pozitia, A.pozitia);
    }
    Angajat&operator=(const Angajat& A){
        if (this != &A) {
            this->id = A.id;
            delete[] this->nume;
            this->nume = new char[strlen(A.nume) + 1];
            strcpy(this->nume, A.nume);
            delete[] this->pozitia;
            this->pozitia = new char[strlen(A.pozitia) + 1];
            strcpy(this->pozitia, A.pozitia);
            this->salariu = A.salariu;
            this->varsta = A.varsta;
        }
        return *this;
    }
    int get_id()const
    {
        return this->id;
    }
    char* get_nume()const
    {
        return this->nume;
    }
    void reset_pozitia(const char*pozitia)
    {
        strcpy(this->pozitia,pozitia);
    }
    char* get_pozitia()const
    {
        return this->pozitia;
    }
    void marire_salariu(double marire)
    {
        this->salariu+=marire;
    }
    double get_salariu()const
    {
        return this->salariu;
    }
    void marire_varsta()
    {
        this->varsta++;
    }
    double get_varsta()const
    {
        return this->varsta;
    }
    ~Angajat()
    {
        if(this->nume!=nullptr)
          delete[] this->nume;
        if(this->pozitia!=nullptr)
          delete[] this->pozitia;
    }
    friend istream& operator >> (istream&citire,Angajat&ob);
    friend ostream& operator << (ostream&afisare,Angajat&ob);
};
istream& operator >> (istream&citire,Angajat&ob)
{
    cout<<"Introduceti ID:";citire>>ob.id;citire.get();
    cout<<"Introduceti numele:";ob.nume=new char[30];citire.get(ob.nume,30,'\n');citire.get();
    cout<<"Introduceti pozitia:";ob.pozitia=new char[30];citire.get(ob.pozitia,30,'\n');citire.get();
    cout<<"Introduceti salariul:";citire>>ob.salariu;citire.get();
    cout<<"Introduceti varsta:";citire>>ob.varsta;citire.get();
    return citire;
}
ostream& operator << (ostream&afisare,Angajat&ob)
{
    cout<<"ID:";afisare<<ob.id<<'\n';
    cout<<"Nume:";afisare<<ob.nume<<'\n';
    cout<<"Pozitia:";afisare<<ob.pozitia<<'\n';
    cout<<"Salariul:";afisare<<ob.salariu<<'\n';
    cout<<"Varsta:";afisare<<ob.varsta<<'\n';
    return afisare;
}
class AngajatVector{
private:
    int size;
    Angajat*data;
public:
    AngajatVector()
    {
        size=0;
        data=nullptr;
    }
    AngajatVector(int k,Angajat&x)
    {
        data=new Angajat[k];
        for(int i=0;i<=k-1;i++)
            data[i]=x;
        size=k;
    }
    AngajatVector(const AngajatVector&v)
    {
        this->data=new Angajat[v.size];
        for(int i=0;i<v.size;i++)
            this->data[i]=v.data[i];
        this->size=v.size;
    }
    Angajat*get_address()
    {
        return this->data;
    }
    Angajat&get_angajatul_k(int k)
    {
        if(k<size)
          return this->data[k];
        else cout<<"Angajatul nr "<<k<<" nu exista!";
    }
    int get_size()
    {
        return this->size;
    }
    void add(AngajatVector&v, Angajat&x)
    {
        Angajat* new_data = new Angajat[v.size + 1];
        for (int i = 0; i < v.size; i++)
        {
            new_data[i] = v.data[i];
        }
        new_data[v.size] = x;
        delete[] v.data;
        v.data = new_data;
        v.size++;
    }
    void test(const AngajatVector&v)
    {
        cout<<&v<<" "<<this;
    }
    ~AngajatVector()
    {
        if(data!=nullptr)
            delete[] data;
    }
    AngajatVector& operator = (const AngajatVector&v)
    {
        if(this!=&v) {
            delete[] this->data;
            this->data = new Angajat[v.size];
            for (int i = 0; i < v.size; i++)
                this->data[i] = v.data[i];
            this->size=v.size;
        }
        return *this;
    }
    friend ostream& operator << (ostream&afisare,AngajatVector&V);
    friend istream& operator >>(istream&afisare,AngajatVector&V);
};
ostream& operator << (ostream&afisare,AngajatVector&V) {
    int n = V.get_size();
    for (int i = 0; i <= n - 1; i++)
        afisare << V.data[i] << "\n\n";
    return afisare;
}
istream& operator >> (istream&citire,AngajatVector&V)
{
    int n,x;
    cout<<"Introduceti numarul de angajati:";citire>>n;
    V.size=n;
    if(V.data!=nullptr)
        delete[] V.data;
    V.data=new Angajat[V.size];
    for(int i=0;i<n;i++)
        citire>>V.data[i];
    return citire;
}
class Client{
private:
    char*nume;
    char*comanda;
    int nr_telefon;
public:
    Client():nume(nullptr),comanda(nullptr)
    {
        nume=new char[1];
        nume[0]='\0';
        comanda=new char[1];
        comanda[0]='\0';
    }
    Client(char*nume,char*comanda,int nr_telefon):nume(nullptr),comanda(nullptr),nr_telefon(nr_telefon)
    {
          this->nume=new char[strlen(nume)+1];
          strcpy(this->nume,nume);
          this->comanda=new char[strlen(comanda)+1];
          strcpy(this->comanda,comanda);
    }
    Client(const Client& A):nr_telefon(nr_telefon){
        this->nume=new char[strlen(A.nume)+1];
        strcpy(this->nume, A.nume);
        this->comanda=new char[strlen(A.comanda)+1];
        strcpy(this->comanda, A.comanda);
    }
    Client&operator=(const Client& A){
        if (this != &A) {
            delete[] this->nume;
            this->nume = new char[strlen(A.nume) + 1];
            strcpy(this->nume, A.nume);
            delete[] this->comanda;
            this->comanda = new char[strlen(A.comanda) + 1];
            strcpy(this->comanda, A.comanda);
            this->nr_telefon = A.nr_telefon;
        }
        return *this;
    }
    ~Client()
    {
        if(this->nume!=nullptr)
            delete[] this->nume;
        if(this->comanda!=nullptr)
            delete[] this->comanda;
    }
    char*get_nume()
    {
        return this->nume;
    }
    char*get_comanda()
    {
        return this->comanda;
    }
    int get_nrtelefon()
    {
        return this->nr_telefon;
    }
    friend istream& operator >> (istream&citire,Client&ob);
    friend ostream& operator << (ostream&afisare,Client&ob);

};
istream& operator >> (istream&citire,Client&ob)
{
    cout<<"Introduceti numele:";ob.nume=new char[30];citire.get(ob.nume,30,'\n');citire.get();
    cout<<"Introduceti comanda:";ob.comanda=new char[30];citire.get(ob.comanda,30,'\n');citire.get();
    cout<<"Introduceti nr_telefon(fara prefixul +40):";citire>>ob.nr_telefon;citire.get();
    return citire;
}
ostream& operator << (ostream&afisare,Client&ob)
{
    cout<<"Nume:";afisare<<ob.nume<<'\n';
    cout<<"Comanda:";afisare<<ob.comanda<<'\n';
    cout<<"Nr_telefon:0";afisare<<ob.nr_telefon<<'\n';
    return afisare;
}
class ClientList{
private:
    class Nod{
    private:
        Client info;
        Nod*next;
    public:
        void set_data(Client&info,Nod*next)
        {
            this->info=info;
            this->next=next;
        }
        Client&get_data()
        {
            return this->info;
        }
        Nod*get_next()
        {
            return next;
        }
        ~Nod()
        {
            if(this->next!=nullptr)
                delete this->next;
        }
    };
    Nod*ROOT,*r,*END;
public:
    ClientList()
    {
        ROOT=nullptr;
    }
    ~ClientList()
    {
        if(this->ROOT!=nullptr)
            delete this->ROOT;
    }
    Client&get_root()
    {
        return this->ROOT->get_data();
    }
    Client&get_end()
    {
        return this->END->get_data();
    }
    void add_begin(Client&val)
    {
        if(ROOT==nullptr)
        {
            ROOT=new Nod;
            (*ROOT).set_data(val,nullptr);
            END=ROOT;
        }
        else{
            r=new Nod;
            (*r).set_data(val,ROOT);
            ROOT=r;
        }
    }
    void add_end(Client&val)
    {
        if(ROOT==nullptr)
        {
            ROOT=new Nod;
            (*ROOT).set_data(val,nullptr);
            END=ROOT;
        }
        else{
            r=new Nod;
            (*r).set_data(val,nullptr);
            (*END).set_data(END->get_data(),r);
            END=r;
        }
    }
    void pop()
    {
        if(ROOT!=nullptr)
        {
            if(ROOT->get_next()!=nullptr)
                ROOT=ROOT->get_next();
            else cout<<"Exista doar un singur client la coada!\n";
        }
        else cout<<"Momentan nu exista clienti!";
    }
    void interogare_coada()
    {
        Nod*aux;
        aux=ROOT;
        cout<<"Coada este formata din urmatorii clienti:";
        while(aux!=nullptr)
        {
            cout<<aux->get_data().get_nume()<<"\t";
            aux=aux->get_next();
        }
    }
};
class HR{
private:
    int nr_angajati;
    int nr_clienti;
    double medie_varsta_angajati;
    double medie_salarii_angajati;
public:
    HR():nr_angajati(0),nr_clienti(0),medie_varsta_angajati(0),medie_salarii_angajati(0)
    {}
    int get_nr_angajati()
    {
        return this->nr_angajati;
    }
    void update_nr_angajati(int x)
    {
        nr_angajati+=x;
    }
    int get_nr_clienti()
    {
        return this->nr_clienti;
    }
    void update_nr_clienti(int x)
    {
        nr_angajati+=x;
    }
    void recalculare_medie_salarii_angajati(double x)
    {
        double aux=medie_salarii_angajati*nr_angajati;
        aux+=x;
        aux=aux/1.0/(nr_angajati+1);
        medie_salarii_angajati=aux;
    }
    void recalculare_medie_varsta_angajati(double x)
    {
        double aux=medie_varsta_angajati*nr_angajati;
        aux+=x;
        aux=aux/1.0/(nr_angajati+1);
        medie_varsta_angajati=aux;
    }
    double get_medie_salarii()
    {
        return this->medie_salarii_angajati;
    }
    double get_medie_varste()
    {
        return this->medie_varsta_angajati;
    }
};
int main()
{
    int cerinta;
    cout<<"Buoungiorno!\nBine ati venit la Tudi's Pizza,adevarata pizza napoletana din Bucuresti!";
    cout<<"Acesta este un meniu interactiv in care puteti gestiona atat clientii cat si angajatii din pizzerie!\nPentru a adauga un angajat,apasati tasta 1\n";
    cout<<"Pentru a adauga un client la coada,apasati tasta 2\nPentru a elimina un client de la coada,apasati tasta 3\nPentru a afisa coada de clienti,apsati tasta 4\n";
    cout<<"Pentru a afisa lista de angajati,apasati tasta 5\nPentru a afisa numarul de amgajati,apasati tasta 6\n";
    cout<<"Pentru a afisa numarul de clienti,apasati tasta 7\nPentru a intrerupe meniul interactiv,apasati tasta 0\n";
    cout<<"Introduceti cerinta:";cin>>cerinta;
    Angajat X;
    AngajatVector V;
    Client Y;
    ClientList L;
    HR H;
    while(cerinta)
    {
        switch(cerinta)
        {
            case 1:{
                cin>>X;
                V.add(V,X);
                H.update_nr_angajati(1);
                H.recalculare_medie_salarii_angajati(X.get_salariu());
                H.recalculare_medie_varsta_angajati(X.get_varsta());
            }break;
            case 2:{
                cin>>Y;
                L.add_end(Y);
                H.update_nr_clienti(1);
            }break;
            case 3:{
                L.pop();
                H.update_nr_clienti(-1);
            }break;
            case 4:{
                L.interogare_coada();
                cout<<'\n';
            }break;
            case 5:{
                cout<<"Lista de angajati este:\n";
                cout<<V;
            }break;
            case 6:{
              cout<<"Numarul de angajati este:"<<H.get_nr_angajati()<<'\n';
            }break;
            case 7:{
                cout<<"Numarul de clienti este:"<<H.get_nr_clienti()<<'\n';
            }break;
            case 8:{
                cout<<"Media salariilor angajatilor este:"<<H.get_medie_salarii()<<'\n';
            }break;
            case 9:{
                cout<<"Media varstelor angajatilor este:"<<H.get_medie_varste()<<'\n';
            }break;
        }
        cout<<"Introduceti cerinta:";cin>>cerinta;cin.get();
    }
    return 0;
}