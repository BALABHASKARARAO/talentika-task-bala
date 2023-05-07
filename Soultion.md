## 1. How do you set up a script to run every time a repository receives new commits through push?

* ans: this can be two days
    a. git post-recive hook
```
The below code is the pose-receive hook
#!/bin/bash
while read oldrev newrev refname
do
    if [[ $refname == "refs/heads/<branch_name>" ]]; then
        echo "Received new commits. Running shell script..."
        //ubunuthome/script.sh
    fi
done
```
  B. second way: we run the shell script by CI/CD toll
  * code push to remote -repository
    * creat new jenkins job, --> job triggered method --> git webhooks
     * in the excute shell -> specify the shell script name 
     
    
 ## 2. How do you find a list of files that have changed in a particular commit?
 Ans: for this multiple ways
     * i used commanad git diff last_commit_id latest_commit_id
     
 ### Monitoring using scripts:

## 1. Monitor a log file, detect a pattern detection, send an email on detection
Ans: take the assumptio email sent setup alreay done
  We need write one shell, run the script forevry one minutes by using cron job

```
Script code
#!/bin/bash
while inotifywait -e modify /tmp/errors.log; do
    mail -s "Error detected in logfile" user@bala.com < /tmp/errors.log
done
```
## 2. Monitor process particular process on an instance, send an email on incase of state change like process got stopped, taking more CPU that threshold
```
#!/bin/bash
while true; do
    if tail -n 1 /tmp/process.log | grep -q "stopped"; then
        mail -s "Process process has stopped" user@bala.com
    elif tail -n 1 /tmp/process.log | awk '{if($9 > 90) exit 1}'; then
        mail -s "Process process is taking more than 90% CPU" user@bala.com
    fi
    sleep 10
done
```

# Docker

1. Create a sample docker container with a Node.js Express app and demonstrate the installation.
* the below is the dockerfile
```
# Build stage
FROM node:14 AS build
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install --only=production
COPY . .
RUN npm run build

# Production stage
FROM node:14-alpine
ENV TZ=Asia/Kolkata
RUN apk --no-cache add tzdata
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN addgroup -S nodejs && adduser -S nodejs -G nodejs
WORKDIR /home/nodejs/app
COPY --from=build /usr/src/app/dist ./dist
USER nodejs
CMD ["pm2-runtime", "start", "dist/index.js"]
```
