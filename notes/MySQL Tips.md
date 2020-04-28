---
tags: [MySQL]
title: MySQL Tips
created: '2020-04-28T01:13:18.118Z'
modified: '2020-04-28T03:21:51.204Z'
---

# MySQL Tips

## tips:
    use indexes:
        - index on commonly queried fields
        - good tradeoff for speed
    normalize your data and use joins:
        - store data separate from one another
        - reference that data with foreign keys
    use innodb:
        - myisam going away
        - innodb all around better
    enforce data integrity at the db level:
        - unique constraints
        - cascade deletes
    log queries in development:
        - great to see queries happening in real time
        - disable in production
    use consistent naming:
        - use short, easy to type field names
        - whichever naming format you choose, do it everywhere
    use transactions for bulk insert:
        - each insert triggers a reindex, this gets by that
        - no visibility of new data mid insert


---
indexes:
alter table table_name add index index_name (column);


instead of:
posts table
    - post_id
    - title
    - author_name

do:
posts table
    - post_id
    - title
    - author_id
authors table
    - author_id
    - name


to log queries, update my.cnf and add/change
```
general_log_file = /var/log/mysql.log
general_log = 1
```

bad naming:
posts table
    - ID
    - title
    - contentBody
    - created_at

also bad:
posts table
    - post_id
    - post_title
    - post_content_body
    - post_created_at

good naming:
posts table
    - post_id
    - title
    - content
    - created_at


five reindexes
```
insert into whatever values (default, 1);
insert into whatever values (default, 2);
insert into whatever values (default, 3);
insert into whatever values (default, 4);
insert into whatever values (default, 5);
```

one reindex
```
start transaction;
insert into whatever values (default, 1);
insert into whatever values (default, 2);
insert into whatever values (default, 3);
insert into whatever values (default, 4);
insert into whatever values (default, 5);
commit;
```
