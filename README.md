# RaspiConfiguration
This repo documents initial setup for raspberry pi needed for kernel development

## Setup ethernet IP address for direct connection with PC.

  After flashing OS image onto SD, make following edits to /etc/dhcpcd.conf
  
  interface eth0
  static ip_address=192.168.0.111/24  # Set the static IP for Raspberry Pi
  # static routers=192.168.1.1  # Optional, can be left out for direct connection
  # static domain_name_servers=8.8.8.8  # Optional, can be left out for direct connection

  Then, reboot raspberry pi.
