# database.ini

### Local:

```ini
[database_real_time]
host = localhost
port = 5432
name = zio_center_real_time
account = zio_center_user
password = 123456
max_connections = 200
max_idle_connections = 200
# minutes
max_connection_lifetime = 5
debug = false

[database_record]
host = localhost
port = 5432
name = zio_center_record
account = zio_center_user
password = 123456
max_connections = 200
max_idle_connections = 200
# minutes
max_connection_lifetime = 5
debug = false

[database]
db_table_limit_rows = 1000000

[mongo]
host = localhost
port = 27017
name = zio_center
account = root
password = 123456
enable_cache = yes
```

### alpha：

```ini
[database_real_time]
host = 61.66.104.98
port = 54038
name = zio_center_real_time
account = zio_center_user
password = 123456
max_connections = 200
max_idle_connections = 200
# minutes
max_connection_lifetime = 5
debug = false

[database_record]
host = 61.66.104.98
port = 54038
name = zio_center_record
account = zio_center_user
password = 123456
max_connections = 200
max_idle_connections = 200
# minutes
max_connection_lifetime = 5
debug = false

[database]
db_table_limit_rows = 1000000

[mongo]
host = localhost
port = 27017
name = zio_center
account = root
password = 123456
enable_cache = yes

```
