---
marp: true
theme: simple
class:
  - lead
paginate: true
header: 'Pragmatic Programmers - Refactoring to predictable code'
footer: 'Dawid Bińkuś & Mateusz Małecki'
---
<!-- _class: invert -->
# Refactoring to predictable code
###### Using functional patterns
<br>
Pragmatic Programmers - Dawid Bińkuś & Mateusz Małecki

---
# Agenda
xxxxx

---
# What and why
- Kotlin > Java
- Języki funkcyjne stanowią coraz większy % rynku
- Elementy funkcyjne pojawiają się w najbardziej popularnych językach programowania - Lambda (Java), LINQ (C#)
---
# Brak efektów ubocznych
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
  processCustomer(customer);
```
---
# No nulls allowed
---
### Możliwe powody wystąpienia wartości null w programie:
- Obiekt nigdy nie został zainicjalizowany,
- Obiekt nie jest poprawny,
- Obiekt nie jest potrzebny,
- Obiekt nie istnieje,
- Coś się zepsuło :(
---
### Przykład - pobieranie użytkownika z bazy:
```scala
CustomerRepository repository = new CustomerRepository();
Customer myCurrentCustomer = repository.findCustomerById(42);
System.out.println(myCurrentCustomer.getName());
```

Co się stanie jeśli użytkownik o danym numerze **id** nie istnieje? 

---
### Pierwsze rozwiązanie - null check
```java
CustomerRepository repository = new CustomerRepository();
Customer myCurrentCustomer = repository.findCustomerById(42);
if (myCurrentCustomer != null)) //Wszędzie pojawiają się null checki!
  System.out.println(myCurrentCustomer.getName());
```

Za każdym razem gdy chcemy pobrać użytkownika z bazy musimy się upewnić że jest on prawidłowy, tj. nie jest wartością **null** i to obsłużyć.

---
### Możliwe rozwiązanie - Opakowanie Optional
``` scala
CustomerRepository repository = new CustomerRepository();
Optional<Customer> myCurrentCustomer = repository.findCustomerById(42);
myCurrentCustomer.ifPresent(customer -> System.out.println(customer.getName()));
```

Powyższe rozwiązanie ze względu na naturę obiektu *Optional*, wymusza na programiście upewnienie się, że dana wartość istnieje.