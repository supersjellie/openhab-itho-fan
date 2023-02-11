# openhab-itho-fan

## Summary
Arjen Hiemstra created and sells an addon for an itho CVE fan. This makes it controlable by WiFi and of course a home automation system. This is an implementation for openHAB including a widget. It's small, so intended use is a small page (phone) or a tiled dashboard. The latter is my setup,so for this it's tested. Most difficult in widgets is responsive behaviour. So it may be required to change CSS to match your needs.

## Preperation
1. Have an openhab installation ;-)
2. Own an itho central ventilation unit ;-)
3. Buy and install the Itho CVE RFT WiFi add-on, see [documentation](https://github.com/arjenhiemstra/ithowifi)
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

	
