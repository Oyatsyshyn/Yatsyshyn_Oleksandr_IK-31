# **Лабораторна робота №2А**
---
## Послідовність виконання лабораторної роботи:
#### 1. Переглянув офіційну документацію для ***Python***.
#### 2. Створюю файл ***test.py*** для виконання базових прикладів.
1. Виводжу вбудованні константи за допомогою команд:

    ```python
    print("Перша константа: ", True)
    print("Друга константа: ", False)
    print("Третя константа: ", NotImplemented)
    ```
1. Виводжу результат роботи вбудованих функцій за допомогою команд:
   ```python
    print("Число 20 в 16-ві системі числення дорівнює: ", hex(20))
    print("2 в 10 степені дорівнює: ", pow(2,10))
    print("Максимальне число з чисел 5,6,9,3,45 :",max(5,6,9,3,45))
    ```
1. Виводжу результат роботи циклу і розгалужень за допомогою команд:
    ```python
    x= [1 for i in range(10)]
    print("Виведення 10 одиниць з масиву: ",x)

    b=5
    print("Змінна B дорівнює 5" if b == 5 else "Змінна B не дорівнює 5")
    ```
1. Виводжу результат роботи конструкції `try`->`except`->`finally` при помилці за допомогою команд:
    ```python
    y=[5,2]
    print("Що буде якщо вивести 10 елемент масиву y[]?: ")
    try:
        print(y[10])
    except Exception as e:
        print(e)
    finally:
        print("То ось що буде!")
    ```
1. Виводжу результат роботи контекст-менеджера `with` за допомогою команд:
    ```python
    i=1
    with open("README.md", "r") as file:
        for line in file:
            print("Рядок " + str(i) + ": " + line)
            i=i+1
    ```
1. Виводжу результат роботи з `lambdas` за допомогою команд:
    ```python
    new_lambda = lambda first_number, second_number: f'Сума першого і другого числа дорівнює {first_number + second_number}'
    print("Розташування функції в памяті: ", new_lambda)
    print("Виклик лямбди: ", new_lambda(5, 6))
    ```
#### 3. Створив у власному рипозеторії такі файли:
   ```text
   lab2a/
   ├── modules/
   │   └── common.py
   ├── __init__.py
   └── __main__.py
   ```
1. Перейшовши у папку з даними файлами запустив виконання програми цією командою:

    ```sh
    python3 .
    ```
    Виконання команди:
    ```text
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ python3 .
    We are in the __main__
    2021-10-12 00:28:11.192473
    linux
    test
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ 
    ```
1. Після запуску команди `python3 .` програма в першому рядку виводить назву файла який виконувався, в другому рядку виводиться час і дата виконання даної програми, в третьому рядку виводиться ос на якій було запущено програму і в четвертому рядку виведено текст "test".

    a. Після запуску команди: `python3 . -h` в консоль виводиться інформація про додаткові параметри та їх використання. Результат виконання команди:
    ```text
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ python3 . -h
    usage: . [-h] [-o OPT] [-l]

    Приклад передачі аргументів у Python програму.

    optional arguments:
      -h, --help            show this help message and exit
      -o OPT, --optional OPT
                            Цей параметр є вибірковим.
      -l, --logs            Якщо виконати команду з цим параметром будуть
                            виводитись логи.
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ 
    ```
    
    b. Після запуску команди: `python3 . -o "Цей текст також має вивестись"` в консоль виводиться інформація така сама як і в пункті 2. тільки додається текст про те що з консолї було передано аргумент і саме повідомлення яке ми передаємо. Результат виконання команди:
    ```text
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ python3 . -o "Цей текст також має вивестись"
    We are in the __main__
    2021-12-12 23:16:16.039055
    linux
    З консолі було передано аргумент
    ========== >> Цей текст також має вивестись << ==========
    test
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ 
    ```
    c. Дитально ознайомився з аргументами.
    d. Ознайомився з логуванням і запустив команду `python3 . --logs`.
    Результат виконання команди:
    ```text
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ python3 . --logs
    2021-12-12 23:27:40,615 root INFO: Тут буде просто інформативне повідомлення
    2021-12-12 23:27:40,615 root WARNING: Це Warning повідомлення
    2021-12-12 23:27:40,615 root ERROR: Це повідомлення про помилку
    test
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ 
    ```
1. Створив власну функцію у файлі `common.py` яка буде виводить всі парні числа від 0 до 100,  якщо у функцію передати значення True і непарні якщо значення False. Виклик цієї функцію виконую з `__main__` .
Код власної функції:
    ```python
    def filtr_number(filtr):
        numbers=range(0,101)
        if filtr=="True":
    	    msg = "Парні елементи: " 
        elif filtr=="False":
    	    msg = "Непарні елементи: "
    	    
        for num in numbers:
        	if (filtr == "True") & (num%2 == 0):
        	    msg += str(num) + " "
        	elif (filtr == "False") & (num%2 != 0):
        	    msg += str(num) + " "
        return msg
    ```
    Результат виконання з параметром `-o True`:
    ```text
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ python3 . -o True
    We are in the __main__
    2021-10-13 17:52:53.195220
    linux
    Парні елементи: 0 2 4 6 8 10 12 14 16 18 20 22 24 26 28 30 32 34 36 38 40 42 44 46 48 50 52 54 56 58 60 62 64 66 68 70 72 74 76 78 80 82 84 86 88 90 92 94 96 98 100 
    З консолі було передано аргумент
     ========== >> True << ==========
    test
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ 
    ```
    Результат виконання з параметром `-o False`:
    ```text
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ python3 . -o False
    We are in the __main__
    2021-10-13 17:33:01.366747
    linux
    Непарні елементи: 1 3 5 7 9 11 13 15 17 19 21 23 25 27 29 31 33 35 37 39 41 43 45 47 49 51 53 55 57 59 61 63 65 67 69 71 73 75 77 79 81 83 85 87 89 91 93 95 97 99 
    З консолі було передано аргумент
     ========== >> False << ==========
    test
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ 
    ```
1. Створив функцію яка може виконуватись з помилкою. У випадку її виникнення виводить `ERROR` повідомлення за допомогою логування використовуючи бібліотеку `logging`. Якщо функція виконалася без помилки то виводить `INFO` повідомлення.
Код власної функції:
    ```python
    def view_array():
        x=[5,9,6,3]
        print("Масив X[]:", x)
        index = int(input("Введіть номер елемента масиву який хочете вивести: "))
        try:
        	print(f"X[{index}] = {x[index]}")
        except IndexError:
            logging.error("Ви ввели число за межами проміжку 0-3")
        else:
        	logging.info("Ви ввели коректні дані")
    ```
    Результат виконання з помилкою:
    ```text
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ python3 .
    We are in the __main__
    2021-12-12 19:02:46.290364
    linux
    Масив X[]: [5, 9, 6, 3]
    Введіть номер елемента масиву який хочете вивести: 9
    2021-12-12 19:02:02,561 root ERROR: Ви ввели число за межами проміжку 0-3
    test
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ 
    ```
    Результат виконання без помилки:
    ```text
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ python3 .
    We are in the __main__
    2021-12-12 19:02:30.064026
    linux
    Масив X[]: [5, 9, 6, 3]
    Введіть номер елемента масиву який хочете вивести: 2
    X[2] = 6
    2021-12-12 19:02:36,204 root INFO: Ви ввели коректні дані
    test
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab2a (master) $ 
    ```
