# Digital Optical Monitoring via SNMP and CLI
* In this wiki I will briefly describe how to get various readings for Optical modules installed in Junos / Junos EVO box via SNMP and CLI..
## References

* [Juniper Root OID](https://oidref.com/1.3.6.1.4.1.2636)
* [Juniper Root OID for MIB Objects](https://oidref.com/1.3.6.1.4.1.2636.3)
* [Juniper Root OID for DOM](https://oidref.com/1.3.6.1.4.1.2636.3.60)

## Execution
* Getting interface SNMP index.
```
show interfaces et-0/2/10 | grep ifindex < - - ZR+ optics             
  Interface index: 1053, SNMP ifIndex: 593
```
* snmp mib walk via Junos cli
```
show snmp mib walk 1.3.6.1.4.1.2636.3.60 | grep 593
jnxDomCurrentAlarms.593 = 00 00 00
jnxDomCurrentAlarmDate.593 = 07 e7 01 0a 09 1a 33 00 2d 08 00
jnxDomLastAlarms.593 = 00 80 00
jnxDomCurrentWarnings.593 = 00 00 00
jnxDomCurrentRxLaserPower.593 = -1974
jnxDomCurrentTxLaserBiasCurrent.593 = 0
jnxDomCurrentTxLaserOutputPower.593 = -1031
jnxDomCurrentModuleTemperature.593 = 51
jnxDomCurrentRxLaserPowerHighAlarmThreshold.593 = 199
jnxDomCurrentRxLaserPowerLowAlarmThreshold.593 = -2823
jnxDomCurrentRxLaserPowerHighWarningThreshold.593 = 0
jnxDomCurrentRxLaserPowerLowWarningThreshold.593 = -2301
jnxDomCurrentTxLaserBiasCurrentHighAlarmThreshold.593 = 0
jnxDomCurrentTxLaserBiasCurrentLowAlarmThreshold.593 = 0
jnxDomCurrentTxLaserBiasCurrentHighWarningThreshold.593 = 0
jnxDomCurrentTxLaserBiasCurrentLowWarningThreshold.593 = 0
jnxDomCurrentTxLaserOutputPowerHighAlarmThreshold.593 = 0
jnxDomCurrentTxLaserOutputPowerLowAlarmThreshold.593 = -1801
jnxDomCurrentTxLaserOutputPowerHighWarningThreshold.593 = -200
jnxDomCurrentTxLaserOutputPowerLowWarningThreshold.593 = -1600
jnxDomCurrentModuleTemperatureHighAlarmThreshold.593 = 76
jnxDomCurrentModuleTemperatureLowAlarmThreshold.593 = -6
jnxDomCurrentModuleTemperatureHighWarningThreshold.593 = 73
jnxDomCurrentModuleTemperatureLowWarningThreshold.593 = -3
jnxDomCurrentModuleVoltage.593 = 3224
jnxDomCurrentModuleVoltageHighAlarmThreshold.593 = 3700
jnxDomCurrentModuleVoltageLowAlarmThreshold.593 = 3000
jnxDomCurrentModuleVoltageHighWarningThreshold.593 = 3599
jnxDomCurrentModuleVoltageLowWarningThreshold.593 = 3049
jnxDomCurrentModuleLaneCount.593 = 1
jnxDomLaneIndex.593.0 = 0
jnxDomCurrentLaneAlarms.593.0 = 00 00 00
jnxDomCurrentLaneAlarmDate.593.0 = 07 e7 01 0a 09 1a 33 00 2d 08 00
jnxDomLaneLastAlarms.593.0 = 04 00 00
jnxDomCurrentLaneWarnings.593.0 = 00 00 00
jnxDomCurrentLaneRxLaserPower.593.0 = -1974
jnxDomCurrentLaneTxLaserBiasCurrent.593.0 = 262200
jnxDomCurrentLaneTxLaserOutputPower.593.0 = -1031
jnxDomCurrentLaneLaserTemperature.593.0 = 51
```
* Getting OID for specific object (e.g jnxDomCurrentModuleTemperature).
```
 show snmp mib walk jnxDomCurrentModuleTemperature | display xml | grep 593
            <name>jnxDomCurrentModuleTemperature.593</name>
                <index-value>593</index-value>
            <oid>1.3.6.1.4.1.2636.3.60.1.1.1.1.8.593</oid>
```
* SNMP Walk through from mgmt client (Ubuntu 20.04 in my case).
* I will use public snmp community which is already configured with read-only authorization on Junos EVO box.
```
sudo apt-get install snmp -y
snmpwalk -v2c -c public 192.168.10.10 1.3.6.1.4.1.2636.3.60.1.1.1.1.8.593
 iso.3.6.1.4.1.2636.3.60.1.1.1.1.8.593 = INTEGER: 52
```
* CLI Commands

```
show interfaces transport pm optics all et-0/2/10
show interfaces diagnostics optics et-0/2/10
show interfaces extensive et-0/2/10
start shell pfe network fpc0
show picd optics fpc_slot 0 pic_slot 2 port 10 cmd info
``
