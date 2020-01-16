---
marp: true
theme: simple
class:
  - lead
paginate: true
header: 'Pragmatic Programmers - Refactoring to functional style'
footer: 'Dawid Bińkuś & Mateusz Małecki'

---
<!-- _class: invert -->
# Refactoring to functional style
Pragmatic Programmers - Dawid Bińkuś & Mateusz Małecki  

---
# Agenda
##### Programowanie imperatywne a funkcyjne
- Definicja programowania imperatywnego
- Definicja programowiania funkcyjnego
##### Zastosowanie
- Biblioteka Stream
- Wstrzykiwanie funkcji
---
# Programowanie imperatywne

- Proces wykonywania - sekwencja instrukcji zmieniających stan programu
- Ciąg komend (instrukcji) do wykonania przez komputer

---

```java
class Dog {
  public String name;
  public String breed;
}

class DogService{
  public List<Dog> getAllDogsWithBreed(List<Dog> dogs, String breedToFind){
    List<Dog> result = new ArrayList<Dog>();
    for(int i = 0; i < dogs.size(); i++){
      Dog dog = dogs.get(i);
      if(dog.breed == breedToFind)
        result.append(dog);
    }

    return result;
  }
}
```

---
# Programowanie funkcyjne

- Wyznaczanie jedynie wartości wyrażeń, nie sposobu ich wykonywania
- Niezmienność danych
- Modularność

---

```java
class Dog {
  public String name;
  public String breed;
}

class DogService{
  public List<Dog> getAllDogsWithBreed(List<Dog> dogs, String breedToFind){
    dogs.stream()
      .filter(dog -> dog.breed.equals(breedToFind))
      .collect(Collectors.toList());
  }
}
```

---
## Table

|      | col1 | col2 | col3 |
| ---- | ---- | ---- | ---- |
| row1 | item | item | item |
| row2 | item | item | item |
| row3 | item | item | item |

---

## Code block
FizzBuzz by Python.
```java
int test(){
   return 2+2; 
}
```
