# S21_CHEET_SHEET
### Гайд по противодействию школе 21

## Компиляция и запуск
**gcc -std=c11 -Wall -Werror -Wextra -Wconversion -Warith-conversion file.c -o file.out & file.out**</br></br>
**-std=c11** задает компилятору стандарт С11, который не допускает применение пары forbidden functions</br>
**-Wall & -Wextra** подсвечивает дополнительные неточности в коде как возможные ошибки</br>
**-Werror** делает возможные ошибки a.k.a. предупреждения ошибками и фейлит процесс компиляции на этапе препроцессинга</br>
**-Wconversion & -Warith-conversion** делает ошибкой неявное приведение типов. Полазив вечером по флагам gcc подметил его, т.к. в некоторых заданиях используется неявное приведение. Условно тот же getchar возвращает значение типа int, а не char, и можно словить на этом ошибку. К тому же С относится к языкам статической типизации, а по сему считаю неявное приведение ошибкой и способ к лучшему написанию кода.</br>

## Проверка на валид ввода
Основная проверка, которая проста в освоении и проходит абсолютно все, что только возможно, имеет следующий вид</br>
```С
#include <stdio.h>

#define args_num 2 // количество аргументов внутри scanf или по-простому количество включений символа '%' внутри

int main() {
  int var;
  if(scanf("%i", &var) == args_num) {
    // ... something good
  } else {
    // ... meh 
  }
  return 0;
}
```
> P.S. вариант с **getchar() == '\n'** и **char c** делают лишние махинации с буфером дробной части числа и валятся автотестами. Простое друг хорошего, простейшее друг лучшего 

## Что проверяют и используют автотесты?(теории заговора 21)
Вероятно всего автотесты построены через парсинг заранее готовых .txt файлов скриптом на сервере.</br>
Это обьясняет то, что они не дают на вход специальные символы прерывания потока ака процессорная термизация.</br>(**ctrl+c** условно прервет код и не даст ему завершиться = ошибка, но автотесты нормально кушают это)</br>
Дают много граничных случаев и вкидывают значения, которые крашат си по-математическим правилам. Любые операции с нулями имеют высокий порог багануться. Мб еще двоичное представление используется, хызы.</br>
Проверяют на соответствие стилям согласно **Google C формат**.</br>
Для стандартизации своего кода достаточно скопировать файл .clang-format из /materials/linters/ в рабочую директорию с *.c и прописать </br>
**clang-format -n file.c**.</br>
Проверяют на эффективность кода утилитой **cppcheck**. Показывает "лишние переменные", более близкие области определения переменных к месту их использования и тд в целях оптимизации</br>
**cppcheck --enable=all --suppress=missingIncludeSystem file.c file.h**</br>
Проверка на утечки "file.out" **valgrind**</br>
**valgrind --tool=memcheck --leak-check=yes file.out**</br>
> P.S. Данные по результатам первого экзамена и будет дополняться в процессе
