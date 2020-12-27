#1980’den itibaren herhangi bir spor grubunda üst üste 3 veya daha fazla madalya almış atletleri bulalım.   

```SQL

select distinct m1.Athlete 
from `dsmbootcamp.gokay_varcok.summer_medals` m1
join (select Year,Athlete from `dsmbootcamp.gokay_varcok.summer_medals`) m2 
    on m1.Year = m2.Year+4 and m1.Athlete = m2.Athlete
join (select Year,Athlete from `dsmbootcamp.gokay_varcok.summer_medals`) m3 
    on m1.Year = m3.Year+8 and m1.Athlete = m3.Athlete
where m2.Athlete is not null and m3.Athlete is not null

```
