
<br>Таблица RecentSupplier
<br>Поля:
<ul><li>SKU {Product_ID}</li>
<li>Warehouse {Warehouse_ID}</li>
<li>SupplierCode {Supplier_ID}</li>
<li>Main {0,1}</li>
<br>а также <i>(например)</i><br>
<li>ItemDescription</li>
<li>SupplierDimension</li>
<li>SupplierUnitMultiplier/li></ul>
Тригер:
<ol><li>Импорт из *.csv (плановый/неплановый)</li>
<li>Выгрузка *.xls </li>
<li>Редактирование Потавщика через Интерфейс</li></ol>
Логика:
<ol><li>Сравнить Поставщиков c данными из таблицы RecentSupplier. Искать среди активных (флаг Main=1) по уникальному ключу SKU & Warehouse. <i>(на Main=0 поставщиков можно пока "забить")</i>.
<br>Если все ровно - Конец</li>
<li>Если появился новый Main поставщик для SKU & Warehouse - проверить, возможно, он есть в таблице <i>среди неактивных (Main=0)</i>
<br>Если в списке Поставщика нет <i>(он новый для уникального ключа SKU & Warehouse</i> - добавляем новую запись в таблицу RecentSupplier. Задаем "новичку" флаг Main=1 <i>(это может быть значением по умолчанию для всех новых Поставщиков)</i>.</li>
<li>Старому Поставщику изменить статус (флаг Main=0) </li>
</ol>
