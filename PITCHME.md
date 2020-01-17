---
marp: true
theme: simple
class:
  - lead
paginate: true
header: 'Pragmatic Programmers'
footer: 'Dawid Bińkuś & Mateusz Małecki'
---
<!-- _class: invert -->
# Przewidywalny kod
###### Za pomocą wzorców funkcyjnych
<br>
Pragmatic Programmers - Dawid Bińkuś & Mateusz Małecki

---
# Ale dlaczego?
- Funkcyjne rozwiązania stają się coraz bardziej popularne,
- Rozwiązania funkcyjne dobrze dopełniają się z zasadami SOLID,
- Elementy funkcyjne pojawiają się w najbardziej popularnych językach programowania - Lambda (Java), LINQ (C#).
---
# Brak efektów ubocznych
---
## Czym jest efekt uboczny?
- Zauważalny efekt wykraczający poza główny cel operacji (zwrócenie wartości),


- Modyfikacja stanu poza obrębem funkcji.

---
```Java
User getUserByEmailAndPassword(String email, String password){
  User user = UserService.getByEmailAndPassword(email, password);
  if (user != null) {
    LoginService.loginUser(user);
  }
  return user;
}
```
---
```java
User getUserByEmailAndPassword(String email, String password){
  User user = UserService.getByEmailAndPassword(email, password);
  if (user != null) {
    LoginService.loginUser(user); //(Side effect!)
  }
  return user;
}
```
- Aktualizacja stanu aplikacji bez wiedzy programisty.
- Proces logowania użytkownika powienien istnieć jako osobna funkcja.
---
# Niezmienialność (immutability)
```
  Set<Customer> customers = new HashSet<>();
  customers.add(customer);
  processCustomer(customer);
```

- Czy obiekt *customer* ma dalej takie samo pole **id** i **name**?
- Czy w kolekcji *customers* nadal znajduje się obiekt customer? 
---
```
  Set<ImmutableCustomer> customers = new HashSet<>();
  customers.add(customer);
  ImmutableCustomer changedCustomer = processCustomer(customer);
```
---
# Unikamy wartości null
---
### Przykład - pobieranie użytkownika z bazy:
```scala
CustomerRepository repository = new CustomerRepository();
Customer myCurrentCustomer = repository.findCustomerById(42);
System.out.println(myCurrentCustomer.getName());
```

- Co się stanie jeśli użytkownik o danym numerze **id** nie istnieje? 

---
### Możliwe powody wystąpienia wartości null w programie:
- Obiekt nigdy nie został zainicjalizowany,
- Obiekt nie jest poprawny,
- Obiekt nie jest potrzebny,
- Obiekt nie istnieje,
- I wiele, wiele więcej.
---
### Pierwsze rozwiązanie - null check
```java
CustomerRepository repository = new CustomerRepository();
Customer myCurrentCustomer = repository.findCustomerById(42);
if (myCurrentCustomer != null)) //Wszędzie pojawiają się null checki!
  System.out.println(myCurrentCustomer.getName());
```

- Za każdym razem gdy chcemy pobrać użytkownika z bazy musimy się upewnić że jest on prawidłowy, tj. nie jest wartością **null** i to obsłużyć.

---
### Możliwe rozwiązanie - Opakowanie CustomerOrError
``` java
CustomerRepository repository = new CustomerRepository();
CustomerOrError myCurrentCustomer = repository.findCustomerById(42);
if(myCurrentCustomer.isCustomer()){
  System.out.println(myCurrentCustomer.get().getName());
}
```

- Możliwe jest również użycie obiektu typu *Optional*
---
# Wstrzykiwanie funkcji jako argumenty
---
```java
class Order{
  private Long id;
  private List<Item> items;
  /*
    methods and accessors ...
  */
}
class Item{
  private String name;
  private Double weight;
  private Double price;
    /*
    methods and accessors ...
  */
}
```
---
```java
public List<Double> calculateOrdersWeight(List<Order> orders){
  List<Double> result = new ArrayList<>();
  for(Order o : orders){
    result.add(o.getOrderWeight());
  }
  return result;
}
```
---
```java
  List<Double> calculateOrdersWeight(List<Order> orders);
  List<Double> calculateOrdersPrice(List<Order> orders);
```
- Co jeśli będziemy chcieli dodać nową funkcjonalność?
---
# Rozwiązanie - wstrzykiwanie funkcji.
```java
  public List<Double> calculateOrders(List<Order> orders, Function<Order, Double> f) {
    List<Double> result = new ArrayList<>();
    for(Order o : orders){
      result.add(f.apply(o));
    }
    return result;
  }
```
---
```java
calculateOrders(input, o -> o.getOrderWeight());
calculateOrders(input, o -> o.getOrderSize());
calculateOrders(input, o -> o.getOrderPrice());
```
---
# Dziękujemy za uwagę