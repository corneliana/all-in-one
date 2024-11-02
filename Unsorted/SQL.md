**Structural Query Language**

table A `LEFT JOIN` table B `ON` A.id = B.id
LEFT JOIN will maintain or increase the number of records in table A.
- increase: if there are multiple records in B whose id equals the same id in A. Then in the joined result, the the records in table A will be copied
