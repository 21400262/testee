# 2020-1 Open Source Software Lab class #4 Project

## 21400262 Hyeonmyeong Noh

## Purpose
People who live alone apart from the University usually have TV in their room as a optional furniture of the rent house. However we don't usually use it because we have smartphone and use Youtube application to watch programs. Therefore it would be useful to use the TV as a screen of the magic mirror. Magic mirror is a device that functions as mirror and screen to show customized information. Originally to make Magic Mirror, a piece of glass and a two-way mirror film have to be attached to the TV screen. But in this project I just created the inner programs and would show the resultant screen. Especially, I tried to show the information of COVID19.

## Result Image
![Output](https://user-images.githubusercontent.com/64130293/84565197-8079e200-ada2-11ea-94d0-70708eac37c6.JPG)

## Installation and Usage
The basic software program can be downloaded in the [Magic Mirror](https://docs.magicmirror.builders/getting-started/installation.html#manual-installation).
Click the link and follow the Manual Installation procedure.

For the automatic start of the program, install pm2.
```
sudo npm install -g pm2
pm2 startup
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u pi --hp /home/pi
pm2 start ~/MagicMirror/installers/mm.sh
pm2 save
```

After saving the above setting, the Magic Mirror will be turned on automatically whenever you start raspberrypi.
- pm2 Commands
```
pm2 start mm      : start Magic Mirror
pm2 stop mm       : stop Magic Mirror
pm2 restart mm    : restart Magic Mirror
```

Magic Mirror has default modules includig Clock, Calender, Current Weather, Weather Forecast, News Feed, Compliments, Hello World, Alert. Additional modules can be downloaded from [3rd Party Modules](https://github.com/MichMich/MagicMirror/wiki/3rd-party-modules).
I uploaded a folder 'moduels' that I used in this project in this repository, but note that some modules are not available because I could not figure out how to use the modules. 'MMM-COVID19', 'clock', 'MMM-OnScreenMenu', 'MMM-EmbedYoutube' and 'newsfeed' can be displayed in this project.
[Clock](https://github.com/MichMich/MagicMirror/tree/master/modules/default/clock) and [newsfeed](https://github.com/MichMich/MagicMirror/tree/master/modules/default/newsfeed) are default modules.

To use other 3rd party modules, you should clone the git repository in the ~/MagicMirror/moduels folder, install and modify the configurations.
**1. [MMM-COVID19](https://github.com/bibaldo/MMM-COVID19)**
This module displays international COVID19 datas.
Enter the commands below in your terminal.
```
cd ~/MagicMirror/modules
git clone https://github.com/bibaldo/MMM-COVID19.git
cd MMM-COVID19
npm install
```
After installation, add the module in the modules array in the ```~/MagicMirror/config/config.js``` file:
```
cd ~/MagicMirror/config/config.js
nano config.js
```
config.js
```
modules: [
      {
          module: "MMM-COVID19",
          position: "bottom_right", ### There are 13 positions. Refer to [Module Configuration](https://docs.magicmirror.builders/modules/configuration.html#example).
          config: {
                updateInterval: 300000,
      		worldStats: true,
      		delta: true,
      		lastUpdateInfo: true,
                  countries: ["S. Korea", "USA", "China", "Japan"],
                  headerRowClass: "small",
      		rapidapiKey : "0ab64e7c61mshcb8650033f0e939p1c653ajsn95784b21f789" ### You can get your rapidapiKey in [Coronavirus monitor API Documentation](https://rapidapi.com/astsiatsko/api/coronavirus-monitor).
                   ... ### other configuration options are listed in [MMM-COVID19](https://github.com/bibaldo/MMM-COVID19).
          }
      },
]
```

**2. [MMM-EmbedYoutube](https://github.com/nitpum/MMM-EmbedYoutube)**
This module displays youtube streaming block.
Enter the commands below in your terminal.
```
cd ~/MagicMirror/modules
git https://github.com/nitpum/MMM-EmbedYoutube.git
cd MMM-EmbedYoutube
npm install
```
After installation, edit the config.js file.
```
{
      module: "MMM-EmbedYoutube", 
	position: "top_left",	
	config: {
		video_id: "NMre6IAAAiU", ### This is in the youtube URL 'https://www.youtube.com/watch?v=NMre6IAAAiU'
		loop: false
	}
},
```

**3. [MMM-OnScreenMenu](https://github.com/shbatm/MMM-OnScreenMenu)**
This module let us control the modules and Magic Mirror program.
Enter the commands below in your terminal.
```
cd ~/MagicMirror/modules
git https://github.com/shbatm/MMM-OnScreenMenu.git
cd MMM-OnScreenMenu
npm install
```
After installation, edit the config.js file.
```
{
	module: "MMM-OnScreenMenu",
	position: "bottom_right",
	/* Valid positions: 'top_right', 'top_left', 'bottom_right', 'bottom_left' */
	config: {
		touchMode: false,
		enableKeyboard: true,
		menuItems: {
			reboot: { title: "Reboot", icon: "spinner" },
			shutdown: { title: "Shutdown", icon: "power-off" },
			moduleHide1: { title: "Hide Youtube", icon: "minus-square", name: "MMM-EmbedYoutube" },
			moduleShow1: { title: "Show Youtube", icon: "plus-square", name: "MMM-EmbedYoutube" },
			moduleHide2: { title: "Hide json-feed", icon: "minus-square", name: "MMM-json-feed" },
			moduleShow2: { title: "Show json-feed", icon: "plus-square", name: "MMM-json-feed" },
		}
	}
},
```

**4. [newsfeed](https://github.com/MichMich/MagicMirror/tree/master/modules/default/newsfeed)**
This module displays real time news.
The newsfeed URL in config.js file comes from [Yeonhapnews](http://www.yonhapnewstv.co.kr/category/news/international/feed/).

To check the config for error, enter ```npm run config:check``` in command.

## Useful tool
[Download VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/) this program will help you to test the Magic Mirror program. This displays raspberrypi screen. in your desktop.

## Limitation
Actually, what I tried to do in this project is to create graphs and charts to show the real time COVID19 information from [LiveCoronaDetector](https://github.com/LiveCoronaDetector/livecod/tree/master/data). I found out several methods to create a module to display graphs in the Magic Mirror, but unfortunately I failed to understand the codes written in js files and other visualazation languages and to create the module.
