#1980’den itibaren spor grubu bazında en çok madalya alan 1. 3. 5. ülkeyi bulalım.

'''SQL
Select country,sport from
    (Select country,
    sport,
    count(medal) sum_medal,
    ROW_NUMBER() OVER(partition by Sport order by count(Medal) desc) num
    from `dsmbootcamp.gokay_varcok.summer_medals` 
    where year >1979 
    group by country,sport
    )a 
where num in (1,3,5);
'''
