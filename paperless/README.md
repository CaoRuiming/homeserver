# Paperless-ngx

Reference: https://docs.paperless-ngx.com/setup/

To set up a cron job (`crontab -e`) to create backups of application state:

```
45 19 * * * cd ~/Projects/homeserver/paperless && docker compose exec -T webserver document_exporter ../export --delete --no-progress-bar
```
