# greenade

Your Git squares are cold and white? Commit and Push makes green your Github, but it's boring if you want do it every day. let's automate it with bash and plant weed together.
purpose this article is work with:

- Systemd
- Bash
- Git

First, We need to a repo and second a modifed/new file to commit. we can use `fortune` for making random text.

Make a file and name it `commit.sh` 

```bash
#!/bin/bash
NUMBER=$(($RANDOM % 13))
# replace your path here
P=/home/xiii/greenade
for ((run=1; run <= NUMBER + 1; run++))
do
  fortune -a > $P/files/file.txt
  git -C $P add .
  git -C $P commit -m "`fortune -sn 32`"
done
```

- Another file for pushing our commit. File name is `push.sh`
```bash
#!/bin/bash
# git config --global --add safe.directory "*"
bash /home/xiii/greenade/commit.sh
git -C /home/xiii/greenade/ push origin main
```

Make it as a excutable file:

```bash
chmod +x push.sh
```

- For automate this job we can make a timer and serivce file in systemd (/usr/lib/systemd/system). both file must have same name. for example I used gitPush.timer and gitPush.service.

```bash
vim sudo /usr/lib/systemd/system/gitPush.timer
```

gitPush.timer

```bash
 [Unit]
Description=Git Push.

[Timer]
OnCalendar=*-*-* 22:34:00

[Install]
WantedBy=multi-user.target
```

And for gitPush.service we have:

```bash
vim sudo /usr/lib/systemd/system/gitPush.service
```

```bash
[Unit]
Description= Git Push

[Service]
Type=simple
ExecStart=runuser -l xiii -c /home/xiii/greenade/push.sh
```

__Note__:  we should run the bash with normal user. you can replace your user with “xiii”


 Now you can start the gitPush.timer

```bash
sudo systemctl start gitPush.timer
```

and check the status

```bash
> sudo systemctl status gitPush.timer
● gitPush.timer - Git Push.
     Loaded: loaded (/usr/lib/systemd/system/gitPush.timer; disabled; preset: disabled)
     Active: active (waiting) since Thu 2022-11-24 22:33:22 +0330; 29min ago
      Until: Thu 2022-11-24 22:33:22 +0330; 29min ago
    Trigger: Fri 2022-11-25 22:34:00 +0330; 23h left
   Triggers: ● gitPush.service

Nov 24 22:33:22 xiii systemd[1]: Started Git Push..
```

After restart your machine you need to run it again, if you want your timer automatically start after restart you can enable it.

```bash
sudo systemctl enable gitPush.timer
```

Are your Github squares not colored yet? so, you should check the Git config email. the email must be same with your primary github email.

I hope enjoy it and I don't recommanded paint your Github automatically. let's don't judging people by their GitHub activity stats. Don't worry if your Git squares cold and white, always be warm and green your heart and your smile.

Thanks for reading.

 
