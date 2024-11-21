# Serial
- spi: full duplex, single master slave, faster i2c 
- i2c: half duplex, multi master, multi slave, easy to add new slaves 
- UART: 
	- special, not a protocol but a circuit 
	- duplex like SPI 
	- 2-wire like i2c, low speed communication
# Serial & Ethernet   
see [[tcp ip and protocols]]
### Serial: 
- called since RS-232 
- transmit individuals bytes
### Ethernet 
- transmit packet (64-1536 bytes)
-> Serial transfer synchronously (chronological order), Ethernet asynchronously (just ensure that sender and receiver has same data)

**Notes**: 
- Ethernet is more secured,fast, reliable
- Serial is faster to setup (no network card, IP address needed )
-> also if a network has Ethernet already, we can setup another network with Serial easily, if setup another Ethernet second network card is required.  



# serial communication
--- 
# Refererences 
[I2C vs SPI vs UART – Introduction and Comparison of their Similarities and Differences - Total Phase Blog](https://www.totalphase.com/blog/2021/12/i2c-vs-spi-vs-uart-introduction-and-comparison-similarities-differences/#:~:text=UART%20vs%20I2C%20vs%20SPI,send%20and%20receive%20the%20data.) 
[Choosing between serial and Ethernet](http://www.velocity11.com/techdocs/helpsystem/vworks_ug/choosingbetweenserialandethernet.html#:~:text=Ethernet%20is%20a%20faster%2C%20more,to%20use%20an%20Ethernet%20network.)
[What is the difference between Ethernet and Serial communication? - Electrical Engineering Stack Exchange](https://electronics.stackexchange.com/questions/31171/what-is-the-difference-between-ethernet-and-serial-communication)

2022 09 23 19:39
#cheatsheet  [[networking]] [[low level]] [[embedded]] 