```SQL
CREATE OR REPLACE TABLE gokay_varcok.content_category
as
SELECT * FROM sample.content_category;

CREATE OR REPLACE TABLE gokay_varcok.content_category_20201222_00_59
as
SELECT * FROM sample.content_category_20201222_00_59;


merge gokay_varcok.content_category c1
using gokay_varcok.content_category_20201222_00_59 c2
        on c1.id = c2.id
when matched and c1.cdc_date!=c2.cdc_date then
        update set c1.category = c2.category,c1.cdc_date=c2.cdc_date
when not matched by target then
        insert (cdc_date, is_deleted,id,category)
        values (c2.cdc_date,c2.is_deleted,c2.id,c2.category)
when not matched by source then
        UPDATE SET c1.is_deleted = true

-------farkına bakmak için-----
WITH t1 as(
SELECT
  *,
  farm_fingerprint(to_json_string( CONCAT(cdc_date, is_deleted, id, category) )) as _hash1
FROM
  gokay_varcok.content_category
)

SELECT *
FROM t1
RIGHT JOIN (SELECT
  *,
  farm_fingerprint(to_json_string( CONCAT(cdc_date, is_deleted, id, category) )) as _hash1
FROM
  gokay_varcok.content_category_target) t2
ON t1.id = t2.id
WHERE t1._hash1 <> t2._hash1
```
