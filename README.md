# HA-ESPHome-WVC-Inverter
HA ESPHome code for replacement of the WiFi module inside the KaiDeng WVC-Life Micro Inverters.

Then to micro inverter is directly sending the actual values to Home Assistant. No cloud communication is active anymore :)

<img src="https://github.com/GernotAlthammer/HA-ESPHome-WVC-Inverter/blob/main/Pictures/WVC800_HA-Page.png">

In the folder ESPHome are 3 exapmles for the YAML code of the Inverter models WVC-600W, WVC-700W and WVC-800W.

That code needs to be changed in some of the default values in order to mach your Inverter model settings.

<h3 tabindex="-1" class="heading-element" dir="auto">Limitations</h3>
The code can cover the following WVC Inverter models of the "Wireless Series R3"

- WVC-300W-Life
- WVC-350W-Life
- WVC-400W-Life
- WVC-600W-Life
- WVC-700W-Life
- WVC-800W-Life
- WVC-1200W-Life
- WVC-1400W-Life
- WVC-1600W-Life
- WVC-2000W-Life
- WVC-2400W-Life
- WVC-2800W-Life

The ESP-07S code incudes

- a Web-Server interface for Online status and OTA
- a WiFi Fallback Hotspot" to setup Wifi network logon data in your W-LAN
- a Home Assistant service "write_uart0" to send commands to the interters microcontroller

For the WVC micro inverters several users have reported unstable network communication and other unstable situations. Many of those problems are related to an uncontrolled overheating of the HF-LPT270 WiFi module.
The Wifi module does also create additional heat and is not conencted to any cooling elements inside the inverter housing.
Therefore it is strongly recommended to add as many thermal pads onto the module that are needed to get connection to the aluminium housing lid.
This helps to provide enought cooling to the module to prevent from overheating during normal operations.


<h3 tabindex="-1" class="heading-element" dir="auto">Notes</h3>
I'm working on this project as a hobby. My work on this software is in no way associated with a company. If you like to use it, or improve on it, feel free. Use it at your own risk - it might work perfectly or it might not.


THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


Edit: 2025-02-02
