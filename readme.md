
#dit is de code voor de syslog generator

#!/bin/bash
# Path to netcat
NC="/bin/nc"
# Where are we sending messages from / to?
#ORIG_IP="192.168.190.11"
SOURCES=("192.168.190.2" "192.168.190.3" "192.168.190.4" "192.168.190.5" "192.168.190.6" "192.168.190.7")
#Destination network
DEST_IP="192.168.0.189"
# List of messages.
MESSAGES=("Error Event" "Warning Event" "Info Event")
# How long to wait in between sending messages.
SLEEP_SECS=1
# How many message to send at a time.
COUNT=1

FACILITIES=("kernel" "user" "mail" "system" "security" "syslog" "lpd" "nntp" "uucp" "time" "ftpd" "ntpd" "logaudit")
LEVELS=("emergency" "alert" "critical" "error" "warning" "notice" "info" "debug")
PRIORITIES=(0 1 2 3 4 5 6 7)

while [ 1 ]
do
	for i in $(seq 1 $COUNT)
	do
		# Picks a random syslog message from the list.
		RANDOM_MESSAGE=${MESSAGES[$RANDOM % ${#MESSAGES[@]} ]}
		PRIORITY=${PRIORITIES[$RANDOM % ${#PRIORITIES[@]} ]}
		SOURCE=${SOURCES[$RANDOM % ${#SOURCES[@]} ]}
		FACILITY=${FACILITIES[$RANDOM % ${#FACILITIES[@]} ]}
		LEVEL=${LEVELS[$RANDOM % ${#LEVELS[@]} ]}

			$NC $DEST_IP -u 514 -w 1 <<< "<$PRIORITY>`env LANG=us_US.UTF-8 date "+%b %d %H:%M:%S"` $SOURCE [$FACILITY.$LEVEL] service: $RANDOM_MESSAGE"
			echo $NC $DEST_IP -u 514 -w 1  "<$PRIORITY>`env LANG=us_US.UTF-8 date "+%b %d %H:%M:%S"` $SOURCE service: $RANDOM_MESSAGE"

	done
	sleep $SLEEP_SECS
done

#nog een paar commands die ik heb gebruiktr
- sudo raspi-config #voor het instellen van de raspberry pi
- sudo apt update # deze en het volgende commando zijn er om te zorgen dat alles uptodate is
- sudo apt full-upgrade
- sudo apt install rsyslog #installeren van rsyslog
- sudo nano /etc/rsyslog.conf #dit en het volgende commando zijn voor de locatie van de syslog gegevens
- sudo nano /etc/rsyslog.d/sys.config
- sudo systemctl restart rsyslog # restarten van de server
- sudo cat /var/log/sys.log
- sudo status systemctl rsyslog #status van de server laten zien
- sudo nano syslogGen1.sh #maken van de generator
- chmod +x syslogGen1.sh #zorgen dat de file de juiste rechten heeft(in dit geval execute rechten)
- dan heb ik een command ingegeven waarmee ik met solarwind verbind
- daarna heb ik syslogGen1.sh laten runnen
- ik heb hier nog allemaal foto's van maar die kan ik hier niet laten zien

#dit zijn in grote lijnen de links waar ik mijn informatie heb gehaald en link naar solarwind voor het visualiseren van de syslog server
#nog wat links naar verschillende bronnen waar ik mijn inspiratie vandaan heb gehaald
- https://pimylifeup.com/raspberry-pi-syslog-server/
- https://www.youtube.com/watch?v=CRpQKcVeVoo
- https://www.youtube.com/watch?v=Cw-TXDirgcQ
- https://www.loggly.com/solution/linux-syslog/?CMP=KNC-TAD-GGL-SW_EMEA_X_PP_CPC_LD_EN_PROD_SW-LGL-12302661005~119270368684_g_c_-~497490656868~~9047645~~&gclid=Cj0KCQjwl7qSBhD-ARIsACvV1X3zz8-wTt-6w2X-ITMD9H1BlKzO2snkqOUphJJeDmhUfwSJ9VnAdokaAmh6EALw_wcB
- https://my.solarwinds.cloud/login?client_id=loggly&response_type=code&scope=openid+swicus&nonce=7kKMDBJZZ309JeDD&redirect_uri=https%3A%2F%2Fapp.loggly.com%2Fsso%2Foidc%2Fdo_sign_on&state=eyJyZnAiOiAibEI4dHlyNHpWb0FSOXFxeiIsICJyZXR1cm5fcGF0aCI6ICIvc2VhcmNoP3Rlcm1zPWxvZ3R5cGU6c3lzbG9nIiwgInN1YmRvbWFpbiI6ICJ0aG9tYXNtb3JlIiwgIm9yaWdpbmF0aW5nX29yZ19pZCI6IG51bGx9&org_id=141767306102525952#terms=syslog.severity%3AError%20OR%20syslog.severity%3ACritical&from=2022-04-06T09%3A48%3A19.593Z&until=2022-04-07T09%3A48%3A19.593Z&source_group=
- https://my.solarwinds.cloud/login?client_id=papertrail&redirect_uri=https%3A%2F%2Fpapertrailapp.com%2Faccount%2Fauth%2Fswicus%2Fcallback&response_type=code&scope=openid+swicus&state=SeYtCOCOTxPo1bcwJ%2BU8kd0sHAd9rt1dJhGnMpP8FMY%3D
- https://cybercademy.org/create-a-centralized-syslog-server-project-overview/

#video met korte uitleg
- https://youtu.be/056nkyqdR0I
