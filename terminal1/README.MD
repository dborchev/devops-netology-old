# Домашнее задание к занятию "3.1. Работа в терминале, лекция 1"

>1. Установите средство виртуализации [Oracle VirtualBox](https://www.virtualbox.org/).

✅
>1. Установите средство автоматизации [Hashicorp Vagrant](https://www.vagrantup.com/).

✅

1. С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:

    * Создайте директорию, в которой будут храниться конфигурационные файлы Vagrant. В ней выполните `vagrant init`. Замените содержимое Vagrantfile по умолчанию следующим:

        ```bash
        Vagrant.configure("2") do |config|
            config.vm.box = "bento/ubuntu-20.04"
        end
        ```
    ✅
    * Выполнение в этой директории `vagrant up` установит провайдер VirtualBox для Vagrant, скачает необходимый образ и запустит виртуальную машину. ✅

    * `vagrant suspend` выключит виртуальную машину с сохранением ее состояния (т.е., при следующем `vagrant up` будут запущены все процессы внутри, которые работали на момент вызова suspend), `vagrant halt` выключит виртуальную машину штатным образом.✅

2. Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?
   1. По-умолчанию выделено 2 vCPU, 1024 MB памяти, 64 GB диск, 1 сетевой адаптер эмулирующий Intel PRO/1000, 4MB видеопямяти, директория хоста содержащая Vagrantfile замаплена с полным доступом 

3. Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: [документация](https://www.vagrantup.com/docs/providers/virtualbox/configuration.html). Как добавить оперативной памяти или ресурсов процессора виртуальной машине?
   1. Добавить памяти или процессора машине можно используя параметры `memory`, `cpus` провайдера (здесь: `virtualbox`) соответсвенно; например так: https://github.com/dborchev/devops-netology/commit/8f2609df0d48eecb5f37b9772ded7025417a54fe

4. Команда `vagrant ssh` из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu. ✅


5. Ознакомиться с разделами `man bash`, почитать о настройках самого bash:
    * какой переменной можно задать длину журнала `history`, и на какой строчке manual это описывается?
      * переменная `HISTSIZE`, описывается начиная со строки 1043
    * что делает директива `ignoreboth` в bash?
      * в контексте переменной `HISTCONTROL`, определяющей порядок сохранния команд в историю, значение `ignoreboth` приводит к игнорированию дубликатов команд и команд начинающихся с пробела
6. В каких сценариях использования применимы скобки `{}` и на какой строчке `man bash` это описано?
   1. для задания выполнения списка команд в текущем окружении (строка 273)
      1. как частный случай, для определения тела функции (строка 427), аналогично напр. языку С
   2. для адресации переменных (т.н. `Parameter Expansion`), см. начиная со строки 1254;
      1. как частный случай, для указателя на переменную хранения файлового дескриптора перенаправления вывода (см. строки 1600 и далее)
   3. в качестве генератора строк (т.н. `Brace Expansion`), см. начиная со строки 1171
   4. для описания формата даты-времени в преглашении комангдной строки, см начиная со строки 2314
7. С учётом ответа на предыдущий вопрос, как создать однократным вызовом `touch` 100000 файлов? 

создать 100000 файлов можно так:
```bash
vagrant@vagrant:/tmp/tmp.KcxemblWfb$ touch {0..99999}
vagrant@vagrant:/tmp/tmp.KcxemblWfb$ ls | wc -l
100000
```
Получится ли аналогичным образом создать 300000? Если нет, то почему?
- нет, потому что экспансия отдает все сгенерированные элементы в touch скопом, и список аргументов превышает допустимую длину
  - определена в конфигурационной переменной ядра `PATH_VAR`  и на тестовой машине составляет `2097152` байт
- предел нашелся на значении 147057 файлов

8. В man bash поищите по `/\[\[`. Что делает конструкция `[[ -d /tmp ]]`
   1. ``[[ ... ]]`` возврещает 0 или 1 в зависимости от истинности выражения в скобках
   2. конструкция проверяет существование директории (`-d`), в данном случае - `/tmp`
   3. конструкция аналогична команде `test`, и просто передает ей агрументы
9. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

    ```bash
    bash is /tmp/new_path_directory/bash
    bash is /usr/local/bin/bash
    bash is /bin/bash
    ```

    (прочие строки могут отличаться содержимым и порядком)
    В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.

```bash
vagrant@vagrant:~$ sudo ln -s /usr/bin/bash /usr/local/bin/bash
vagrant@vagrant:~$ mkdir /tmp/new_path_directory/
vagrant@vagrant:~/tmp/tmp.nE7h9ioIYR$ sudo ln -s /usr/bin/bash /tmp/new_path_directory/bash
vagrant@vagrant:~/tmp/tmp.nE7h9ioIYR$ export PATH=/tmp/new_path_directory:/usr/local/bin:/bin
vagrant@vagrant:~$ type -a bash
bash is /tmp/new_path_directory/bash
bash is /usr/local/bin/bash
bash is /bin/bash
```


10. Чем отличается планирование команд с помощью `batch` и `at`?
- `at` исполняет заданные команды в заданное время
- `batch` исполняет команды при загрузке системы ниже 1.5
  - на нашей тестовой машине загрузка 0.00, поэтому команда исполняется немедленно

11. Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука. ✅

