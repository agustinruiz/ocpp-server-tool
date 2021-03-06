# OCPP Central System Tool

This tool was used to simulate a OCPP 1.5 Central System server. IT DOES NOT SIMULATE A CHARGING STATION, so you need to use a real one (or use a software to simulate it as well).

To use it, you need to run ```mvn package``` and then execute the jar using ```java -jar PATHTOJAR.jar``` (the required libs are moved to a /lib folder and need to be in the same JAR folder). 

You need to create a settings.properties at the same folder where the JAR is, with the desired Charge Point endpoint URL. A settings.properties model is available at the src/main/resources folder.

## Supported Commands

Not all commands are supported, but the ones below are. You just need to type the command at the console + its arguments, as a "bash command". As you will see, all commands require the charge station id.

### remotestart
```
-cpid,--charge point id <arg>   charge point id
-c,--connector <arg>            connector id
-t,--tag <arg>                  tagid
```

### remotestop
```
-cpid,--charge point id <arg>   charge point id
-tid,--transaction <arg>        transaction id`
```

### changeavailability
```
-cpid,--charge point id <arg>   charge point id
-c,--connector <arg>            connector id
-t,--type <arg>                 availability type: operative|innoperative
```

### getconfiguration
```
-cpid,--charge point id <arg>   charge point id
-k,--key <arg>                  key
```

### changeconfiguration
```
-cpid,--charge point id <arg>   charge point id
-k,--key <arg>                  key
-v,--value <arg>                value
```

### unlockconnector
```
-c,--connector <arg>            connector id
-cpid,--charge point id <arg>   charge point id
```

### reset
```
-cpid,--charge point id <arg>   charge point id
-t,--type <arg>                 reset type: hard|soft
```

### datatransfer
```
-cpid,--chargepoint <arg>   charge point id
-d,--data <arg>             data
-msgid,--messageid <arg>    message id
-vid,--vendor <arg>         vendor id
```

### clearcache
```
-cpid,--chargepoint <arg>   charge point id
```

### getlocallist
```
-cpid,--chargepoint <arg>   charge point id
```

### cancelreservation
```
-cpid,--chargepoint <arg>   charge point id
-rid,--reservation <arg>    reservation id
```

### reservenow (right now is not working properly)
```
-c,--connector <arg>        connector id
-cpid,--chargepoint <arg>   charge point id
-e,--expiredate <arg>       expire date (in unix timestamp)
-pt,--parenttag <arg>       parent tag
-rid,--reservation <arg>    reservation id
-t,--tag <arg>              tagid
```

## Todo:
- Develop Tests
- Implement the missing request commands:
    - getDiagnostics
    - updateFirmware
    - sendLocalList
- Define a proper code style
- Allow users to choose which OCPP version they want to use
- Create a OCPP Charge Station simulator (who knows)
- Create an external settings file

## Contributing
Feel free to implement new features and push to this repo. I will review it as soon as possible.

But, if you want to contribute to the project implementing the missing commands, do as follows:
1. Create a class at ocpp.cp.commands which implements OcppCommand
    - It should receive a string with all the required parameter on the constructor
    - It should inject the ChargePointService on the contructor
    - It should filter the parameters using the Apache Common CLI
    - SUGGESTION: use the other classes as a base
2. Add it to the ChargePointCommandFactory (with the prefix create and without the suffix Command)
3. Add the command to the chargesystem.UserInputManager.execute()

AHH! Do not forget to use Guice to inject stuff... this project uses dependency injection and you should avoid using the new() operator.
