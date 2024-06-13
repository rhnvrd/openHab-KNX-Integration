# OpenHab KNX integration Step-by-step guide



## First thing first

- config your devices on KNX bus (maybe with ETS6) [learn ETS6 Ecampus](https://my.knx.org/)

- needed module : a simple KNX net contain 3 device 
  
  1. actuator
  
  2. switch
  
  3. Gateway (for communicate with OH & Program KNX devices)
     
        *this is a KNX IP interface or router*

- read OH documentaition : [KNX - Bindings | openHAB](https://www.openhab.org/addons/bindings/knx/)

- make sure that OH host connected to Gateway (maybe ping it with ip address on host)



## OpenHab KNX Binding initialize

1. add [knx binding]([KNX - Bindings | openHAB](https://www.openhab.org/addons/bindings/knx/)) addon to your OpenHab panel 

    `Settings -> Bindings -> (+) (add) KNX Binding`

            *for connection to KNX bus you need 2 [Things](https://www.openhab.org/docs/concepts/things.html) in your OH*

2. KNX/IP Gateway (your gateway)

3. KNX Device (virtual definition of a KNX device on KNX bus)




## Config KNX/IP Gateway

this is a KNX IP interface or router

for adding new gateway in your OH [Things](https://www.openhab.org/docs/concepts/things.html) with administrator permission goto :

`Settings -> Things -> (+) (add) KNX Binding -> KNX/IP Gateway`

##### Configs :

*this is an example*

```yaml
UID: knx:ip:ed1a7ff380
label: myKNXGatewayTest
thingTypeUID: knx:ip
configuration:
  useNAT: true
  readRetriesLimit: 3
  ipAddress: 192.168.1.181
  autoReconnectPeriod: 60
  type: TUNNEL
  localSourceAddr: 0.0.0
  readingPause: 50
  portNumber: 3671
  responseTimeout: 10
location: secondFloor
```

- *Local Device Address (localSourseAddr) : 0.0.0 let OH decide what address to use - this is you gateway Address on KNX bus*

- *type : depend on your KNX/IP gateway specs*

- *ipAddress : your gateway ipAddress on LAN Network*



## Config KNX device

this is an addressable basic KNX device

for adding new device in your OH [Things](https://www.openhab.org/docs/concepts/things.html) with administrator permission goto :

`Settings -> Things -> (+) (add) KNX Binding -> KNX Device`

##### Configs :

*this is an example*

```yaml
UID: knx:device:a2300e6ac9
label: myKNXDeviceTest
thingTypeUID: knx:device
configuration:
  pingInterval: 600
  address: 1.1.100
  readInterval: 0
  fetch: false
bridgeUID: knx:ip:ed1a7ff380
location: secondFloor
```

- *bridgeUID (Parent Bridge) : this will represent your KNX/IP Gateway that you create in previous part*

- *address: your KNX/IP Gateway address on KNX bus*



## Adding [Channels](https://www.openhab.org/docs/configuration/things.html) and link it to [Items](https://www.openhab.org/docs/concepts/items.html)

Channels could be a represent of a Group address in KNX bus

for adding Channels to a KNX device on OH with administrator permission goto :

`Settings -> Things -> myKNXDeviceTest -> Channels -> Add Channel`

you need to specify

- Channel Identifier : a manual code

- Label : blablabla

- Channel Type : According to your KNX device specs and configs on KNX bus

- Configuration : may add groupAddressed and other configs (E.g., 0/2/1)

example for a simple Switch :

*this code is from knx device -> code*

```yaml
channels:
  - id: "Channel Identifier"
    channelTypeUID: knx:switch
    label: "Channel Identifier"
    description: "blablabla"
    configuration:
      ga: 0/0/1
  - id: "Channel Identifier2"
    channelTypeUID: knx:switch
    label: "bla"
    description: "bla"
    configuration:
      ga: 0/0/2
```

##### link your channel to an item :

`Settings -> Things -> myKNXDeviceTest -> Channels -> (Channel Identifier) -> Add Link to Item...`


