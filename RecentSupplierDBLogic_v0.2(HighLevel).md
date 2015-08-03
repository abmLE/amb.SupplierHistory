#FR-001
> Как конечных пользователь приложения, хочу выбирать поставщика как из общего перечня, так и из списока "избранных" (ранее активных) для товара/склада. Это позволит быстрее переназначать поставщика и как результат упростит работу с системой.

##Цель FR:
Для улучшения usability, отображать в __SKU Card__(SKU data) информацию о последних Поставщиках.

##Дизайн диалогового окна:
- [Screen As-is](https://photos-4.dropbox.com/t/2/AADlk2ntM2M6k49E2s1zYbBZznLa1J_eCR8gIQmY9_ZfBQ/12/450548831/png/32x32/1/_/1/2/SuppliersAsIs.PNG/EKau9dEDGCQgBygH/kFie2qLuLNve01eTDs5moc4E6f3SBjvGO02b3tC53w8?size=1024x768&size_mode=2)
- [Mockup To-be](https://www.dropbox.com/s/6f4vn3x198uewjo/SuppliersFB_v0.1.PNG?dl=0)

##Решение на уровне БД:
Сохранять в отдельной Таблице _(например, RecentSupplier)_ "историю" Поставщиков.

###Поля таблицы:
- SKU {Product_id} 
- Warehouse {Warehouse_id} 
- SupplierCode {Supplier_id} 

Т.е. Supplier_id для SKU+Warehouse. 

А также, возможно:
- Main {true,false} - флаг, Поставщик активный/неактивный, а можно брать в таблице __skubody__
- Date_end - дата, когда Поставщик перестал быть Активным.

##Примечание:
Поставщик может быть отредактирован путем

1. Импорта из *.csv (плановый/внеплановый)
2. Выгрузки *.xls
3. Редактирования через интерфейс приложения

##Логика:

![alt text](https://photos-4.dropbox.com/t/2/AABIXcYPYrKt05ri3m-ZuGC0VxRNVzrwkEi0g-egVQ8iIQ/12/450548831/png/32x32/1/_/1/2/RecentSupplierLogicv1.PNG/EKau9dEDGCQgBygH/Isk9SLg1sATHrfFScrbHdDLK5clCEjM8IvsvMlcVYjE?size=1024x768&size_mode=2 "Logo Title Text 1")

1. Сравнить "новых" Поставщиков c данными из Таблицы. 
<br>Искать среди активных (флаг Main=1) по уникальному ключу SKU & Warehouse. _(это ускорит поиск благодаря снижению количества проверок)_.
Если все ровно -> Конец
2. Если появился новый Main поставщик для SKU & Warehouse _(для уникального ключа SKU & Warehouse)_:
2.1. Поставщик есть среди неактивных (Main=0) -> меняем флаг активности Main=1, обновляем дату (Date_beg=TODAY)
2.2. Поставщик отсутствует с таблице RecentSupplier  -> добавляем новую запись в Таблицу. Задаем "новичку" флаг Main=1 _(это может быть значением по умолчанию для всех новых Поставщиков)_ и проставляем дату Date_beg=TODAY.

3. Старому (предыдущему) Поставщику изменить статус (флаг Main=0)
