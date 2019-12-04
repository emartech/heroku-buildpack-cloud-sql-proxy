# Google Cloud SQL proxy buildpack 

This heroku buildpack adds the Google Cloud SQL proxy to your app to enable
connections to SQL instances in the GCP.

## Prerequisite

- Google service account with the Cloud SQL/Cloud SQL client role
- JSON key for the service account
- Google Cloud SQL instance set up with a postgresql user able to connect 
to the database

## Install

Add the proxy to your buildpacks. It's important that this buildpack should
not be the last one in the list, as that's used by heroku to determine your 
startup processes. (--index=1)

     heroku buildpacks:add --index=1 https://github.com/emartech/heroku-buildpack-cloud-sql-proxy
     
Add the GCP JSON credentials as `GSP_CREDENTIALS` env variable to you app.

Set the instance the proxy should connect to with the `GSP_INSTANCES` env 
variable. Format: `<instance connection name>=tcp:<local port>`. You can get the
**instance connection name** from the Cloud SQL console overview.

Set the connection string for your DB library to 
`postgres://<username>:<password>@localhost:<local port>/<database-name>`

Start the proxy before your app tries to connect to the database by e.g. adding 
`bin/run_cloud_sql_proxy` to the `.procfile` in the root of your project.
