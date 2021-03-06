# python-tuya

Rewritten version
-----------------
https://github.com/TradeFace/tuya

At the url above a fully rewitten version. Or find it at Pypi https://pypi.org/project/tuyaface/

__This fork no longer maintained.__

Modified version 
---------------
- added support for devices that don't respond to 0a status request command
- updated connection persistence
- keep reading till footer message
- ~used by https://github.com/TradeFace/tuyamqtt~

---------------
Python 2.7 and Python 3.6.1 interface to ESP8266MOD WiFi smart devices from Shenzhen Xenon.
If you are using the Jinvoo Smart App, this allows local control over the LAN.
NOTE requires the devices to have already been **activated** by Jinvoo Smart App (or similar).

## Key extraction

https://github.com/clach04/python-tuya/wiki has background information for how to get device id and local key.
(the device id can be seen in Jinvoo Smart App, under "Device Info").

Known to work with:
  * SKYROKU SM-PW701U Wi-Fi Plug Smart Plug - see https://wikidevi.com/wiki/Xenon_SM-PW701U
  * Wuudi SM-S0301-US - WIFI Smart Power Socket Multi Plug with 4 AC Outlets and 4 USB Charging


Demo:

    import pytuya

    d = pytuya.OutletDevice('DEVICE_ID_HERE', 'IP_ADDRESS_HERE', 'LOCAL_KEY_HERE')
    data = d.status()  # NOTE this does NOT require a valid key
    print('Dictionary %r' % data)
    print('state (bool, true is ON) %r' % data['dps']['1'])  # Show status of first controlled switch on device

    # Toggle switch state
    switch_state = data['dps']['1']
    data = d.set_status(not switch_state)  # This requires a valid key
    if data:
        print('set_status() result %r' % data)

    # on a switch that has 4 controllable ports, turn the fourth OFF (1 is the first)
    data = d.set_status(False, 4)
    if data:
        print('set_status() result %r' % data)
        print('set_status() extrat %r' % data[20:-8])

TODO demo timer (with comment not all devices support this, one way to check, is to check Jinvoo Smart App and see if there is a clock icon that is not dimmed out).

### Encryption notes

These devices uses AES encryption, this is not available in Python standard library, there are three options:

 1) PyCrypto
 2) PyCryptodome
 3) pyaes (note Python 2.x support requires https://github.com/ricmoo/pyaes/pull/13)

### Related Projects

  * https://github.com/sean6541/tuyaapi Python API to the web api
  * https://github.com/codetheweb/tuyapi node.js
  * https://github.com/Marcus-L/m4rcus.TuyaCore - .NET
  * https://github.com/SDNick484/rectec_status/ - RecTec pellet smokers control (with Alexa skill)

### Acknowledgements

  * Major breakthroughs on protocol work came from https://github.com/codetheweb/tuyapi from the reverse engineering time and skills of codetheweb and blackrozes, additional protocol reverse engineering from jepsonrob and clach04.
  * nijave pycryptodome support and testing
  * Exilit for unittests and docstrings
  * mike-gracia for improved Python version support
  * samuscherer for RGB Bulb support
  * magneticflux- for improved Python version support
  * sean6541 - for initial PyPi package and Home Assistant support <https://github.com/sean6541/tuya-homeassistant>
