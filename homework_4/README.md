# Homework #4 :atom:

Homework for the class **'Loading the OS and processes'**

Linux. Workstation - Geekbrains

# List of the tasks

## Task #1

#### Ru

**Потоки ввода/вывода**

Создать файл, используя команду ```echo```. Используя команду ```cat```, прочитать содержимое каталога etc, ошибки перенаправить в отдельный файл.

#### En

**Input/Output flows**

Create a file using the ```echo``` command. Using the command ```cat```, read the contents of the directory etc, errors redirected to a separate file.

## Task #2

#### Ru

**Конвейер (pipeline)**

Использовать команду ```cut``` на вывод длинного списка каталога, чтобы отобразить только права доступа к файлам. Затем отправить в конвейере этот вывод на ```sort``` и ```uniq```, чтобы отфильтровать все повторяющиеся строки.

#### En

**Pipeline**

Use the ```cut``` command to display a long directory list to show only file access rights. Then send this output to ```sort``` and ```uniq``` in the conveyor to filter out all repetitive lines.

## Task #3

#### Ru

**Управление процессами**

Изменить конфигурационный файл службы ```SSH: /etc/ssh/sshd_config```, отключив аутентификацию по паролю ```PasswordAuthentication no```. Выполните рестарт службы ```systemctl restart sshd (service sshd restart)```, верните аутентификацию по паролю, выполните ```reload``` службы ```systemctl reload sshd (services sshd reload)```. В чём различие между действиями ```restart``` и ```reload```? Создайте файл при помощи команды ```cat > file_name```, напишите текст и завершите комбинацией ```ctrl+d```. Какой сигнал передадим процессу?

#### En

**Process Management**

Change the service configuration file ```SSH: /etc/ssh/sshd_config``` by disabling password authentication ```PasswordAuthentication no```. Restart the service ```systemctl restart sshd (service sshd restart)```, return password authentication, perform reload ```systemctl reload sshd (services sshd reload)```. What is the difference between ```restart``` and ```reload```? Create a file using the command ```cat > file_name```, write the text and finish with the combination ```ctrl+d```. Which signal will we send to the process?

## Task #4

#### Ru

**Сигналы процессам**

Запустите ```mc```. Используя ```ps```, найдите PID процесса, завершите процесс, передав ему сигнал 9.

#### En

**Process signals**

Run ```mc```. Using ```ps```, find the PID of the process, finish the process by sending it signal 9.

# Contributing

Pull requests are welcome.

# Source

[Geekbrains](https://geekbrains.ru)
