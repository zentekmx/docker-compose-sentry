
sentry-compose
========================

docker-compose for sentry at http://sentry.io

Setup
---------------

Create the .env file for docker secrets:

```
SENTRY_SECRET_KEY=...  # 32 random chars
SENTRY_POSTGRES_HOST=...
SENTRY_DB_USER=...
SENTRY_DB_PASSWORD=...
```

Run with docker-compose:

```
docker-compose up -d
docker-compose exec sentry sentry upgrade
docker-compose restart sentry
```

Reverse proxy config:
```
    http {
        # ...

        upstream web {
            server localhost:9000;
        }
        server {
            listen 80 default_server;
            server_name _;

            location / {
                    proxy_pass http://web;
                    proxy_set_header X-Real-IP $remote_addr;
                    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header Host $host;
            }
        # ...
```

Getting Started
---------------

#### Django
Append to production.py settings:

```
import sentry_sdk
from sentry_sdk.integrations.django import DjangoIntegration
from sentry_sdk.integrations.celery import CeleryIntegration

# ...
sentry_sdk.init(
    dsn=os.environ.get('SENTRY_DSN'),
    integrations=[DjangoIntegration(), CeleryIntegration()],
    environment='production'
)
```

Extras
---------------
#### Integrations
For installing plugins, run:

```
docker-compose exec sentry pip install sentry-slack
```

* https://github.com/getsentry/sentry-redmine - Sentry integration for creating Redmine issues
* https://github.com/getsentry/sentry-github - Sentry extension which integrates with GitHub
* https://github.com/getsentry/sentry-phabricator - Sentry extension which integrates with Phabricator
* https://github.com/getsentry/sentry-pagerduty - Sentry plugin for integrating with PagerDuty
* https://github.com/getsentry/sentry-teamwork - Sentry plugin that integrates with Teamwork
* https://github.com/getsentry/sentry-heroku - Sentry extension which integrates Heroku release tracking
* https://github.com/getsentry/sentry-freight - Sentry extension which integrates with Freight release tracking
* https://github.com/getsentry/sentry-youtrack - Sentry extension which integrates with YouTrack
* https://github.com/getsentry/sentry-bitbucket - Sentry extension which integrates with Bitbucket
* https://github.com/getsentry/sentry-jira - Plugin for sentry that lets you create JIRA issues
* https://github.com/getsentry/sentry-irc - Plugin for Sentry that logs errors to an IRC room
* https://github.com/getsentry/sentry-trello - Plugin for Sentry that creates cards on a Trello board
* https://github.com/getsentry/sentry-campfire - Sentry plugin for sending notifications to Campfire
* https://github.com/getsentry/sentry-groveio - Plugin for Sentry that logs errors to an IRC room on Grove.io
* https://github.com/getsentry/sentry-irccat - Plugin for Sentry which sends errors to irccat (or any other service which supports irccat's simple socket-based protocol)
* https://github.com/getsentry/sentry-slack - Slack integration for Sentry
* https://github.com/linovia/sentry-hipchat - Sentry plugin that integrates with Hipchat
* https://github.com/butorov/sentry-telegram - Plugin for Sentry which allows sending notification via Telegram messenger
* https://github.com/mattrobenolt/sentry-twilio - A plugin for Sentry that sends SMS notifications via Twilio
* https://github.com/Banno/getsentry-kafka - An Apache Kafka plugin for Sentry

Reference
---------------

Take a look at the docs for more information:

* https://sentry.io
