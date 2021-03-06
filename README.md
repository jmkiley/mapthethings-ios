# MapTheThings-iOS

The iOS app portion of [MapTheThings](http://map.thethings.nyc), the
global coverage map for The Things Network (TTN).

## Using the App
- Launch to the Map view - Shows local map with latest sample data
- Switch to Sampling view because I'm ready to add my own data.
- Log in with email and selected service (Ideally any of Google, Twitter, FB, Github)
- No devices configured - when I tap the device picker, it switches me to the Devices page. Oh, right, I should have noticed that there is a badge on the devices tab button indicating there is a BLE node available near me.
- Pair with my node or add a LoRa-only node
- Back to Sampling view. I can hit Start and see samples coming in.
- Back to Map view and I see new samples appearing on the map in real time. Now that I'm actively sampling, Map shows a pause button and details of the most recent sample added.

## Client Responsibilities
- Map view - Show MapTheThings map of local area. This could be simple a WebView looking at map.thethings.nyc.
- Devices view - Set the ID of the device to listen to in MQTT Collection mode.
- Sampling view - Show current lat/lon in text and on map. Show time since last successful TTN packet transmission and time since most recent attempt (if known). Show RSSI and SNR of last successful packet. Select collection mode. Show appropriate status from server that data is being collected.
- Authenticate via OAuth2 with identity providers of the user's choice: Github, Google, Facebook, Twitter. Securely store authentication token if requested by user.
- Track location with CoreLocation.
- Subscribe to TTN to listen for successful message sends.
- Transmit collection data to MapTheThings server API.
- Store collection data locally for later sync to MapTheThings server API.

## Collection Modes
- Active Collect mode - App connects to node and drives transmission. App directs node to transmit current lat/lon. App records each transmission and periodically sends list of attempts to the API. Server then reconciles with packets it has received from TTN. In this mode we are certain when an attempt was made and not successfully sent through TTN, so we can report packet loss.

- Matching Collect mode - (the Chris Merck method) App posts periodic GPS samples which are then time-aligned with packets sent by passive node.
There is a node in the same location as the iOS device running the app. The node transmits packets periodically. The app collects periodic GPS samples. The app sends the GPS samples to the API. The server then aligns GPS samples with TTN packets to determine where the user was then the TTN packet was sent.

- Listener Collect mode - App subscribes to MQTT and posts on packet received.
There is a node in the same location as the iOS device running the app. The node transmits packets periodically. The app is subscribed to MQTT and when it receives a packet from the device, it posts the packet data and lat/lon to the API. This is how ttnmapper.org collects info.

## People
- Frank - Focused on working example of Active Collection. Bluetooth communication. Sync GPS samples with server.
- Forrest - Putting burgeoning Swift skills to work.
- ???

## License
Source code for Map The Things is released under the MIT License,
which can be found in the [LICENSE](LICENSE) file.
