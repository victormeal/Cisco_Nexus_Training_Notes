# OnPrem_Smart_Licensing_config

```
feature license smart

callhome
  email-contact sch-smart-licensing@cisco.com
  destination-profile CiscoTAC-1 transport-method http
  destination-profile CiscoTAC-1 index 1 http https://172.21.120.17/Transportgateway/services/DeviceRequestHandler
  transport http use-vrf management
  enable
```
you can also add
```
  license smart enable
```
from previous config you might only need to change the IP address 172.21.120.17
to register a device use:
```
license smart register idtoken <token> <force>
```
