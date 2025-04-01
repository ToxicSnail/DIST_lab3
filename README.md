# Лабораторная работа: DLL Injection с использованием C++ Win32 API

## Описание
Данный проект демонстрирует механизм внедрения DLL в другой процесс с использованием Windows API. Реализованы следующие компоненты:

- **TargetProcess.exe**  
  Приложение-мишень, которое постоянно выводит сообщения в консоль. В него будет внедряться DLL.

- **VirusDLL.dll**  
  DLL, которая при загрузке выводит MessageBox с сообщением "BOOM!", подтверждая успешную инъекцию.

- **DllInjectorAsDll.dll**  
  DLL-инжектор, экспортирующий функцию `HelperFunc`, которая получает PID целевого процесса и запускает процедуру инъекции (с использованием OpenProcess, VirtualAllocEx, WriteProcessMemory, GetProcAddress и CreateRemoteThread).

- **DLLInjectorAsProcess.exe**  
  Отдельное приложение-инжектор, выполняющее аналогичный процесс инъекции через консоль.

- **DLLLoader.exe**  
  Программа для загрузки DLL через функцию LoadLibrary – для тестирования корректности сборки DLL.

- **Prize.html**  
  HTML-страница, использующая ActiveX в Internet Explorer для вызова `rundll32.exe` и запуска инъекции через DLL-инжектор.

- **Java GUI**  
  Дополнительное Java-приложение с графическим интерфейсом (на базе Swing), которое автоматически находит PID процесса TargetProcess.exe и запускает инъекцию.

## Требования
- **ОС:** Windows (или среда, поддерживающая Win32 API)
- **Компилятор C++:** mingw-w64, Visual Studio или другой, поддерживающий Win32 API
- **Java JDK:** для компиляции и запуска Java GUI
- **Internet Explorer:** для работы Prize.html (ActiveX работает только в IE)

## Сборка
### C++ компоненты
Соберите исходные файлы в соответствующие исполняемые файлы и DLL. Пример сборки с помощью mingw-w64:
- **TargetProcess.exe:**  
```bash
x86_64-w64-mingw32-g++ -static -o TargetProcess.exe TargetProcess.cpp
```
- **VirusDLL.dll:**  
```
x86_64-w64-mingw32-g++ -static -shared -Wl,--subsystem,windows -o VirusDLL.dll VirusDLL.cpp
```bash
- **DllInjectorAsDll.dll:**  
```bash
x86_64-w64-mingw32-g++ -static -shared -Wl,--subsystem,windows -o DllInjectorAsDll.dll DllInjectorAsDll.cpp
```
- **DLLInjectorAsProcess.exe:**  
```bash
x86_64-w64-mingw32-g++ -static -o DLLInjectorAsProcess.exe DLLInjectorAsProcess.cpp
```
- **DLLLoader.exe:**  
```bash
x86_64-w64-mingw32-g++ -static -o DLLLoader.exe DLLLoader.cpp
```
### Java GUI
1. Скомпилируйте Java-файл:
```bash
javac DllInjectorGUI.java
```
2. Запустите приложение:
```bash
java DllInjectorGUI
```
## Запуск
1. **Запустите TargetProcess.exe** – он будет работать как процесс-мишень.
2. **Инъекция DLL:**
- Через консольное приложение:  
  Запустите `DLLInjectorAsProcess.exe` с параметром PID целевого процесса.
- Через DLL-инжектор (DllInjectorAsDll.dll):  
  Используйте команду:
  ```
  rundll32.exe "полный_путь\DllInjectorAsDll.dll" HelperFunc <PID>
  ```
- Через Java GUI:  
  Нажмите кнопку в приложении, чтобы автоматически найти PID и выполнить инъекцию.

## Примечание
Проект создан в образовательных целях для демонстрации методов DLL-инъекции с использованием Win32 API. Использование подобных техник без разрешения является незаконным и может нанести вред.

## Лицензия
Этот проект распространяется под лицензией MIT.


