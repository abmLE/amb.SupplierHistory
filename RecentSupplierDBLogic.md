Цель:
<br>Хранить в таблице RecentSupplier "историю" Поставщиков товара (SKU) на склад (Warehouse)

<br>Поля:
<ul><li>SKU {Product_ID}</li>
<li>Warehouse {Warehouse_ID}</li>
<li>SupplierCode {Supplier_ID}</li>
<li>Main {0,1} - флаг, Поставщик активный/неактивный</li> 
<br>а также <i>(например)</i><br>
<li>ItemDescription</li>
<li>SupplierDimension</li>
<li>SupplierUnitMultiplier</li></ul>

Триггер:
<ol><li>Импорт из *.csv (плановый/внеплановый)</li>
<li>Выгрузка *.xls </li>
<li>Редактирование Поставщика через Интерфейс</li></ol>
Логика:
<ol><li>Сравнить "новых" Поставщиков c данными из таблицы RecentSupplier. Искать среди активных (флаг Main=1) по уникальному ключу SKU & Warehouse. <i>(на Main=0 поставщиков можно пока "забить")</i>.
<br>Если все ровно -> Конец</li>
<li>Если появился новый Main поставщик для SKU & Warehouse <i>(для уникального ключа SKU & Warehouse)</i>
<ol><li>Поставщик есть среди неактивных (Main=0) -> меняем флаг активности Main=1</li>
<li>Поставщик отсутствует с таблице RecentSupplier  -> добавляем новую запись в таблицу RecentSupplier. Задаем "новичку" флаг Main=1 <i>(это может быть значением по умолчанию для всех новых Поставщиков)</i>.</li></ol>
<li>Старому (предыдущему) Поставщику изменить статус (флаг Main=0) </li>
</ol>
