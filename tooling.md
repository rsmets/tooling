Must haves:
VsCode
SourceTree
Postman
brew
_________
Postgress db view:
DataGrip (if can license, otherwise DBeaver)

Mongo db viewer:
MongoDB Compass
_________
* This guy was on top of HN today. "cyber swiss army knife" seems pretty handy when the time(s) strikes. https://gchq.github.io/CyberChef/


* ShiftIt for osx windows management, however looks like it is not being maintained anymore. https://github.com/fikovnik/ShiftIt
Actually have found the Rectangle which does the same thing. https://rectangleapp.com/ (Now way preferred)


* Pretty cool improvement on navigating GitHub source via the browser UI: https://chrome.google.com/webstore/detail/octotree/bkhaagjahfmjljalopjnoealnfndnagc?hl=en-US


* This could be a useful API if we want to do anything geolocation related. Could easily grab the log,lag to the thousanths decimal point with our hub checkins programmatically. just saw this project as it made top 25 on HN this morning. https://github.com/Risk3sixty-Labs/geoapi


* A tool I use everyday, thought I'd share. Super lightweight git cli tool. Really useful for quick diffs and selecting which files to stage. https://github.com/golbin/git-commander


* Here's another one, a chrome tabs manager that tracks web site hopping paths https://chrome.google.com/webstore/detail/tabs-outliner/eggkanocgddhmamlbiijnphhppkpkmkl


* I have had my bash profile configured to write my bash history to files denoted by calendar dates for some time now. Makes for a super easy time reviewing what I did certain days. How to configure:

```
# Saving history to file
export PROMPT_COMMAND='if [ "$(id -u)" -ne 0 ]; then echo "$(date "+%Y-%m-%d.%H:%M:%S") $(pwd) $(history 1)" >> ~/.logs/bash-history-$(date "+%Y-%m-%d").log; fi'
export HISTSIZE=100000
export HISTTIMEFORMAT="%d/%m/%y %T  "

# Avoid duplicates
export HISTCONTROL=ignoredups:erasedups

#After each command, append to the history file and reread it
export PROMPT_COMMAND="${PROMPT_COMMAND:+$PROMPT_COMMAND$'\n'}history -a; history -c; history -r"
```

then I have an alias to bash function to search over my ~/.logs directory:
```
alias s='search'
search() {
   ls -rt ~/.logs/*.log | xargs grep -rnw "$1"
}
```

* Alfred with the paid "powerpack". A major improvement over the default osx finder. The main value of the paid version IMO is you get clipboard history. I wouldn't consider myself an alfred super user but the features I do use I find indispensable. https://www.alfredapp.com/


* [not useful unless using parse server...] here is the script I have that handles killing (if running) and restarting parse-dashboard, redis-server and mongo
```
lsof -i:4040 -Fp | head -n 1 | sed 's/^p//' | xargs kill
lsof -i:6379 -Fp | head -n 1 | sed 's/^p//' | xargs kill
lsof -i:27017 -Fp | head -n 1 | sed 's/^p//' | xargs kill
$(parse-dashboard --config /Users/raysmets/dev/nk-dashboard/parse-dashboard-config.json)&
redis-server&
$(mongod --dbpath ~/data/db)&
```

* here is another script I use to ensure that my ssh keys are added to my ssh profile. seems like the ssh profile has a ttl for them? not sure but sometimes i get a failed auth while sshing and 9/10 times it's because the profile does not have the key it needs in it.
```
ssh-add -K ~/.ssh/nexkey/aws-eb.pem
ssh-add -K ~/.ssh/nexkey/aws-prod.pem
ssh-add -K ~/.ssh/nexkey/aws-staging.pem
ssh-add -K ~/.ssh/nexkey/scale-grid-aws-prod.pem
ssh-add -K ~/.ssh/nexkey/scale-grid-aws.pem
```

* A nugget from the startup script.... killByPort bash function
```
alias kbp='killByPort'
killByPort() {
  lsof -i:$1 -Fp | head -n 1 | sed 's/^p//' | xargs kill
}
```

* Here are actually some other bash functions that find useful:
```
alias dsr='dockerStopRemove'
dockerStopRemove() {
   docker stop $1 && docker rm $1
}
alias s='search'
search() {
   ls -rt ~/.logs/*.log | xargs grep -rnw "$1"
}
alias fbp='findByPort'
findByPort() {
   lsof -i tcp:$1
}
alias kbp='killByPort'
killByPort() {
  lsof -i:$1 -Fp | head -n 1 | sed 's/^p//' | xargs kill
}
alias curlt='curlWithTime'
curlWithTime() {
   curl -w "@/Users/raysmets/.curl-format.txt" -o /dev/null -s "$1"
}
```

* Step 4 of this guide explains how to auto format using eslint style guide upon every file save.
https://www.digitalocean.com/community/tutorials/linting-and-formatting-with-eslint-in-vs-code
