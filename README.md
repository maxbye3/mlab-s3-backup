# mLab S3 Backup

This is a node project that makes backing up your mLab databases to S3.
The config file specifies which database to back-up and includes cron "time" field to explicitly backup at a set time.

## Installation
<ul>
<li> Clone or download this repo. </li>
<li> To configure the backup, you need to pass the binary a JSON configuration file </li>
<li> Open <code>config.json</code> in your favourite code editor </li>
</ul>


## Configuration

The file should have the following format:

    {
      "sesTransport": {
        "to": "YOUR_EMAIL",
        "from": "SES_TRANSPORT_SERVER_EMAIL",
        "key": "SES_TRANSPORT_SERVER_KEY",
        "secret": "SES_TRANSPORT_SERVER_SECRET",
        "region": "SES_TRANSPORT_SERVER_REGION"
      },
      "mongodb": {
        "host": "SES_KEY",
        "port": PORT_NUMBER,
        "username": USERNAME,
        "password": PASSWORD,
        "db": "DATABASE_TO_BACKUP"
      },
      "s3": {
        "key": "YOUR_S3_KEY",
        "secret": "YOUR_S3_SECRET",
        "bucket": "SS_BUCKET_TO_UPLOAD_TO",
        "destination": "/",
        "encrypt": TRUE,
        "region": "S3_REGION_TO_USE"
      },
      "cron": {
        "time": "11:59",
      }
    }

A breakdown on how to setup each of these fields follows.

### Config.json SES (Simple Email Service) Transport Fields
Nodemailer will email you if the s3 server is unreachable or there is a problem with extracting database data.
The SES Transport fields are used to configure nodemailer. 
However, emailing errors is entirely optional and leaving these fields blank will not effect the backup.
If you work for Rebel Minds, bother Max Bye for these details.
Otherwise, a configured Amazon SES is recommended and setup instructions can be found <a href="https://aws.amazon.com/ses/" target="_blank">here</a>. 


### Crontabs

You may optionally substitute the cron "time" field with an explicit "crontab"
of the standard format `0 0 * * *`.

      "cron": {
        "crontab": "0 0 * * *"
      }

*Note*: The version of cron that we run supports a sixth digit (which is in seconds) if
you need it.

### Timezones

The optional "timezone" allows you to specify timezone-relative time regardless
of local timezone on the host machine.

      "cron": {
        "time": "00:00",
        "timezone": "America/New_York"
      }

You must first `npm install time` to use "timezone" specification.

## Running

To start a long-running process with scheduled cron job:

    mongodb_s3_backup <path to config file>

To execute a backup immediately and exit:

    mongodb_s3_backup -n <path to config file>
