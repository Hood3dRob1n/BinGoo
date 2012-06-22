                                         __   _,--="=--,_   __
                                        /  \."    .-.    "./  \
				       /  ,/  _   : :   _  \/` \
                                       \  `| /o\  :_:  /o\ |\__/
                                        `-'| :="~` _ `~"=: |
                                           \`     (_)     `/
                                    .-"-.   \      |      /   .-"-.
                               .---{     }--|  /,.-'-.,\  |--{     }---.
                                )  (_)_)_)  \_/`~-===-~`\_/  (_(_(_)  (
                               (   ____  _        ____             _   )
                                ) | __ )(_)_ __  / ___| ___   ___ | | (
                               (  |  _ \| | '_ \| |  _ / _ \ / _ \| |  )
                                ) | |_) | | | | | |_| | (_) | (_) |_| (
                               (  |____/|_|_| |_|\____|\___/ \___/(_)  )
                                )                                     (
                               '-----v1--------------By-Hood3dRob1n----'

                                  Welcome to the BinGoo README file!    

This should give you a general overview of what the tool is all about and how to go about using it. This is a the result of a project of mine to come up with better solution for gathering mass links for research and I tweaked it to align with my hacking needs. I also decided I was going to do everything in bash to make things difficult on myself, well cause thats how I am :p

BinGoo is my version of an all-in-one dorking tool written in pure bash. It leverages Google AND Bing main search pages to scrape a large amount of links based on provided search terms. You can choose to search a single dork at a time or you can make lists with one dork per line and perform mass scans. Once your done with that, or maybe you have links gathered from other means, you can move to the Analyzing tools to test for common signs of vulnerabilities. The results are neatly sorted into their own respective files basedon findings. If you want to take further you can run them through the SQL or LFI tools which are some semi working homebrewed creations I made in bash or you can use the SQLMAP and FIMAP wrapper tools I wrote which work much better and with greater accuracy and results. I have also included a few neat features to make life easy, such as Geo dorking based on domain type or domain country codes or shared hosting checker which uses preconfigured Bing search and a dork list to find possible vulns on other sites on same server. I also included a simple admin page finder which simply works based on a provided list and server response codes for confirmation of existance. Together I think it all works as a nice little package!

----------
BinGoo 101:
--------------
Pre-requisites:
--------------
	- LYNX & CURL required for core functionality
		- LYNX/CURL Install: apt-get install lynx; apt-get install curl
	- NMAP, FIMAP & SQLMAP required for plugins and testing functionality (digger, fimap & sqlmap wrappers). Install with SVN from where you want to store them:
		- FIMAP install: svn checkout http://fimap.googlecode.com/svn/trunk/ fimap
			- cd fimap/; chmod +x fimap.py; ./fimap.py --help
		- SQLMAP install: svn checkout https://svn.sqlmap.org/sqlmap/trunk/sqlmap sqlmap
			- cd sqlmap/; chmod +x sqlmap.py; ./sqlmap.py --help
		- NMAP install: svn co https://svn.nmap.org/nmap nmap
			- cd nmap/; ./configure; make; sudo make install; nmap --help

NOTE: The FIMAP instance needs to be the SVN version as it contains features used which are not included in the standard version (or that which is included with BackTrack by default, --bmin, --bmax, -D + options....) so delete your old copy or just install it side by side, whatever floats your boat...

You can enter all path info in the BinGoo file on lines 14-21 in the config section to adjust as needed. 

------------------
FUNCTIONS OVERVIEW:
------------------------
1) Google & Bing Dorkers:
------------------------
Simply choose the search engine you want to use at the main menu and then follow the prompts. You can run a single dork based on your user input and get as custom as you like OR you can point it to a dork file. I have included a few with the default package which can be found in the dorks/ directory within BinGoo/ folder. You can point it anywhere you want, just ensure there is one dork per line so it doesnt mess things up. Once all info provided the tool will work its magic and generate links files for output containing all results. You will get a file called b-links.txt for Bing searches and g-links.txt for Google searches.

NOTE: The Google dorker doesnt bypass the Google restrictions anymore but if you scan and then analyze or scan with Google, then scan with Bing, then Analyze you will keep the blocks down to a minimum and shouldnt notice any issues. If you dont get results from Google it is likely due to temporary IP block, just use Bing until it refreshes and goes away (take note of first advice to avoid this situation).

------------------
2) Bing Geo Dorker:
------------------
This is my way of allowing users to define the domain type for their searches. I have set a pre-built list of dorks which i use for this option (dorks/site.lst) which you can modify if you like. It runs a list scan against the user provided SITE TYPE or COUNTRY CODE and generates the link results into a file which will include the Geo code or Site Type provided to make it easier to keep track of. 

Options for Bing Geo Dorker include:
AC AD AE AF AG AI AL AM AO AQ AR AS AT AU AW AX AZ BA BB BD BE BF BG BH BI BJ BM BN BO BR BS BT BW BY BZ
CA CC CD CF CG CH CI CK CL CM CN CO CR CU CV CX CY CZ DE DJ DK DM DO DZ EC EE EG ER ES ET EU FI FJ FK FM FO FR
GA GD GE GF GG GH GI GL GM GN GP GQ GR GS GT GU GW GY HK HM HN HR HT HU ID IE IL IM IN IO IQ IR IS IT JE JM JO JP  
KE KG KH KI KM KN KP KR KW KY KZ LA LB LC LI LK LR LS LT LU LV LY MA MC MD ME MG MH MK ML MM MN MO MP MQ MR MS MT MU MV MW MX MY MZ  
NA NC NE NF NG NI NL NO NP NR NU NZ OM PA PE PF PG PH PK PL PM PN PR PS PT PW PY QA RE RO RS RU RW
SA SB SC SD SE SG SH SI SK SL SM SN SO SR SS ST SV SY SZ TC TD TF TG TH TJ TK TL TM TN TO TR TT TV TW TZ
UA UG UK US UY UZ VA VC VE VG VI VN VU WF WS YE ZA ZM ZW 

BIZ COM INFO NET ORG AERO ASIA CAT COOP EDU GOV INT JOBS MIL MOBI MUSEUM TEL TRAVEL XXX

----------------------------
3) Bing Shared Hosting Check:
----------------------------
This module will run a list scan using dorks/sharedhosting.lst against user provided IP or Site name to see if there are any other sites on the same server which might have possible vulnerabilities. You can shrink or expand the dork file used as you like.

--------------------
4) Digger Recon Tool:
--------------------
This is another script I wrote separately and decided to incorporate into the mix as I find it useful all the time so figured others would too. It is in the plugins folder and is called by the main script via the menu options. It uses a few built in tools and a few online sites to gather information on a target site. It will run a check on Alexa Ranking, SameIP Shared hosting check, Bing shared hosting check, Sub-domain check, whois info, and a quick nmap scan. Together this can provide a wealth of information with barely taking direct aim (you can comment out the nmap scan if you like to tone it down a bit)

-----------------------
Analyze & Tools Section:
-----------------------
This option actually takes you to a new menu section where you can perform post dorking activities. Here is quick overview of the available options from this menu:

Analyze Bing Links File: 
this option will analyze ONLY the b-links.txt file which is generated by a Bing search from the main menu

Analyze Google Links File: 
this option will analyze ONLY the g-links.txt file which is generated by a Google search from the main menu

Analyze BOTH Google & Bing links files: 
this option actually combines the two files into a single file and then runs the analysis check on the new file. 

Analyze My Links File: 
This is an option which allows you to point the tool to your own links file. This can be any file you want as long as it has a single link per line

NOTE: above analyze options all run a injection and regex check against the pages to check for common signs of injection vulnerabilities and classifies accordingly. You can check source for regex if you want to add or remove anything from the lists used. The positive results are filted into the results/ folder and classified based on how or what was found. The positive regex is clearly labeled as LFI or SQL and in terminal even includes the line where it was found which is helpful in determine what is garbage and what is real. Possibles.results file is full of pages where the injected page returned a 85% loss of text AND had a reduction in table rows (a decent indication that the injection has caused changed also indicating a potential vuln), not very accurate but better to mark and review manually than to omit altogether. If you have a better method please share with me so I can improve this function :) Small issue when back to back vulns identified causing the next third site to be marked as vuln when it isn't, further validation tests will get rid of these but this is your heads up its not 100% perfect. 

------------
Admin Finder:
------------ 
This is my homebrewed admin finder. It uses a small word list (plugins/admin.lst) and judges server response and reports back accordingly. You can add to the list as much as you like. The source has been coded to optimize and emulate multi-threading to handle larger lists if/when needed. The default list works pretty good though. 

---------
LFI Tools:
---------
This takes you to another sub-menu where you can choose to run my homebrewed validator script or you can run my FIMAP wrapper to confirm LFI vulnerabilities. My tester doesnt work 100% and is a work in progress but included in case someone wants to help me get it going a little better just to show it can be done in bash :) 
	FIMAP wrapper works great! It allows you to choose if you want to run scan against a single site, 		the default found results/LFI.results file which is generated after a successful analysis run, or 		you can point it at your own link file to test. This means you can process one at a time or in 		bulk!

----------
SQLi Tools:
----------
Very similar to the LFI tools section. I have created a homebrew column counter for verbose vulnerabilities. It works ok, but still in the works. As a backup I have built a SQLMAP wrapper script to allow you to pass them off to be validated by SQLMAP for certainty. As with the LFI wrapper you can choose to run against a single site, the default results/SQLi.results file from analysis stage, or a custom file of your chosing with one link per line to test in bulk. 


------------
KNOWN ISSUES:
------------
curl --ssl option not recognized on default BackTrack curl installation. Simply remove this from the curl_magic() function within the main bingoo script (line 474 & 475) and you should be fine. 

If your not using the SVN version of FIMAP you need to remove the "--bmin=4 --bmax=9 -D --dot-trunc-also-unix" from all three (3) occurances within the LFI script located in the plugins/ directory (line 77, line 96, and line 109). Eventually this will be included in updated versions of FIMAP and therefore default installs but until then we need to work out of SVN for best results. Deal with it :p

Occassionally SQLMAP formatting gets junked in terminal. Results and process flow should flow as normal so dont worry about it. If you need to see the results clearer just open the sqlmap/output/<site> folder and view the log file to see the full details from SQLMAP scan. Let it run as if you dont get a clean exit it can leave some processes running in background. You can try to kill them all with some command along the lines of this: 
	ps aux | grep bingoo | cut -d' ' -f6 | while read line; do kill -9 $line; done;
If it doesnt work you will know as shit still flows to the terminal screen, but this should kill off all instances of bingoo if left running in background. You might also repeat for "curl" if some threads sneak by even after the first kill shot using "bingoo" :)


You can press CTRL+C at anypoint to abort an active session and most active processes will be killed and all in progress work dumped to crash.results file so you dont completely loose things. This project is a constant work in progress so changes and updates may occur as time goes on. Again if you have questions, suggestions, or improvements and contributions you can email me at hood3drob1n@gmail.com

Let it also be known that I in no way take any responsibility for anything you do with this tool. Use at your own risk. Be safe and until next time, enjoy!

Greetz,
H.R.

#EOF
