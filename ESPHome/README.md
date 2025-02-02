# ESPHome code
In this folder the ESPHome YAML codes are stored for the ESP-07S module that is replacing the original HF-LPT20 WiFi+BT module

The example code id for WVC-600W-Life, WVC-700W-Life and WVC-800W-Life micro inverters.

<h3 tabindex="-1" class="heading-element" dir="auto">Important</h3>
To compile the ESPHome code for the ESP-07S it is necessary to copy the file uart_read_line_sensor.h to the ESPHome file directory of your Home Assistant.

There is another example code for an ESP8266 D1 Mini V3 with enhanced UART comunication features. This was used for initial debugging of the internal serial interface between the HF-LPT27 module and the inverter main controller. 
