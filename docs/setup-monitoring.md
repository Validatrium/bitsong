# BITSONG MONITORING

## Contents
1. [Monit monitoring tool](#1-monit)
2. [Panic monitoring tool](#2-panic)

## 1. Monit

Links: 
- [Monit Docs](https://mmonit.com/monit/documentation/monit.html). Tool we used to monitor server 

### Notifications you will recieve with this config
-   free disk space is less than 20% and 10% - _(low disk space notification)_
-   height is not changing - _(network connection error notification)_
-   node proccess is not running - _(node has been turned off)_
-   node hasn't peers - _(no peers - no sync. It's important)_
-   validator isn't in active set - _(to know if you're in jail)_
-   RPC server is not running - _(important to monitor parameters listed above)_
-   API service is not running - _(For future updates, by default it's not working. But you can turn it on)_

### Required:
1. **root** account on server
2. telegram bot token .You can follow [this instruction](https://marketplace.creatio.com/sites/marketplace/files/app-guide/Instructions._Telegram_bot_1.pdf)
3. Reciever chat_id. Follow [this guide](https://www.wikihow.com/Know-Chat-ID-on-Telegram-on-Android#:~:text=Locate%20%22Chat.%22%20It's%20about,Last%20Name%2C%20and%20your%20Username.&text=Note%20the%20number%20next%20to,is%20your%20personal%20Chat%20ID) to get your telegram 

```bash
# prerequires
apt install monit jq -y

# clone our repository: 
cd /root
git clone https://github.com/Validatrium/valmonit.git

# set your telegram.conf
cd $HOME/valmonit
cp telegram.conf.example telegram.conf
# enter your "telegram-id" and "bot-token"
nano telegram.conf

# verify that you can get notifications: 
./bin/sendtelegram -m "hi there!" -c ./telegram.conf

# by default it's monitor rpc-port/26657 and api-port/1317
# if you run on other ports you have to change it in: 
nano sh/cosmos-rpc.sh


# configure monit: 
# add bitsong configs to monit conf: 
echo "
include /root/valmonit/conf/bitsong-mainnet/*
include /root/valmonit/conf/system
include /root/valmonit/conf/default-cosmos-monitoring
" >> /etc/monit/monitrc

# if you need a web interface for your monitoring tool:
# but it's not actually required
cat <<EOF >> /etc/monit/monitrc
set httpd port 2812 and     # run on port 2812
  use      address 0.0.0.0  # run on internet interface 
  allow    * 		  	    # allow everyone connetc
  allow    admin:monit 	    # user:password pair
EOF

# restart monitoring tool 
systemctl restart monit
```

#### Tutorial created by Validatrium (more info on our projects at [validatrium.com](http://validatrium.com))

If you have any additional questions regarding this tutorial, please join the [BitSong Discord](https://discord.gg/AErfstSw) and tag Validatrium members.

## 2. Panic

With [Panic](https://github.com/SimplyVC/panic "Panic") running on a separate server (preferably in a different location) you are able to monitor all your nodes on different blockchains like, Chainlink, Substrate-based blockchains and of course Cosmos-based blockchains. Different notification channels can be configured as well:

- Telegram
- Slack
- Email
- Opsgenie
- PagerDuty
- Twilio (phone-calls)

From [Telegram and Slack](https://github.com/SimplyVC/panic/blob/master/docs/DESIGN_AND_FEATURES.md#telegram-and-slack-commands "Telegram and Slack") you will also be able to send commands to the Panic server.

This monitoring tool can notify you on both node and system alerts. For the full list and configuration options of [blockchains](https://github.com/SimplyVC/panic/blob/master/docs/DESIGN_AND_FEATURES.md#high-level-design "blockchains"), [notification channels](https://github.com/SimplyVC/panic/blob/master/docs/DESIGN_AND_FEATURES.md#alerting-channels "notification channels"), [alert types](https://github.com/SimplyVC/panic/blob/master/docs/DESIGN_AND_FEATURES.md#alert-types "alert types"), [node alert](https://github.com/SimplyVC/panic/blob/master/docs/DESIGN_AND_FEATURES.md#list-of-alerts "node alert") and [system alerts](https://github.com/SimplyVC/panic/blob/master/docs/DESIGN_AND_FEATURES.md#system-alerts "system alerts") you can check out the [Panic design github page](https://github.com/SimplyVC/panic/blob/master/docs/DESIGN_AND_FEATURES.md "Panic design GitHub page").

### Installation Guide
The Panic GitHub has pretty good installation guide already so I will just link to their GitHub instead of copy pasting it here again. That way it will also automatically stay up-to-date: https://github.com/SimplyVC/panic

### Need Help?
Panic has been added by CommunityStaking Validator, but is not involved with Panic in any way. Full credits go to the SimplyVC team. If you run into issues or have questions feel free to join the [BitSong Discord](https://discord.gg/SDHtyJNNEN "BitSong Discord") and tag CommunityStaking or join the [Panic Telegram](https://t.me/SimplyVC "Panic Telegram") and ask for help.
