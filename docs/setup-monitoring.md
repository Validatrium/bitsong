# BITSONG MONITORING

Links: 
- [Monit Docs](https://mmonit.com/monit/documentation/monit.html). Tool we used to monitor server 


## Notifications you will recieve with this config
-   free disk space is less than 20% and 10% - _(low disk space notification)_
-   height is not changing - _(network connection error notification)_
-   node proccess is not running - _(node has been turned off)_
-   node hasn't peers - _(no peers - no sync. It's important)_
-   validator isn't in active set - _(to know if you're in jail)_
-   RPC server is not running - _(important to monitor parameters listed above)_
-   API service is not running - _(For future updates, by default it's not working. But you can turn it on)_

## Required:
1. **root** account on server
2. telegram bot token .You can follow [this instruction](https://marketplace.creatio.com/sites/marketplace/files/app-guide/Instructions._Telegram_bot_1.pdf)
3. Reciever chat_id. Follow [this guide](https://www.wikihow.com/Know-Chat-ID-on-Telegram-on-Android#:~:text=Locate%20%22Chat.%22%20It's%20about,Last%20Name%2C%20and%20your%20Username.&text=Note%20the%20number%20next%20to,is%20your%20personal%20Chat%20ID) to get your telegram 

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

### Tutorial created by Validatrium (more info on our projects at [validatrium.com](http://validatrium.com))

If you have any additional questions regarding this tutorial, please join [BitSong Official Discord Channell](https://discord.gg/AErfstSw) and tag Validatrium members.
