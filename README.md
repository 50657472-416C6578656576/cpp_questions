# cpp_questions
Ответы на вопросы по ООП № 37, 38* 50, 51, 52, 53 из [таблицы](https://docs.google.com/spreadsheets/d/1Xa1adOa6OHZmitgNn0bf2ZkWPHSHtjVP4N1kiDtX_wo/edit#gid=0)

---

## Вопрос №37.

### В  чём  различия  обработки  поступившего  указателя  в  `delete` и `delete[]`?

`delete` возвращает память, выделенную оператором `new`, обратно в кучу. Вызов `delete` должен происходить для каждого вызова `new`, чтобы избежать утечки памяти. После вызова `delete` объект, указывающий на этот участок памяти, становится некорректным и не должен больше использоваться (хорошая практика - присвоить указателю `nullptr`).

В чем же отличие `delete` от `delete[]`?
- `delete` - используется, чтобы отменить выделение памяти для отдельных объектов.
- `delete[]` - используется, чтобы отменить выделение памяти для массивов объектов. Вызов `delete[]` для массива объектов приведет к вызову деструктора для каждого объекта перед освобождением памяти, выделенной под массив.

Использование `delete[]` вместо `delete` не всегда ведет к утечке памяти: казалось бы, что должна освободиться память только указателя, но иногда память, выделенная на массив объектов, также освобождается. В любом случае, каждый из них необходимо использовать в определенных выше случаях - иначе, зачастую, результат будет непредсказуем и зависеть от конкретного случая неверного использования.

### Наглядно в коде (взят из [документации](https://docs.microsoft.com/ru-ru/cpp/cpp/delete-operator-cpp?view=msvc-170)):
```cpp
struct UDType
{
};

int main()
{
   // Allocate a user-defined object, UDObject, and an object
   //  of type double on the free store using the
   //  new operator.
   UDType *UDObject = new UDType;
   double *dObject = new double;
   // Delete the two objects.
   delete UDObject;
   delete dObject;
   // Allocate an array of user-defined objects on the
   // free store using the new operator.
   UDType (*UDArr)[7] = new UDType[5][7];
   // Use the array syntax to delete the array of objects.
   delete [] UDArr;
}
```

---

## Вопрос №38 (Вероятно, у Коли есть [дополненная версия](https://github.com/Nick440Suvoro/OOP-exam-questions/blob/main/%D0%92%D0%BE%D0%BF%D1%80%D0%BE%D1%81%2038.md)).

### Перегрузка двуместных операций `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`.

Все эти операции перегружаются примерно одинаково и должны возвращать некое значение. Так как такие операции являются двуместными, то их реализация происходит с помощью дружественной классу функции (`friend`).

### Рассмотрим operator+ для сложения двух объектов класса `Integer`:
Пусть класс выглядит следующим образом (просто значение типа `int` обернутое в класс):
```cpp
class Integer
{
private:
    int value;
public:
    Integer(int i): value(i) {}
    
    friend const Integer operator+(const Integer& left, const Integer& right);
};
```

Далее вне класса определяем дружественный `operator+`. В качестве аргументов двуместного оператора выступают два значения: левый и правый операнд, желательно передаваемые по константным ссылкам.
```cpp
const Integer operator+(const Integer& left, const Integer& right) {
    return Integer(left.value + right.value);
}
```

---

## Вопрос №50 и 51.

### Способы наследования private, protected, public и спецификаторы в родительском классе private, protected, public.

Спецификаторы доступа:
- `public` - доступ открыт всем;
- `protected` — доступ открыт только классам, производным от данного;
- `private` — доступ открыт самому классу и друзьям (`friend`) данного класса: как функциям, так и классам. Однако даже производные классы не получают доступа к этим данным.

```cpp
class MyClass {
  
  public:    // Public access specifier
    int x;   // Public attribute
  
  protected:   // Protected access specifier
    int y;   // Protected attribute
  
  private:   // Private access specifier
    int z;   // Private attribute
    
};


class MyChild : public MyClass {
   public:
      void poke()
      {
         x = 10;     // Allowed (public)
         y = 1010;   // Allowed (protected)
         z = 101010; // Not allowed (private)
      }
};



int main() {
  MyClass myObj;
  myObj.x = 25;  // Allowed (public)
  myObj.y = 50;  // Not allowed (protected)
  myObj.z = 75;  // Not allowed (private)
  
  return 0;
}
```

Типы наследования:
- публичный (`public`) - публичные (`public`) и защищенные (`protected`) данные наследуются без изменения уровня доступа к ним;
- защищенный (`protected`) - все унаследованные данные становятся защищенными (`protected`);
- приватный (`private`) - все унаследованные данные становятся приватными (`private`).

---

## Вопрос №52.

### Что доступно в классе, порождённом от порождённого от базового (2 ступень наследования) при первичном наследовании public?

---

## Вопрос №53.

### Что доступно в классе, порождённом от порождённого от базового (2 ступень наследования) при первичном наследовании protected?
