# openhab-itho-fan

## Summary
Arjen Hiemstra created and sells an addon for an itho CVE fan. This makes it controlable by WiFi and of course a home automation system. This is an implementation for openHAB including a widget. It's small, so intended use is a small page (phone) or a tiled dashboard. The latter is my setup,so for this it's tested. Most difficult in widgets is responsive behaviour. So it may be required to change CSS to match your needs.

Features
1. Depending on the speed the fan will show 1 of 7 images. 
	* Below 30 it's shows grey (off, assuming you never completelly turn it off)
	* Between 30-70, it's on (green) with no stripes
	* Between 70-110, it's on, one stripe
	* Between 110-150, it's on, two stripes
	* Between 150-190, it's on, three stripes
	* Between 190-230, it's on, four stripes
	* Above 230, it's on, five stripes
2. In the center of the fan the speed is shown
	* It will show in % using the itho addon range 0-254
	* It can go below 20 but I read not all fans will keep turning at a very low setting. This is a safe value.
2. It has a low, medium, high button. 
	* This will set the corresponding speed (as set in the itho addon)
	* The chosen button will light up green (also when it's set by the remote!)
3. It had a timer button
	* This will set the fan to the high setting for a limited time (as set in the itho addon)
	* As long as the timer is active, the button will light up green (also when it's set by the remote)
	* (although it's the high setting, the high button will remain gray in this case)
4. It had a setup (wrench) button
	* It will show a popup screen you can use to manually set a value between 20 and 254
	* If it happens to be the low/medium/high (+-5), this button will show green
	* Any other value will make the setup/wrench button green
5. It had a humidity icon
	* Below 40 or above 60 (recommended values for health) it will be gray, else green
	* Pressing it will show a popup with 24 hours graph of humidity
	
It new at the moment, so work in progress. Expect changes. 
	* Next addition will be a rule to automaticly contro fan when showering or at day/night.
	* The current setup uses the web api with a refresh of one minute. Command is sent immidiatelly but the widget is delayed on updating. Maybe use mqtt? Or it's not worth it ...

## Preperation
1. Have an openhab installation ;-)
2. Own an itho central ventilation unit ;-)
3. Buy and install the Itho CVE RFT WiFi add-on, see [documentation](https://github.com/arjenhiemstra/ithowifi)
	* To use all features, also buy/install the 866 mhz radio controller and humidity sensor
	* If you own it, add your remote(s) to the addon (not the itho box).
4. When installing the above you can choose a host name (WiFi settings), remember it. The manual assumes 'fan'
5. Install in openhab the http binding, see [documentation](https://www.openhab.org/addons/bindings/http/)
6. Install in openhab the jsonpath transformation, see [documentation](https://www.openhab.org/addons/transformations/jsonpath/)

## Create thing
1. Create a new thing
2. Choose the HTTP binding
3. Don't use GUI but move to tab code
4. Copy and paste the yaml in the thing folder of this project
5. Save the thing 
6. Don't close it

## Create equipment
1. If it's closed, navigate to your fan thing
2. Move to the tab 'channels'
3. Use button 'Add equipment to model
4. Select all channels and add-on
5. Openhab will create/add equipment and items

## (Optional) Slider to set speed
When opening the current_speed it can't be controlled with the default GUI. You can configure a slider to make this possible
1. Open the item current_speed
2. Choose 'add metadata' 
3. Add 'State Description' and add a min of 20, a max of 250, a step of 5 (or whatever you like) and save
4. Add 'Default Standalone Widget' , choose 'oh-slider-card' and save
5. Usually you have to close and reopen the item. Now a slider appears you can use to change the speed of the fan

## Widget installation
1. Copy all images in the project folder widget-images to your openhab configuration 'html' folder 
1. Add a new widget
2. Remove default code
3. Copy and past the yaml in the widget code of this project and save

## Widget usage/configuration
1. Add a new widget to your dashboard/page
2. Set the props and save
	* Easiest is selecting only the equipment (items will be found on default names)
	* If you chose other names for the items, you can also select the seperate items
	* set low, medium and high according to the values in the itho addon system setup

## Code
The code is pretty standard. Things you might find interesting for changes
1. It uses the the web api at api.html (documentation  in the webpages of the add-on itself)
	* api.html?get=ithostatus (get most values in json format)
	* api.html?get=currentspeed (get speed in plaintext)
	* api.html?command=low/medium/high/timer1 (set speed to fixed value or timer)
	* api.html?speed=0-254 (set speed at given value)
2. It uses the the web api at queue.html (undocumented api to retrieve timer actions in json format)
3. Range openHab accepts for speed can be changed in metadata of the current_speed item
4. Since the settings for low/medium/high can't be retrieved from the api (as far as I know) the widget takes them as input parameters
5. Most behaviour is 'programmed' in the widget
6. The widget uses a trick to allow both the equipment as seperate items using the default values and concatenating them in the formulas
	* Like items[props.fan+props.speed].state
	* If the equipment 'fan' is given it adds the default item names 'fan' + '_current_speed'
	* If the items are given it adds '' + 'fan_current_speed'
	* Of cource, if both are supplied this trick won't work
7. If you're lazy, add your low/medium/high speed as a default. I used mine :)
	
