# HTML-Phone
Call and Receive in HTML 

## Version 1
This is the first version of HTML Phone. It is a single page HTML file capable of making and recieving calls.

## Installation and Usage
1. Set the default SIP host, username, & password in `index.html`. 
2. Secure Web Sockets 8089 = wss://fubar:8089/ws : `8089` Required to be open on host/router.
3. SIP UDP Port 5060 = udp://fubar:5060 : Port `5060` Required to be open on host/router.
4. RTP Port Range `10000 - 20000` are required to be open on host/router. Configure RTP in Asterisk.
5. Open `index.html`. Wait until it says `Registered`, then type a number in the field or press a button.
6. HTML Navigator object will ask the browser to utilize the microphone / camera and the call will commence.

###Enable WebRTC in Asterisk FreePBX
1. Creating or edit an extension.
2. Select the 'Advanced' tab and set the extension settings to the following:
````
    Type = sip / chan_sip
    Send RPID = Send Remote-Party-ID header
    Connection Type = Peer
    NAT Mode = No
    Port = 5060
    Qualify = Yes
    Transport = All - WSS Primary
    Enable AVPF = Yes
    Force AVP = Yes
    Enable ICE Support = Yes
    Enable rtcp Mux = Yes
    Enable Encryption = Yes (SRTP only)
    Video Support = Yes
    Allowed Codecs = 'ulaw&g729'
````

You may have to create a SSL certificate inside Asterisk FreePBX to enable Secure Websockets [WSS]
1. Go to Admin > Certificate Management
2. Click the New Certificate button. Click Generate Let's Encrpypt Certificate
3. Enter your subdomain/FQDN (fully qualified domain name) to create a certificate. 
4. Use your Apache/httpd config/.htaccess files to 'Proxy-Pass' your subdomain/FQDN to Asterisk's IP (10.0.10.120) for DNS verification.

SIP Settings [chan_sip]
NAT = never
IP Config = Static
Override External IP = 10.0.10.120

Allow Anonymous Inbound SIP Calls = No
Allow SIP Guests = No
Default TLS Port Assignment = Chan SIP

External Address = 10.0.10.120
Local Networks = 10.0.10.0/24, 10.0.101.0/24

RTP Port Ranges
  Start = 10000
  End = 20000
Strict RTP = No

Audio Codecs
ulaw
g726
alaw
opus

Enable TLS = Yes
Certificate Manager = sip.janglehost.com
SSL Method = tlsv1
Dont' Verify Server = Yes

Non Standard g726 = No
Reinvite Behaviour = yes

Default Context = <none>
Bind Port = 5060
TLS Bind Port = 5061
Enable SRV Lookup = Yes
Enable TCP = Yes
Call Events = Yes
