JavaRush test task Задание на стажировку. Нужно дописать приложение, которое ведет учет космических кораблей в далеком будущем (в 3019 году). Должны быть реализованы следующие возможности:
получать список всех существующих кораблей;
создавать новый корабль;
редактировать характеристики существующего корабля;
удалять корабль;
получать корабль по id;
получать отфильтрованный список кораблей в соответствии с переданными фильтрами;
получать количество кораблей, которые соответствуют фильтрам. Для этого необходимо реализовать REST API в соответствии с документацией.
В проекте должна использоваться сущность Ship, которая имеет поля: Long id ID корабля String name Название корабля (до 50 знаков включительно) String planet Планета пребывания (до 50 знаков включительно) ShipType shipType Тип корабля Date prodDate Дата выпуска.
Диапазон значений года 2800..3019 включительно Boolean isUsed Использованный / новый Double speed Максимальная скорость корабля. Диапазон значений 0,01..0,99 включительно. Используй математическое округление до сотых. Integer crewSize Количество членов экипажа. Диапазон значений 1..9999 включительно. Double rating Рейтинг корабля. Используй математическое округление до сотых.
Также должна присутствовать бизнес-логика: Перед сохранением корабля в базу данных (при добавлении нового или при апдейте характеристик существующего), должен высчитываться рейтинг корабля и сохраняться в БД. Рейтинг корабля рассчитывается по формуле: 4. Если параметр pageNumber не указан – нужно использовать значение 0. 5. Если параметр pageSize не указан – нужно использовать значение 3. 6. Не валидным считается id, если он:
не числовой
не целое число
не положительный
При передаче границ диапазонов (параметры с именами, которые начинаются на «min» или «max») границы нужно использовать включительно.
REST API. Get ships list URL /ships Method GET URL Params Optional: name=String planet=String shipType=ShipType after=Long before=Long isUsed=Boolean minSpeed=Double maxSpeed=Double minCrewSize=Integer maxCrewSize=Integer minRating=Double maxRating=Double order=ShipOrder pageNumber=Integer pageSize=Integer Data Params None Success Response Code: 200 OK Content: [ { “id”:[Long], “name”:[String], “planet”:[String], “shipType”:[ShipType], “prodDate”:[Long], “isUsed”:[Boolean], “speed”:[Double],
“crewSize”:[Integer],
“rating”:[Double] }, … ] Notes Поиск по полям name и planet происходить по частичному соответствию. Например, если в БД есть корабль с именем «Левиафан», а параметр name задан как «иа» - такой корабль должен отображаться в результатах (Левиафан).
pageNumber – параметр, который отвечает за номер отображаемой страницы при использовании пейджинга. Нумерация начинается с нуля
pageSize – параметр, который отвечает за количество результатов на одной странице при пейджинге
Get ships count URL /ships/count Method GET URL Params Optional: name=String planet=String shipType=ShipType after=Long before=Long isUsed=Boolean minSpeed=Double maxSpeed=Double minCrewSize=Integer maxCrewSize=Integer minRating=Double maxRating=Double Data Params None Success Response Code: 200 OK Content: Integer Notes
Create ship URL /ships Method POST URL Params None Data Params { “name”:[String], “planet”:[String], “shipType”:[ShipType], “prodDate”:[Long], “isUsed”:[Boolean], --optional, default=false “speed”:[Double],
“crewSize”:[Integer] } Success Response Code: 200 OK Content: { “id”:[Long], “name”:[String], “planet”:[String], “shipType”:[ShipType], “prodDate”:[Long], “isUsed”:[Boolean], “speed”:[Double],
“crewSize”:[Integer],
“rating”:[Double] } Notes Мы не можем создать корабль, если:
указаны не все параметры из Data Params (кроме isUsed);
длина значения параметра “name” или “planet” превышает размер соответствующего поля в БД (50 символов);
значение параметра “name” или “planet” пустая строка;
скорость или размер команды находятся вне заданных пределов;
“prodDate”:[Long] < 0;
год производства находятся вне заданных пределов. В случае всего вышеперечисленного необходимо ответить ошибкой с кодом 400.
Get ship URL /ships/{id} Method GET URL Params id Data Params None Success Response Code: 200 OK Content: { “id”:[Long], “name”:[String], “planet”:[String], “shipType”:[ShipType], “prodDate”:[Long], “isUsed”:[Boolean], “speed”:[Double],
“crewSize”:[Integer],
“rating”:[Double] } Notes Если корабль не найден в БД, необходимо ответить ошибкой с кодом 404. Если значение id не валидное, необходимо ответить ошибкой с кодом 400.
Update ship URL /ships/{id} Method POST URL Params id Data Params { “name”:[String], --optional “planet”:[String], --optional “shipType”:[ShipType], --optional “prodDate”:[Long], --optional “isUsed”:[Boolean], --optional “speed”:[Double], --optional “crewSize”:[Integer] --optional } Success Response Code: 200 OK Content: { “id”:[Long], “name”:[String], “planet”:[String], “shipType”:[ShipType], “prodDate”:[Long], “isUsed”:[Boolean], “speed”:[Double],
“crewSize”:[Integer],
“rating”:[Double] } Notes Обновлять нужно только те поля, которые не null. Если корабль не найден в БД, необходимо ответить ошибкой с кодом 404. Если значение id не валидное, необходимо ответить ошибкой с кодом 400.
Delete ship URL /ships/{id} Method DELETE URL Params id Data Params
Success Response Code: 200 OK Notes Если корабль не найден в БД, необходимо ответить ошибкой с кодом 404. Если значение id не валидное, необходимо ответить ошибкой с кодом 400.