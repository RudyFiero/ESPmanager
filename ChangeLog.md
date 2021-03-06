V2.0.0-rc1

# Breaking changes
- Old settings files are no longer compatible, on first boot it will restore connection to existing wifi, you must click SAVE button in top right to save settings.
- Had two methods for returning host, removed one.  const char * is no longer suitable as the buffer goes out of scope. So it now returns String. `String getHostname();`
- autochecking of SPIFFS files have been removed.  You must now upload the data folder from example/data folder. 

# New Features
- Completely changed the underlying memory and data structure.  Moved to dynamic model, where data struct is populated when needed and held in memory for 1 min, or when modified, to be saved by user action in the webgui.  This is a trial that 'should' save RAM, as most of the data, hostname, update urls, IPs etc are  not needed after start and can be retrieved from SPIFFS when required.  Enabling and disabling STA and AP are done via passing structs around by reference, if you're interested... minimal sketch RAM after setup is 36768 (using 2.3.0).
- Settings are no longer saved by default.  When changed a save button appears, have to save changes for them to persist!
- Completely redesigned update mechanism with live progress for connected users using webEvents. Update the ESP with one URL, including SPIFFS files.  All files have MD5 checked, and only updated if changed.  Files updated/changed logged to 'Console' in webgui. Use buildmanifest.py to generate a suitable manifest file.  
`python buildmanifest.py pathtodata pathtooutputmanifest`.  Copy binary to this folder, rename the file firmware.bin to include it in update.
- Update frequency in min can be specified in webGui.
- Credentials entered in sketch at initialization override all other settings, this can be disabled with Use Sketch Credentials (on/off).  This prevents Host, SSID and PSK being used from sketch.    
- Moved to [asyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer) :)
- By default when no STA connection, the AP is enabled allowing connection and configuration. It also disables the STA.  I've found that whilst searching for the STA, the ESP is performing a WiFi scan every second, and this makes connecting reliably to the AP very difficult.  Thus if there is no STA on boot, it enables AP.  If STA connection is established on boot, but then lost the ESP will continue to try to reconnect (unless reconnect is set to false, and autoReconnect).
- Setting the AP SSID has been removed, it is now the deviceID.
- Web Events give connected web users info as to the state of updates, and changes, giving confirmation.
- handle() only works every 500ms, so very little burden to main sketch.
- Lots of changes to web interface. Including appcache support, so it is very fast loading. can be disabled with `#DISABLE_MANIFEST`
- Wrote my own mini string class, for the fun of it; `myString`.   
- Moved many strings to static variables so they are reused as much as possible.
- DHCP now handled natively via setting IPAddress(0,0,0,0).
- DNS are settable. DNS2 is optional.
- UMM information now presented in about page.
- ToolTip helpers on items on front page.
- OTApassword now working.  Password stored in plaintext for now... will change when next stable is released.
- Enable disable OTAupload.

# Bugfixes
- Many!
- Lots of bug fixes to AP logic, and tested.
- WiFi connection logic much improved.  Uses waitforconnection() result.  Super fast, much more reliable.

# ToDo

- Sort out buildmanifest directory structure... 

- Create alert with link for successful STA connection.
- Add Authentication

- OTA Page
  - Add enable/disable OTA @ boot

- Add file browser + uploader + file options... rename etc...
