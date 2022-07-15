# Secure-connection

## З'єднання через ssh-keys

Ви навчитеся:
* З'єднуватися через ssh-key
* Забороняти з'єднання через пароль (+90% до твоєї впевненості)

### З'єднання через ssh-key
## Відкриваємо комп'ютер з якого хочемо заходити на сервер
Спершу отримуємо права root
```
sudo su
```

Тепер ми повинні згенерувати публічний і приватнний ключі. Це можна зробити такою командою

```
ssh-keygen
```
Тебе запитає в якій дерикторії ти хочеш зберегти його. Натискаємо Enter і зберігаємо по замовчуванню (/root/.ssh/id_rs)

Далі якщо ти перший раз створюєш ключі, тобі запропонує зразу створити парольнуфразу її можна проігнорувати просто натиснувши Enter, але вона може вам колись і знадобитися

![Знімок екрану з 2022-07-08 11-59-19](https://user-images.githubusercontent.com/102728347/177958332-2f60fcd5-1b6d-429e-ad3b-ebfd663c02c6.png)

Якщо у вас вже були створені ключі ви побачите це(якщо ви виберете `yes` то вже попередні ключі не можна буде вернути тому будьте з цим обережні)

![Знімок екрану з 2022-07-08 11-58-12](https://user-images.githubusercontent.com/102728347/177958366-d9e889f3-b02d-4aa8-831e-50e4d8d5e796.png)

Якщо ви створили успіно ключ ось що ви побачите наступне

![Знімок екрану з 2022-07-08 14-19-19](https://user-images.githubusercontent.com/102728347/177982810-a1e9062a-f05b-4059-ae00-54c32f98bebd.png)


Переходимо до деректорії `~/.ssh`. Там будуть знаходитись такі файли `id_rsa` і `id_rsa.pub`
```
cd .ssh/ && ls
```

![Знімок екрану з 2022-07-08 14-20-40](https://user-images.githubusercontent.com/102728347/177982764-440ea812-7d9a-47ef-a6fc-fc722a910687.png)


Виводимо на екран все що знаходиться в файлі `id-rsa.pub`
```
cat ~/.ssh/id_rsa.pub
```
Публічнний ключ буде починатися на ssh-rsa AAAA... 

## Працюємо з сервером

Для цього:
* Заходимо на сервер
 ```
 ssh user@ip
 ```
 
* Переходимо в дерикторію `~/.ssh`
```
mkdir -p ~/.ssh/ && cd ~/.ssh 
```

* Записуємо публічний ключ в файл `authorized_keys`, якщо файла немає створіть його командою `touch authorized_keys`(Замість pubkey вводимо наш ключ який ми виверли на екран на компютері)
```
echo pubkey >> ~/.ssh/authorized_keys
```

Вітаю!!!
Тепер ми можемо підключитися з нашого компютера до нашого сервера не водячи паролю


### Заборонити з'єднання через пароль

Для цього ми переходимо до такої дирикторії `/etc/ssh`

```
cd /etc/ssh
```
Тепер нам потрібно відредагувати файл `sshd_config`

```
nano sshd_config
```

В sshd_config занходимо такий параметер `PasswordAuthentication` його значення буде дорівнюувати `yes`, він буде виглядати так.

![Screenshot from 2022-07-15 15-43-42](https://user-images.githubusercontent.com/102728347/179226485-1bef9eac-3d70-468d-8541-8bdcfa3b3624.png)

![Screenshot from 2022-07-15 15-44-06](https://user-images.githubusercontent.com/102728347/179226533-0920c8ab-d325-475f-8ef7-3f00301c778d.png)

А нам потрібно щоб він виглядав так

![Screenshot from 2022-07-15 15-50-33](https://user-images.githubusercontent.com/102728347/179226553-c5bee4b1-7ec1-4797-b933-94d42ba9700f.png)


Зберігаємо.
Тепер останій крок, щоб воно запрацювало. Терба перезапустити ssh сервіс.
```
service ssh restart
```
