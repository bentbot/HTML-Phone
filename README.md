# HTML-Phone
Call and Receive in HTML 

## Version 1
This is the first version of HTML Phone. It is a single page HTML file capable of making and recieving calls.

## Installation and Usage
1. Set the default SIP host, username, & password in `index.html`. 
2. Enable the secure web socket protocoll:
    - Socket URI `wss://YOURHOSTNAME:8089/ws` set in `index.html`
    - Port number `8089` is required to be open on host/router.
3. Add your SIP Hostname / Subdomain / FQDN
    - SIP URI `udp://YOURHOSTNAME:5060` set in `index.html`
    - Port number `5060` is required to be open on host/router.
4. Create the RTP Port Range
    - `10000 - 20000` are required to be open on host/router. 
    - Configure RTP in Asterisk in Step 8.
5. Log In To FreePBX / Asterisk. Create or edit an extension.
6. Select the 'Advanced' tab and set the extension settings to the following:
    - Type = sip / chan_sip
    - Send RPID = Send Remote-Party-ID header
    - Connection Type = Peer
    - NAT Mode = No
    - Port = 5060
    - Qualify = Yes
    - Transport = All - WSS Primary
    - Enable AVPF = Yes
    - Force AVP = Yes
    - Enable ICE Support = Yes
    - Enable rtcp Mux = Yes
    - Enable Encryption = Yes (SRTP only)
    - Video Support = Yes
    - Allowed Codecs = 'ulaw&g729'

3. You have to create a SSL certificate inside Asterisk FreePBX to enable Secure Websockets [WSS]
    - Go to Admin > Certificate Management
    - Click the New Certificate button. 
    - Click Generate Let's Encrpypt Certificate
    - Enter your subdomain/FQDN (fully qualified domain name) to create a certificate. 
    - Use your Apache/httpd config/.htaccess files to __ProxyPass__ a sub-domain or FQDN to your Asterisk's IP if you are having DNS verification issues with Let's Encrypt.
8. Finally, configure the SIP settings in Asterisk to allow for us to use connect to it with a browser.
    - Type = chan_sip
    - NAT = never
    - IP Config = Static
    - Override External IP = *10.0.10.120*
    - Allow Anonymous Inbound SIP Calls = No
    - Allow SIP Guests = No
    - Default TLS Port Assignment = Chan SIP
    - External Address = *10.0.10.120*
    - Local Networks = *10.0.10.0/24, 10.0.101.0/24*
    - __RTP Port Ranges__
        - Start = __10000__
        - End = __20000__
    - Strict RTP = No
    - Audio Codecs = __ulaw, g726, alaw, opus__
    - Enable TLS = Yes
    - Certificate Manager = *sip.janglehost.com*
    - SSL Method = tlsv1
    - Dont' Verify Server = Yes
    - Non Standard g726 = No
    - Reinvite Behaviour = yes
    - Default Context = <none>
    - Bind Port = 5060
    - TLS Bind Port = 5061
    - Enable SRV Lookup = Yes
    - Enable TCP = Yes
    - Call Events = Yes
9. Add all ports, certificates, and settings to the system. Open the the HTML file in an editor and update the first three variables in the <script> tag: 
    - **host**='YOUR_FQDN' 
    - **username**='LINE#' 
    - **password**='SIP_USER_PASSWORD'
10. Done! Now you can turn your browser into a IP phone just like the old Nortel Meridian~
