DROP DATABASE IF EXISTS glpi;
CREATE DATABASE glpi CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
ALTER USER 'glpi'@'localhost' IDENTIFIED BY 'debian';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpi'@'localhost';
FLUSH PRIVILEGES;
EXIT;


sudo chown -R www-data:www-data /var/www/html/glpi
sudo find /var/www/html/glpi -type d -exec chmod 755 {} +
sudo find /var/www/html/glpi -type f -exec chmod 644 {} +
sudo chmod -R 775 /var/www/html/glpi/files
sudo chmod 755 /var /var/www /var/www/html


sudo ln -s /var/www/html/glpi/install /var/www/html/glpi/public/install
sudo systemctl restart apache2





 Шаг 1. Создание тестовых данных

Что делаем: В веб-интерфейсе GLPI создаем компьютер Testbackupk.
Команда: (не нужна, делается мышкой в браузере)
Смысл: Появляются данные, которые потом будем проверять после восстановления.

---

 Шаг 2. Создание резервной копии

Что делаем: Запускаем скрипт бэкапа.

```bash
sudo /home/debian/backup_glpi.sh
```

Смысл: Скрипт создает 2 файла в /backup/glpi:

· glpi_files_ДАТА.tar.gz — архив файлов сайта
· glpi_db_ДАТА.sql — дамп базы данных

---

 Шаг 3. Проверка, что бэкап создан

```bash
ls -lh /backup/glpi/
```

Смысл: Убеждаемся, что оба файла на месте.

---

 Шаг 4. Очистка тестовых данных и удаление файлов

4.1. В браузере удаляем компьютер Testbackupk (кнопка «Удалить навсегда»).
4.2. Имитируем сбой — переименовываем папку сайта:

```bash
sudo mv /var/www/html/glpi /var/www/html/glpi_broken
```

Смысл: Сайт «сломан», данных нет — имитация аварии.

---

 Шаг 5. Восстановление файлов из архива

```bash
sudo tar -xzvf /backup/glpi/glpi_files_2026-10-07.tar.gz -C /
```

Смысл: Распаковываем архив обратно в /var/www/html/glpi.

Затем возвращаем права:

```bash
sudo chown -R www-data:www-data /var/www/html/glpi
sudo chmod -R 755 /var/www/html/glpi
```

Смысл: Веб-сервер снова может читать файлы.

---

 Шаг 6. Восстановление базы данных

6.1. Заходим в MySQL:

```bash
sudo mysql
```

6.2. Удаляем пустую базу и создаем новую:

```sql
DROP DATABASE IF EXISTS glpi;
CREATE DATABASE glpi CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
ALTER USER 'glpi'@'localhost' IDENTIFIED BY 'debian';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpi'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

6.3. Загружаем данные из дампа:

```bash
sudo mysql -u glpi -pdebian glpi < /backup/glpi/glpi_db_2026-10-07.sql
```

Смысл: Файл .sql содержит команды, которые заново создают все таблицы с данными.

---

 Шаг 7. Перезапуск и проверка

```bash
sudo systemctl restart apache2
```

Смысл: Перезапускаем веб-сервер. Заходим в браузер → GLPI → «Компьютеры» → видим Testbackupk на месте.

---

 Шаг 8. Удаление «сломанной» папки (если всё ок)

```bash
sudo rm -rf /var/www/html/glpi_broken
```

Смысл: Убираем мусор, всё уже работает из восстановленной копии.


