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


# Security:
 ## 1. Show how to block ports
  * ans: sudo iptables -D INPUT -p tcp --dport 22 -j DROP
## 2. Show how to setup port forwarding
 * ans: step1: we need install __reveser porxy__ example nginx
 *  edit /etc/nginx/nginx.conf or /etc/nginx/sites-available/default
* update the content like  below

```
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
# Network:
## 1. Show list of processes using the network.
* ans: sudo lsof -i

## 2. Show the list of IPs a process is connected to
* ans: netstat -tupln
## 3. Show how to list open files and kill processes tied to a user
* ans: sudo lsof -u <username>  (or)
  * sudo killall -u <username>

# Code:
## 1. For each multiple of 3, print "AVA" instead of the number.
```
#!/bin/bash
for i in range(1, 100):
    if i % 3 == 0:
        print("AVA")
    else:
        continue
```

## 2. For each multiple of 5, print "AMO" instead of the number.
```
#!/bin/bash
for i in range(1, 100):
    if i % 5 == 0:
        print("AVA")
    else:
        continue
```
