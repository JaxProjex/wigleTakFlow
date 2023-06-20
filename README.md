# wigleTakFlow
Node-Red Flow to forward cellular coverage or open WiFi APs from WiGLEs database to ATAK via multicast or TAKServer. (used in conjunction with Ampledatas node-red-contrib-tak).

![flow](/wigleTakFlow.png?raw=true "Node Red Flow")


REQUIREMENTS:

-Node-Red

-node-red-contrib-tak https://github.com/snstac/node-red-contrib-tak

-TAKServer (or Multicast supported devices)

-WiGLE account to obtain an API Token https://www.wigle.net

------------------------------

SETUP NODE-RED:

-https://nodered.org/docs/getting-started/

-options include installing node-red on your local PC, a Raspberry Pi or similar single board computer or mini PC, deploying node-red on a cloud server or virtual private server (VPS), Docker Container, or local virtual environment. After installing node-red you should be able to go to the node-red dashboard at http://*nodeRedIPaddress:1880 you may have to open appropriate ports (1880) to allow devices to access the node-red dashboard.

------------------------------

IMPORT .JSON FLOW TO NODE RED:

-in GitHub: click on "wigleTakFlow.json" > click on the download icon "Download raw file" > note where the "wigleTakFlow.json" file downloaded to, default is in your Downloads folder.

-in Node-Red: click on menu icon (3 horizontal lines top right) > click on "Import" > click on "select a file to import" > go to Downloads folder and click on "wigleTakFlow.json" > Upload > Import

ALTERNATIVELY..

-you can just copy the whole "wigleTakFlow.json" code from GitHub and paste it into the Node-Red Import Clipboard.

------------------------------

CONFIGURE TAKSERVER:

-TAKServer can be hosted on various platforms: Raspberry Pi, Mini PCs, your local PC, Cloud Servers or Virtual Private Servers (VPS), Docker Containers, or Virtual Environments. Regardless, if you're looking for instructions on setting up your TAKServer I recommend ATAKHQs guides https://www.atakhq.com/en/tak-server.

-Configuring the TCP (TAKServer) node in Node-Red wigleTakFlow, Input your TAKServer IP and Port, and checkbox whether you're using SSL/TLS or not. If you are using SSL/TLS, ensure you upload you TAKServer certificates and key that are located in the directory of your TAKServer that stores all client certificates/keys (ie: ubuntu docker container is "/opt/tak/certs/files"). I Suggest creating a TAK client "node-red" to be used specifically for Node-Reds handling of forwarding data to TAK. In your certificates file you will want to copy and upload "node-red.pem" as the "Certificate", "node-red.key" as the "Private Key" and "ca.pem" as the "CA Certificate" in the TLS Configuration Properties in the TCP (TAKServer) node in Node-Red. You will also need to enter the Passphrase for your TAKServer truststore, this can be found in your TAKServers CoreConfig.xml file. Default is "atakatak".

------------------------------

OBTAINING WIGLE API KEY:

-register for a WiGLE account at https://wigle.net

-navigate to WiGLEs API page for your account at https://wigle.net/account > Login > Show Token

-Encoded for use: "Basic " followed by this token will be copy pasted in the Authorization Header in Node-Red for both http req nodes in wigleTakFlow. The Authorization Header should look like "Basic YOURTOKEN" (quotation marks included).

-API Name: this token will be copy pasted to the "Username" under "basic authentication" in Node-Red for both http req nodes in wigleTakFlow.

-API Token: this token will be copy pasted to the "Password" under "basic authentication" in Node-Red for both http req nodes in wigleTakFlow.

------------------------------

USING THE WIGLETAKFLOW:

-once your TCP TAKServer node is connected and your WiGLE API tokens are entered (correctly), you're ready to start having your TAK devices get cell and wifi data.

-in ATAK: Menu (3 horizontal lines) > Drawing Tools > *choose square > *tap your 3 points to create your box > *select center CoT marker of your box (default name is Rectangle 1) > *click bottom right waypoint/menu icon in radial menu > change shape name to: Cellular (to get cellular coverage) or change shape name to: Wifi (to get open WiFi APs) > Send > Broadcast > cellular or wifi data will return as CoT icons inside your square. In the remarks section of each CoT returned will be additional data on the cell tower or wifi AP.

-in iTAK: Draw > *select polygon (the not circle shape) > select > *create a square around your area of interest > *change name to: Cellular (to get cellular coverage) or change shape name to: Wifi (to get open WiFi APs) > Share > Share with Contacts > Share With All >cellular or wifi data will return as CoT icons inside your square. In the remarks section of each CoT returned will be additional data on the cell tower or wifi AP.

![itak](/itak1.png?raw=true "itak")
![itak](/itak2.png?raw=true "itak")

-------------------------------

HOW THE WIGLETAKFLOW WORKS:

-TCP (TAKServer) node sends CoT traffic to TAK node to turn into a readable JSON format.

-TAK node sends CoT as JSON to filter data node.

-filter data node constantly reads and waits til a CoT marker includes "cellular" or "wifi" as the callsign in which it grabs the plotted locations of the square shape as the maximum and minimum range to get the wifi or cellular data for.

-http req nodes send a http GET to request of specific data from WiGLEs API depending on the callsign name.

-wigle json node formats a readable CoT to be passed to TAK node to turn into a XML CoT that TAK can understand.

-TAK turns the json data to CoT and pushes it via TCP (TAKServer) or UDP depending on what is chosen.
