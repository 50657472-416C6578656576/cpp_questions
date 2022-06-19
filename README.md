# cpp_questions
Ответы на вопросы по ООП № [37](#37), [38](#38), [39](#39), [40](#40), [41](#41), [43](#43), [50](#50), [51](#51), [52](#52), [53](#53) из [таблицы](https://vk.com/away.php?to=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F1Xa1adOa6OHZmitgNn0bf2ZkWPHSHtjVP4N1kiDtX_wo%2Fedit%3Fusp%3Dsharing&amp;el=snippet)

---

## Вопрос №37.

### В  чём  различия  обработки  поступившего  указателя  в  delete и delete[]?

- delete - используется, чтобы отменить выделение памяти для отдельных объектов.
- delete - используется, чтобы отменить выделение памяти для массивов объектов.

### Наглядно в коде:
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
