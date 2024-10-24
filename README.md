# DRV8328Shield

RevA errors:
- PVDD should be tied to VIN instead of 5V  
  5V is simply too low to keep the high side switched on continuously

- TPS27S100B doesn't turn off if two consecutive shorts are too close together  
  This is also an issue for the ACK detection. The second ACK would simply get lost in this case:  
  ![tps27s100b](data/images/tps27s100b.jpg)  

  The only reliable fix for this might be to monitor IMON as well. Here is a capture of the second ACK pulse of an MX645 during a DECUP update. IMON is above 500mV for about 8µs.
  ![decup_mx645_second_short_imon](data/images/decup_mx645_second_short_imon.png)  

  As it turns it this isn't a workaround either. While some shorts look like this  
  ![decup_mx645_second_short_imon_1060mV](data/images/decup_mx645_second_short_imon_1060mV.png)  

  Others look like this  
  ![decup_mx645_second_short_imon_420mV](data/images/decup_mx645_second_short_imon_420mV.png)  
  And that 420mV pulse is too short for a common LM339... now what?

- TPS27S100B could use another 1µF ceramic capacitor at IN (e.g. same as at OUT)

- N and P tracks are reversed  
  We currently generate the signal for INHA (output SHA) with the first half-bit 1 and the second half-bit 0. It must stay that way because we want the RMT end-of-transmission level to be 0. The problem is that a signal with the first half-bit 1 would mark the P and not the N track. Currently we feed SHA into the N pin though.  
  ![track](data/images/track_connector.png)  