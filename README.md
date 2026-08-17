# SignalRGB WIZ Network Plugin
This plugin adds a new network Interface to your SignalRGB Network Tab.
You need to activate the feature for local network communication inside your WIZ App

* It will start automatically to search for devices inside your home network.
* All found devices will automatically be added to your Device list as separate components. 
* The plugin will check every 60 seconds for new devices inside your network. 

#### This Plugin is still work-in-progress and I will try to continuously update the plugin with new devices and cover more features of WIZ lamps. 

## Stability fixes applied in 1.0.1
* `updateWithValue()` read `ip`/`port` out of the parsed WIZ JSON payload, where
  those fields do not exist. Every duplicate registration reply or `getPilot`
  heartbeat set the controller endpoint to `undefined`, which then reached
  `udp.send()`. Now read from the discovery value object.
* `Discovered()` had no exception guarding. A malformed datagram, a
  `{"error":...}` registration reply, or a `getSystemConfig` result without
  `moduleName` each threw straight back into the native UDP callback. All three
  paths are now validated and wrapped.
* `Render()` sent one datagram per rendered frame, over 60/sec per bulb. Sends
  are coalesced to 10/sec with a trailing flush so the final colour still lands.
* `getSystemConfig` was requested exactly once per device. If that datagram was
  dropped the device never announced. Now retries up to 5 times at 3s intervals.
* `device.isTW` was always undefined, so tunable-white bulbs kept their unusable
  colour properties. Reads `controller.isTW` now.
* `data.homeid` never matched the firmware's `homeId`. Constructor used
  `roomid`/`groupid` while `setDeviceInfo` used `roomId`/`groupId`.
* `dimming` is clamped to the 10-100 range WIZ firmware actually accepts.
* `minBrightniss` used the invalid parameter type `hue`.
* `TurnOffOnShutdown` was referenced but never declared as a property.

## Things known for improvement needed
* Better names for device components
* Cleanup the mess of a code I wrote
* Add images for device components
* Add support for white color temperatures
* Add support for device groups
* Add settings for discovery behavior as:
    * Discovery interval
    * Update interval 
