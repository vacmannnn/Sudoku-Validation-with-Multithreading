# Обзор

Данная программа валидирует сетку судоку. Запускается в несколько потоков, в каждом из них валидируется каждая колонка, каждый ряд и подполе (квадратик 3х3). Изначально был общий флаг `valid`,
обращение к которому осуществлялся с каждого потока.

# Utlis

Исполняемый фалй запускался после компиляции с флагами `g++ -fsanitize=thread -lpthread -g main.cpp`, в результате были найдены следующие ошибки (26 штук):

```
WARNING: ThreadSanitizer: data race (pid=174301)
  Write of size 1 at 0x558e99058164 by thread T26:
    #0 subGrid8(void*) /home/vacman/Sudoku-Validation-with-Multithreading/main.cpp:1347 (a.out+0x3fec) (BuildId: 724d1dce41f2dc2a3166956a93e3ca880d86da7d)

  Previous write of size 1 at 0x558e99058164 by thread T25:
    #0 subGrid7(void*) /home/vacman/Sudoku-Validation-with-Multithreading/main.cpp:1295 (a.out+0x3e10) (BuildId: 724d1dce41f2dc2a3166956a93e3ca880d86da7d)

  Location is global 'valid' of size 1 at 0x558e99058164 (a.out+0x7164)
```

Санитайзер правильно указал на гонку данных и действительно программа выдавала неккоректные результаты из-за этого

# Моя работа

Вместо добавления гонки, я решил ее исправить. Под каждую функцию проверки выделялся собственный флаг, после чего проходил по флагам и валидировал сетку.

В результате санитайзер перестал ругаться, а программа работает корректно.
