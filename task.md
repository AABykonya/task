```
bank=# select * from client order by id;
```

 id |              fio               | date_of_birth |    phone    
----+--------------------------------+---------------+-------------
  1 | Тиньков Олег Юрьевич           | 1967-12-25    | 79123452334
  2 | Греф Герман Оскарович          | 1964-02-08    | 79051883232
  3 | Ким Чен Ын                     | 2010-07-12    | 79154325678
  4 | Дуров Павел Валерьевич         | 1984-10-10    | 79103124590
  5 | Помидоров Платон Галактионович | 1931-10-10    | 79999999999
  6 | Багров Данила Сергеевич        | 1975-08-05    | 79999869034
(6 rows)


```
bank=# select * from card order by 2;
```

 id | client_id |    cardnumber    | expire_date 
----+-----------+------------------+-------------
  1 |         1 | 4444456825443212 | 2038-11-11
  2 |         1 | 5444456345443223 | 2038-11-11
  3 |         1 | 2444456348243223 | 2038-11-11
  4 |         2 | 1234435654364356 | 2038-11-11
  5 |         3 | 1213123124364356 | 2038-11-11
  6 |         4 | 3564945124364356 | 2038-11-11
  7 |         5 | 4398573424349944 | 2038-11-11
(7 rows)




1. Найти ФИО клиентов из таблицы client, у которых нет ни одной карты в таблице card, результат отсортировать по ФИО.

```
bank=# select cl.fio from client cl 
left join card cd on cl.id = cd.client_id 
where cd.client_id is NULL order by cl.fio;
```

fio           
-------------------------
 Багров Данила Сергеевич
(1 row)


2. Найти ФИО клиентов, у которых имеется более двух карт.

```
bank=# select cl.fio, count(cd.client_id) from client cl 
join card cd on cl.id = cd.client_id 
group by cl.fio 
having count(cd.client_id) > 2 
order by cl.fio;
```

         fio          | count 
----------------------+-------
 Тиньков Олег Юрьевич |     3
(1 row)

3. Вывести список клиентов, у которых есть и карта Visa (cardnumber начинается на «4»), и карта Mastercard (cardnumber начинается на «5»).

```
bank=# with Visa as (select client_id from card cd where cd.cardnumber like '4%'), 
Master as (select client_id from card cd where cd.cardnumber like '5%') 
select fio from client where id in (select V.client_id from Visa V join Master M on V.client_id = M.client_id);
```

 fio          
----------------------
 Тиньков Олег Юрьевич
(1 row)

4. Вывести список клиентов, фамилия которых начинается на «П» и заканчивается на «В».


  
  
  
