
#!/bin/bash

target_branch="BRANCH NAME"
working_tree="PATH"

ENV="production"

PROJECT=PROJECT NAME"
ENV_URL="PROJECT DOMAINE URL"
CHANNEL="general"
REPO_NAME="REPO NOAME"
SLACK_HOOK=""

DATE=$(date +%Y%m%d%H%M)

while read oldrev newrev refname
do
    branch=$(git rev-parse --symbolic --abbrev-ref $refname)
    if [ -n "$branch" ] && [ "$target_branch" == "$branch" ]; then

       GIT_WORK_TREE=$working_tree git checkout $target_branch -f
       NOW=$(date +"%Y%m%d-%H%M")
       git tag release_$NOW $target_branch

       echo "   /==============================="
       echo "   | DEPLOYMENT COMPLETED"
       echo "   | Target branch: $target_branch"
       echo "   | Target folder: $working_tree"
       echo "   | Tag name     : release_$NOW"
       echo "   \=============================="
    fi
done


#6. Slack
curl -X POST --data-urlencode 'payload={"channel": "#'$CHANNEL'", "username": "meetxsweatbot", "text": "'$( date +%F)' New deploy on '$ENV'.$


