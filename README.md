# LoRaWanWeatherStation
<h2>Introduction</h2>
<p>This is an example of a nice LoRa application. The weather station contains a temperature sensor, air pressure sensor and humidity sensor. The data is read out and sent to Cayenne Mydevices and Weather Underground using LoRa and The Things Network.</p>
<p><img alt="file" src="https://ttnstaticfile.blob.core.windows.net/media/md_editor/image-1508417779575.large.png"><br>
<img alt="file" src="https://ttnstaticfile.blob.core.windows.net/media/md_editor/image-1508417844287.large.png"></p>
                
                  
<h2>The Hardware</h2>
<div>For this project I used the following hardware:</div><ul><li>Arduino Pro Mini 328 - 3.3V/8MHz&nbsp; (<a href="https://www.sparkfun.com/products/11114" rel="nofollow" target="_blank">https://www.sparkfun.com/products/11114</a>)</li><li>RFM95W&nbsp;(<a href="http://www.hoperf.com/rf_transceiver/lora/RFM95W.html" rel="nofollow" target="_blank">http://www.hoperf.com/rf_transceiver/lora/RFM95W.html</a>) (<a href="https://www.aliexpress.com/item/RFM95W-20dBm-100mW-868Mhz-915Mhz-DSSS-spread-spectrum-wireless-transceiver-module-SPI-SMD/32799536710.html" rel="nofollow" target="_blank">https://www.aliexpress.com/item/RFM95W-20dBm-100mW-868Mhz-915Mhz-DSSS-spread-spectrum-wireless-transceiver-module-SPI-SMD/32799536710.html</a>)</li><li>BME280 (<a href="https://www.aliexpress.com/item/BME280-Digital-Sensor-Temperature-Humidity-Barometric-Pressure-Sensor-Module-I2C-SPI-1-8-5V-GY-BME280/32848964559.html" rel="nofollow" target="_blank">https://www.aliexpress.com/item/BME280-Digital-Sensor-Temperature-Humidity-Barometric-Pressure-Sensor-Module-I2C-SPI-1-8-5V-GY-BME280/32848964559.html</a>)</li></ul>

<h2>The Wiring</h2>
<p><img src="https://ttnstaticfile.blob.core.windows.net/media/django-summernote/2017-10-19/811b9b9d-8271-4e64-a113-7fa30a6935aa.png" style=""></p><p>The wiring is based on the scheme of this story:&nbsp;<a href="https://www.thethingsnetwork.org/labs/story/build-the-cheapest-possible-node-yourself" rel="nofollow" target="_blank">https://www.thethingsnetwork.org/labs/story/build-the-cheapest-possible-node-yourself</a><a href="https://www.thethingsnetwork.org/labs/story/build-the-cheapest-possible-node-yourself" rel="nofollow" target="_blank"></a></p><p>Follow the instrucitons of&nbsp;<a href="https://www.thethingsnetwork.org/labs/story/build-the-cheapest-possible-node-yourself" rel="nofollow" target="_blank">BUILD THE CHEAPEST POSSIBLE NODE YOURSELF</a>&nbsp;to add the RFM95W to the Arduino Pro Mini.&nbsp;If successful, add the sensors as shown in the scheme above.&nbsp;</p><p>Finally, solder a 86 millimetre wire to the RFM95W antenna pin to increase the range.<br></p>

<h2>The Casing</h2>
<p></p><p>To place the weather station somewhere, I drew a case and printed it with the 3d printer.&nbsp;</p><p>The models can be found on Thingiverse. Of course you can of course make your own variant.&nbsp;</p><p><a href="https://www.thingiverse.com/thing:2594618" rel="nofollow" target="_blank">https://www.thingiverse.com/thing:2594618</a>

<h2>The Software</h2>
<p>The code I have used can be found on GitHub: <a href="https://github.com/henri98/LoRaWanWeatherStation" rel="nofollow" target="_blank">https://github.com/henri98/LoRaWanWeatherStation</a>&nbsp;</p><p> I used Atom with PlatformIO to realize this project, so this is a PlatformIO project. </p><p>I used the folowing libarys: </p><ul><li>LoraMAC-in-C for Arduino thank to&nbsp;Thomas Telkamp and Matthijs Kooijman (<a href="https://github.com/matthijskooijman/arduino-lmic" rel="nofollow" target="_blank">https://github.com/matthijskooijman/arduino-lmic</a>)</li><li>CayenneLPP of The Things Network Arduino Library (<a href="https://github.com/TheThingsNetwork/arduino-device-lib" rel="nofollow" target="_blank">https://github.com/TheThingsNetwork/arduino-device-lib</a>)</li><li>Adafruit DHT Humidity &amp; Temperature Unified Sensor Library (<a href="https://github.com/adafruit/DHT-sensor-library" rel="nofollow" target="_blank">https://github.com/adafruit/DHT-sensor-library</a>)</li><li>Low-Power: Lightweight low power library for Arduino (<a href="https://github.com/adafruit/DHT-sensor-library" rel="nofollow" target="_blank">https://github.com/adafruit/DHT-sensor-library</a>)</li></ul><p><br></p>

<h2>Weather Underground</h2>
<p>To send data to Weather underground, create an HTTP integration in the Console of The Things Network.&nbsp;&nbsp;The data will be sent to the URL with a POST or a GET.&nbsp;&nbsp;The following script captures the data and sends it to Weather Underground. Register your own Personal Weather Station on&nbsp;<a href="https://www.wunderground.com/personal-weather-station/signup" rel="nofollow" target="_blank">https://www.wunderground.com/personal-weather-station/signup</a>&nbsp;</p>
<pre>&lt;?php
        echo time();

        file_put_contents('json/post'.time().'.json', file_get_contents('php://input'));
        $json = file_get_contents('php://input');
        $data = json_decode($json);

        // take the data out of the json
        $temperature_1 = $data-&gt;payload_fields-&gt;temperature_1;
        $barometric_pressure_2 = $data-&gt;payload_fields-&gt;barometric_pressure_2;
        $relative_humidity_3 = $data-&gt;payload_fields-&gt;relative_humidity_3;

        // tempc to tempf
        $tempf = ($temperature_1 * 9/5) + 32;

        // pressure 
        $pressure = $barometric_pressure_2/33.863886666667;<br>

        if( isset($pressure) &amp;&amp; !empty($pressure) &amp;&amp; isset($tempf) &amp;&amp; !empty($tempf) &amp;&amp; isset($relative_humidity_3) &amp;&amp; !empty($relative_humidity_3)){&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <br>   file_get_contents("<a href="https://rtupdate.wunderground.com/weatherstation/updateweatherstation.php?ID=XXXXXXX&amp;PASSWORD=XXXXXXXX&amp;dateutc=now&amp;tempf=" rel="nofollow" target="_blank">https://rtupdate.wunderground.com/weatherstation/updateweatherstation.php?ID=XXXXXXX&amp;PASSWORD=XXXXXXXX&amp;dateutc=now&amp;tempf=</a>" . $tempf . "&amp;humidity=" . $relative_humidity_3 . "&amp;baromin=" . $pressure);
        }
</pre>
<img src="https://ttnstaticfile.blob.core.windows.net/media/django-summernote/2017-10-19/7c9ed53f-24e6-49f5-9640-83e6e950f5db.png" style="">


<img src="https://lh3.googleusercontent.com/2EEyawmH3rNHn1ICmcI4wHNzP1-plMN-IJxe3NnAsub2lSi9Zp1jWigjPEmqTnlTdkgrc150-cKWNF7q5sqJG9YTd4yc025VWEuFGw6Vczmv7FxojwAH0PHZHernPQTOCJg0n4YV3n28G4cQmKhcoJUWUemuw64o_M2pYzNppPF7c-25R_hjsCnmG1vbaC8iWtlcDZrDig82UTJvVyiBSjT-MDaof8mOEqhKD6YwUATIU5tDj0Ea7ArKiceiSxVLnBioM0xQ2RMr6EdKEvwv3CP7LvauiYLwdvDCT3lQ-oZ5UhhobHQNLrYVtR95zp-M2YXr6zbpcq0M4Y1mY8hVkBDsS3l_vOIqYVbNXUynr51Qhim5W8AGHRVkArBvvE7C7cSS_LeC7K9v0o1Ro21xkPvsVdyal8ZG6Jc2GkpmqtxyVKPyC3YxDlUP5OEhF5V0RVrEpfMO38Mivstr8NCtJZVwfbGohZ1s4lx3YDT48lOUJb7DJKX5NlSqsPIHx17cCrAXngzpLKMDJO7iEnof1KRW2g5gWLQfSnZSUgXEJDn_m49YEuQk9oi7pRXUeqNmEhCvaKmENnA2bNwH7sU7mp_6n62YN3VSn174ixyU0g=w1214-h910-no"><img src="https://lh3.googleusercontent.com/2EEyawmH3rNHn1ICmcI4wHNzP1-plMN-IJxe3NnAsub2lSi9Zp1jWigjPEmqTnlTdkgrc150-cKWNF7q5sqJG9YTd4yc025VWEuFGw6Vczmv7FxojwAH0PHZHernPQTOCJg0n4YV3n28G4cQmKhcoJUWUemuw64o_M2pYzNppPF7c-25R_hjsCnmG1vbaC8iWtlcDZrDig82UTJvVyiBSjT-MDaof8mOEqhKD6YwUATIU5tDj0Ea7ArKiceiSxVLnBioM0xQ2RMr6EdKEvwv3CP7LvauiYLwdvDCT3lQ-oZ5UhhobHQNLrYVtR95zp-M2YXr6zbpcq0M4Y1mY8hVkBDsS3l_vOIqYVbNXUynr51Qhim5W8AGHRVkArBvvE7C7cSS_LeC7K9v0o1Ro21xkPvsVdyal8ZG6Jc2GkpmqtxyVKPyC3YxDlUP5OEhF5V0RVrEpfMO38Mivstr8NCtJZVwfbGohZ1s4lx3YDT48lOUJb7DJKX5NlSqsPIHx17cCrAXngzpLKMDJO7iEnof1KRW2g5gWLQfSnZSUgXEJDn_m49YEuQk9oi7pRXUeqNmEhCvaKmENnA2bNwH7sU7mp_6n62YN3VSn174ixyU0g=w1214-h910-no"><img src="https://lh3.googleusercontent.com/2EEyawmH3rNHn1ICmcI4wHNzP1-plMN-IJxe3NnAsub2lSi9Zp1jWigjPEmqTnlTdkgrc150-cKWNF7q5sqJG9YTd4yc025VWEuFGw6Vczmv7FxojwAH0PHZHernPQTOCJg0n4YV3n28G4cQmKhcoJUWUemuw64o_M2pYzNppPF7c-25R_hjsCnmG1vbaC8iWtlcDZrDig82UTJvVyiBSjT-MDaof8mOEqhKD6YwUATIU5tDj0Ea7ArKiceiSxVLnBioM0xQ2RMr6EdKEvwv3CP7LvauiYLwdvDCT3lQ-oZ5UhhobHQNLrYVtR95zp-M2YXr6zbpcq0M4Y1mY8hVkBDsS3l_vOIqYVbNXUynr51Qhim5W8AGHRVkArBvvE7C7cSS_LeC7K9v0o1Ro21xkPvsVdyal8ZG6Jc2GkpmqtxyVKPyC3YxDlUP5OEhF5V0RVrEpfMO38Mivstr8NCtJZVwfbGohZ1s4lx3YDT48lOUJb7DJKX5NlSqsPIHx17cCrAXngzpLKMDJO7iEnof1KRW2g5gWLQfSnZSUgXEJDn_m49YEuQk9oi7pRXUeqNmEhCvaKmENnA2bNwH7sU7mp_6n62YN3VSn174ixyU0g=w1214-h910-no"><img src="https://lh3.googleusercontent.com/2EEyawmH3rNHn1ICmcI4wHNzP1-plMN-IJxe3NnAsub2lSi9Zp1jWigjPEmqTnlTdkgrc150-cKWNF7q5sqJG9YTd4yc025VWEuFGw6Vczmv7FxojwAH0PHZHernPQTOCJg0n4YV3n28G4cQmKhcoJUWUemuw64o_M2pYzNppPF7c-25R_hjsCnmG1vbaC8iWtlcDZrDig82UTJvVyiBSjT-MDaof8mOEqhKD6YwUATIU5tDj0Ea7ArKiceiSxVLnBioM0xQ2RMr6EdKEvwv3CP7LvauiYLwdvDCT3lQ-oZ5UhhobHQNLrYVtR95zp-M2YXr6zbpcq0M4Y1mY8hVkBDsS3l_vOIqYVbNXUynr51Qhim5W8AGHRVkArBvvE7C7cSS_LeC7K9v0o1Ro21xkPvsVdyal8ZG6Jc2GkpmqtxyVKPyC3YxDlUP5OEhF5V0RVrEpfMO38Mivstr8NCtJZVwfbGohZ1s4lx3YDT48lOUJb7DJKX5NlSqsPIHx17cCrAXngzpLKMDJO7iEnof1KRW2g5gWLQfSnZSUgXEJDn_m49YEuQk9oi7pRXUeqNmEhCvaKmENnA2bNwH7sU7mp_6n62YN3VSn174ixyU0g=w1214-h910-no"><span style="font-size: 12px;"><img src="https://ttnstaticfile.blob.core.windows.net/media/django-summernote/2017-10-19/5ae8f001-501c-49f4-8635-998464588e90.jpg" style="font-size: 14px;"><img src="https://ttnstaticfile.blob.core.windows.net/media/django-summernote/2017-10-19/130c3138-c4e2-4ec5-9768-8d801a2507a3.jpg" style="font-size: 14px;"><img src="https://ttnstaticfile.blob.core.windows.net/media/django-summernote/2017-10-19/2ca3ce99-b7c8-41c2-88ca-90802c851e88.jpg" style="font-size: 14px;"><img src="https://ttnstaticfile.blob.core.windows.net/media/django-summernote/2017-10-19/4d9f4308-5b7e-4378-a862-b525c4fa67f7.jpg" style="font-size: 14px;"><img src="https://ttnstaticfile.blob.core.windows.net/media/django-summernote/2017-10-19/54d728d9-7c77-4ff6-b902-01cb2511bd86.jpg" style="font-size: 14px;"></span><img src="https://ttnstaticfile.blob.core.windows.net/media/django-summernote/2017-10-19/0951e494-24c0-4930-b561-32a396f7bce6.jpg" style="">
