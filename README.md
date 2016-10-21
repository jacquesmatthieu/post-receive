#!/bin/sh

#Post receive in DEV
#THIS HOOK SWITCH BRANCH

WEB_DIR="/home/webdev/wooma"
GIT_PATH="deploy.git"
APP_PATH="site/htdocs"
ENV="staging"

PROJECT="Wooma"
ENV_URL="https://dev.frontendeveloper.fr/wooma/site/htdocs/"
CHANNEL="general"
REPO_NAME="wooma > refonte"
SLACK_HOOK="https://hooks.slack.com/services/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"

DATE=$(date +%Y%m%d%H%M)


#3. Run actions and logged in
#-----------------------------------------------------------------------------------------------

echo "[log] Received push request at $( date +%F)"
echo "[log] Starting $ENV Deploy..."
echo "[log] - Starting code update"

#3. Run actions and logged in
echo "[log] Received push request at $( date +%F)"

#Read revisions
while read oldrev newrev ref
  do
    branch=`echo $ref | cut -d/ -f3-10`
    echo -e "[log] - Old SHA: $oldrev New SHA: $newrev Branch Name: $ref"
    echo "[log] - Old SHA: $oldrev New SHA: $newrev Branch Name: $ref"
    echo "$branch ref received.  Deploying $branch branch to /home/webdev/wooma/site/htdocs"
    SHA=$newrev
    REF=$ref
    git --work-tree=/home/webdev/wooma/site/htdocs --git-dir=/home/webdev/wooma/deploy.git checkout $branch -f
  done

echo 'run composer build dev'
cd /home/webdev/wooma/site/htdocs/wp-content/themes/wooma/
composer build-dev


#6. Slack
curl -X POST --data-urlencode 'payload={"channel": "#general", "username": "webhookbot", "text": "This is posted to #general and comes from a bot named webhookbot.", "icon_emoji": ":ghost:"}' $SLACK_HOOK
