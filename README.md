# Fiche révision C++

## 1. Variables

```c++
extern const double mon_pi; // déclaration variable globale (header)
const double mon_pi = 3.14159; // déclaration variable globale (source)
```

## 2. Tableau & Pointeurs

```c++
void swap(int* pi, int* pj){
    int tmp = ∗pi;
    ∗pi = ∗pj;
    ∗pj = tmp;
}
swap(&x, &y); // Swap des valeurs pointées
```

## 3. Références
On peut aussi déclarer des références non-modifiables via le mot-clé **const**
```c++
struct Personne{
  char nom[100];
  char prenom[100];
};

void affiche (const Personne & p){
  std::cout << p.prenom << " " << p.nom << std::endl;
}

int main(){
  Personne P1 = { "Jean", "Bon" };
  affiche(P1);
}
```

## 4. Structure et classe
### 1. Structure
```c++
// Point.hpp
#ifndef POINT HPP

#define POINT_HPP
// évite d'inclure deux fois un fichier en-tête

class Vecteur; // déclare l'existence d'une classe Vecteur

class Point {
    double coord[2]; // privé, accessible à partir des méthodes.
    
public:
    Point () ; // déclare un point à 1'origine du repère (0,0)
    Point (double aX, double aY): / déclare un point (aX, aY)
    double x() const: // const = indique que la méthode ne change pas les données membres
    double y() const:
    // crée un nouveau point translaté (donc méthode const)
    Point operator+(const Vecteur & v) const;
    // Translate ce point du vecteur spécifié (done méthode non-const)
    Point& operator+=(const Vecteur & v);
};
#endif
```
### 2. Classe
```c++
// Point.cpp
#include "Point.hpp"
#include "Vecteur.hpp"

Point::Point(){
    coord[0] = 0.0; 
    coord[1] = 0.0;
}
Point::Point(double aX, double aY)
{
    coord[0] = aX;
    coord[1] = aY;
}
double Point::x() const {
    return coord[0] ;
}
double Point::y() const{
    return coord[1];
}
Point Point::operator+(const Vecteur & v) const { 
    return Point(x() + v.x(), y() + v.y());
}
Point& Point::operator+=(const Vecteur & v) {
    coord[0] += v.x();
    coord[1] += v.y();
    return ∗this; // Retourne une référence à soi même
}
```
### 3. Héritage
![Code](https://i.imgur.com/sAzzbeD.png)

### 4. Polymorphisme dynamique ; redéfinition (abstract)
On utilise le mot-clé virtual devant la méthode pour indiquer que celle-ci peut être redéfinie dans une sous-classe et le mot-clé = 0 derrière la méthode pour rendre la classe abstraite et forcer la redéfinition.

![enter image description here](https://i.imgur.com/0OHhTuS.png)

### 5. Transtypage
Dans une hiérarchie de classes, il est possible de transtyper directement un pointeur ou une référence d’une classe fille vers une classe mère.
![enter image description here](https://i.imgur.com/KUIJMpc.png)

## 5. Surcharge opérateur
### 1. Basique
Comme autre exemple, on peut envisager d’additionner des éléments variés. On peut donc surcharger une fonction **add** pour divers paramètres, ou même **surcharger l’opérateur binaire +.**
```c++
struct Point { double abs; double ord; };
double add(double a, double b){
    return a+b
};
Point add(Point p1, Point p2){
    Point q = { p1.abs + p2.abs , p1.ord + p2.ord } ;
    return q;
}
Point operator+(Point p1, Point p2){
    return add(p1, p2);
}
```
### 2. Surcharge des opérateurs flux
On peut aussi surcharger les opérateurs de flux de sortie (operator«) et d’entrée (operator»), afin de faciliter la sérialisation ou l’affichage d’un objet. On rajouterait par exemple dans le fichier Point.hpp les lignes suivantes :
```c++
ostream& operator<<(ostream & out, const Point & p){
    out << " ( " << p.x() << " , " << p.y() << " ) ";
    return out;
}
```

## 6. Mécanismes d’allocation mémoire
**Allocation statique**: Celle-ci est faite automatiquement par le compilateur à la déclaration de la variable. La zone mémoire allouée est sur la pile d’exécution du fils d’exécution. A la sortie du bloc où la variable a été déclarée, la zone mémoire de la variable est automatiquement désallouée. La durée de vie de la variable est donc limitée au bloc de définition. 
![enter image description here](https://i.imgur.com/ZLpMD1D.png)

**Allocation dynamique:** Celle-ci est nécessairement spécifiée par l’utilisateur par le biais d’un appel à l’opérateur new. Celui-ci retourne l’adresse du zone mémoire où la ou les variables ont été instanciés. La zone mémoire allouée est sur le tas du processus. A la sortie du bloc où l’opérateur new a été appelé, la zone mémoire allouée n’est pas désallouée et continue à exister. Ce n’est qu’à l’appel de l’opérateur delete que la zone est effectivement désallouée et rendue au système. La durée de vie de la variable ou des variables pointées n’est donc pas limitée au bloc de définition.

> Elle se fait avec les opérateurs **new** et **delete** pour une variable, et **new[]** et **delete[]** pour les tableaux. Les opérateurs new alloue l’espace mémoire correspondant (qui dépend bien sûr de la taille mémoire du type et du nombre de cases du tableaux), appellent le(s) constructeur(s) spécifié(s) ou par défaut, puis retourne l’adresse en mémoire correspondante. Les opérateurs delete appellent le(s) destructeur(s) puis désalloue l’espace mémoire correspondant et le rende au système.

![enter image description here](https://i.imgur.com/F6jv3gC.png)

### 1 . Exemple tri à bulle
#### 1. Avec un Comparable
![enter image description here](https://i.imgur.com/ZzYOEsj.png)
#### 2. Version générique 
![enter image description here](https://i.imgur.com/LgdwHmL.png)
![enter image description here](https://i.imgur.com/dYoqh7t.png)
### 2. Patron de fonction ; fonction générique
![enter image description here](https://i.imgur.com/i4xZLRV.png)
![enter image description here](https://i.imgur.com/DapkLiy.png)
#### 1. Patrons de classe ou classes génériques![enter image description here](https://i.imgur.com/P5Q9yag.png)
#### 2. Spécialisation des patrons de classes
![enter image description here](https://i.imgur.com/ZKszLJ1.png)
![enter image description here](https://i.imgur.com/gMFsoY4.png)
#### 3. Spécialisation des patrons de fonctions
![enter image description here](https://i.imgur.com/MeTyTCq.png)

## Généricité et Standard Template Library (STL)
- **std::vector** un tableau de taille modifiable, 
- **std::list** une séquence de valeurs,  
- **std::stack** une pile de valeurs, 
-  **std::queue** une file de valeurs,
-  **std::deque** une file à double-entrée de valeurs (en gros, une pile + une file en même temps), 
- **std::priority_queue** une file à priorité de valeurs, 
- **std::set** un ensemble de valeurs (sans duplicata), 
- **std::multiset** un ensemble de valeurs (avec duplicata), 
- **std::map** un tableau associatif clé/valeur (sans duplicata), 
- **std::multimap** un tableau associatif clé/valeur (avec duplicata),

![enter image description here](https://i.imgur.com/0dKX3cP.png)

***Exemple : Vecteur***
Le conteneur le plus utilisé est sans conteste **std::vector**, qui correspond à un **tableau de taille ajustable** à l’exécution. Asymptotiquement, pour n éléments dont chaque instance coût b octets, la taille mémoire est nb.
![enter image description here](https://i.imgur.com/xUCiVPG.png)

**Conteneurs associatifs**
Les conteneurs associatifs permettent de modéliser des ensembles d’éléments (appelés clés) ou des tableaux associatifs associant une valeur à tout élément (appélé clé). Il faut un opérateur de comparaison entre clés, soit donné par l’opérateur inférieur, soit donné par un objet comparateur.

***Exemple : Set***
![enter image description here](https://i.imgur.com/CSfGz8E.png)

***Exemple : Map***
![enter image description here](https://i.imgur.com/ALlYp9e.png)

### 1. Itérateurs
Tous les conteneurs précités et même un certain nombre de quasi-conteneurs (string, valarray, bitset, etc) fournissent des itérateurs pour parcourir et modifier les données. Ils n’ont en revanche pas tous les mêmes propriétés et n’offrent pas tous les mêmes services. En un sens, les itérateurs forment une hiérarchie (au sens sémantique), mais ne sont pas reliés physiquement par des relations d’héritage. Voilà les principales catégories d’itérateurs :

**InputIterator** Cet itérateur est CopyConstructible, Assignable, Destructible. En plus, il peut être incrémenté (operator++), et peut être comparé (operator== et operator!=). Enfin, il peut être déréférencé (operator*) comme une rvalue. 35 

**OutputIterator** Pareil qu’un InputIterator sauf qu’il peut être déréférencé (operator*) comme une lvalue. 

**ForwardIterator** Fusionne InputIterator et OutputIterator et rajoute que l’on peut accéder plusieurs fois au même élément par un déréférencement. Imaginez le cas d’un itérateur en lecture sur un flux audio/vidéo : une fois lu, la valeur est perdu si l’itérateur est passé à la suite. Dans ce cas, l’itérateur est un InputIterator et non un ForwardIterator. 

**BidirectionalIterator** Ajoute au ForwardIterator les opérations de décrémentation (operator–). 

**RandomAccessIterator** Ajoute au BidirectionalIterator toutes les opérations arithmétiques pour avancer ou reculer de plusieurs éléments d’un seul coup, les comparateurs inférieurs, supérieurs, et l’opérateur de déréférencement à la n-ème case (operator[]). 

 **list, set, map** fournissent des BidirectionalIterators. Il y a une version lecture/écriture appelée iterator et une version lecture seulement appelée const_iterator. 

**vector** fournit des RandomAccessIterators. Il y a une version lecture/écriture appelée iterator et une version lecture seulement appelée const_iterator.

#### Exemples
Le code ci-dessous montre comment éliminer les duplicata dans un conteneur vector. Les éléments restants sont triés mais tous distincts.
![enter image description here](https://i.imgur.com/qnxJdss.png)

Le code suivant ne garde que les noms entre les lettres "D" (inclus) et "L" de l’annuaire
![enter image description here](https://i.imgur.com/yqbte4v.png)
![enter image description here](https://i.imgur.com/HHrJyjD.png)

# 2. Exemples pour l'examen 
## 1. File générique T (LIFO : Last in First Out)
```c++
template <class T>
class Queue{  
private:
	Node<T>* my_front;
	Node<T>* my_back;
	int my_alloc;
public:
	Queue(void){
		my_front = NULL;
		my_back = NULL;
		my_alloc = 0;
	}

	~Queue(void){
		while(!isEmpty())
			Dequeue();
	}

	void Enqueue(T element){
		Node<T>* tmp = new Node<T>();
		tmp->setData(element);
		tmp->setNext(NULL);

		if (isEmpty()) {
			my_front = my_back = tmp;
		}
		else {
			my_back->setNext(tmp);
			my_back = tmp;
		}
		my_alloc++;
	}

	T Dequeue(void){
		if (isEmpty())
			cout << "Queue is empty" << endl;
		T ret = my_front->getData();
		Node<T>* tmp = my_front;
		my_front = my_front->getNext();
		my_alloc--;
		delete tmp;
		return ret;
	}

	T First(void){
		if (isEmpty())
			cout << "Queue is empty" << endl;
		return my_front->getData();
	}

	int Size(void){
		return my_alloc;
	}

	bool isEmpty(void){
		return my_alloc == 0 ? true : false;
	}
};

template <class T>
class Node
{
private:
	T data;
	Node* next;
public:
	void setData(T element){
		data = element;
	}
	void setNext(Node<T>* element){
		next = element;
	}
	T getData(void){
		return data;
	}
	Node* getNext(void){
		return next;
	}
};
```

## 2. Itérateur
```c++
struct Iterator 
{
    reference operator*() const { return *m_ptr; }
    pointer operator->() { return m_ptr; }

    // Prefix increment
    Iterator& operator++() { m_ptr++; return *this; }  

    // Postfix increment
    Iterator operator++(int) { Iterator tmp = *this; ++(*this); return tmp; }

    friend bool operator== (const Iterator& a, const Iterator& b) { return a.m_ptr == b.m_ptr; };
    friend bool operator!= (const Iterator& a, const Iterator& b) { return a.m_ptr != b.m_ptr; };     

private:

    pointer m_ptr;
};
```

Utiliser l'iterator

```c++
for (auto it = integers.begin(), end = integers.end(); it != end; ++it) { 
	const  auto i = *it; std::cout << i << "\n"; 
}
```
