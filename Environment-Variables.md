## Environment Variables

This is a complete list of available envrionment variables; required variables are marked with a `✓`.

|Variable|Description|Required|Default|
|---|---|:---:|---|
|IGDB_CLIENT_ID|Client ID for IGDB API|✓||
|IGDB_CLIENT_SECRET|Client Secret for IGDB API|✓||
|DB_HOST|Host name of MariaDB instance|✓|`localhost`|
|DB_PORT|Port number of MariaDB instance||`3306`|
|DB_NAME|Should match MYSQL_DATABASE in mariadb||`romm`|
|DB_USER|Should match MYSQL_USER in mariadb|✓||
|DB_PASSWD|Should match MYSQL_PASSWORD in mariadb|✓||
|REDIS_HOST|Host name of Redis instance|✓|`localhost`|
|REDIS_PORT|Port number of Redis instance||`6379`|
|REDIS_PASSWORD|Only set this if Redis is password protected|||
|ROMM_AUTH_USERNAME|Username for default user||`admin`|
|ROMM_AUTH_PASSWORD|Password for default user||`admin`|
|ROMM_AUTH_SECRET_KEY|Generate a key with `openssl rand -hex 32`|✓||
|ROMM_HOST|Host name of ROMM instance||`localhost`|
|ENABLE_RESCAN_ON_FILESYSTEM_CHANGE|Enable rescanning of library when filesystem changes||`false`|
|RESCAN_ON_FILESYSTEM_CHANGE_DELAY|Delay in minutes before rescanning library when filesystem changes||`5`|
|ENABLE_SCHEDULED_RESCAN|Enable scheduled rescanning of library||`false`|
|SCHEDULED_RESCAN_CRON|Cron expression for scheduled rescanning||`"0 3 * * *"`|
|ENABLE_SCHEDULED_UPDATE_SWITCH_TITLEDB|Enable scheduled updating of Switch TitleDB index||`false`|
|SCHEDULED_UPDATE_SWITCH_TITLEDB_CRON|Cron expression for scheduled updating of Switch TitleDB||`"0 4 * * *"`|
|ENABLE_SCHEDULED_UPDATE_MAME_XML|Enable scheduled updating of MAME XML index||`false`|
|SCHEDULED_UPDATE_MAME_XML_CRON|Cron expression for scheduled updating of MAME XML||`"0 5 * * *"`|