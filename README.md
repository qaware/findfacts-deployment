# findfacts-deployment
Deployment repository for the [findfacts](https://www.github.com/qaware/isabelle-afp-search) project.

Deployment configs are stored here additively so that older versions still run when re-deploying.

There is a small tool, `update-deployed.sh` that creates a new deployment with the files from templates
(and is able to do some variable replacement for consistent version numbers).
Set the version number variables in this script appropriately.

__Don't change the `deployed` folder manually!__

## How to deploy
Requirements: `docker` >= 18, `docker-compose`

Steps to deploy:
1. Check out and `cd` into repo
2. Create and set application secret:

   ```shell
   head -c 32 /dev/urandom | base64
   ```
   Set result as value in `deployed/app/app.env` for `APPLICATION_SECRET` key
3. Set hostname in `deployed/app/server.env`
4. Start infrastructure

   ```shell
   cd deployed/infrastructure
   docker-compose up -d
   ```
   (If detach does not work for some reason, use `ctrl+z` to leave + run in background)
5. Start db, then app (same way as infrastructure)

#### Services
You can then reach the following endpoints:
 - 80/443: reverse-proxy
 - 3000: app
 - 8983: solr
 - 514,601,6514: syslog
 
So make sure only ports 80 and 443 are exposed to the web.

Logs are collected in the `infrastructure_logs` volume (can also be accessed from `prod_syslog` container under `/logs`).
