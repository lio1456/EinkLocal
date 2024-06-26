### driver.py

This file contains the driver for the E-Ink display. The driver is based on a driver provided by Waveshare but has been customized for this project.

#### Customizations in driver.py

- **RAM Allocation**: Modifications have been made to allocate RAM for large byte arrays first to avoid heap fragmentation.
  ```python
  buffer_black = bytearray(48000)
  buffer_red = bytearray(48000)
  ```

- **Display Initialization**: Customizations to handle display hardware initialization and prevent memory issues.
  ```python
  class EPD_7in5_B:
      def __init__(self, tiles):
          global buffer_black, buffer_red
          self.tiles = tiles
          self.buffer_black = buffer_black
          self.buffer_red = buffer_red
          ...
- **Check Battery Voltage**: The code reads the battery voltage using an ADC (Analog-Digital Converter) and prints the voltage. If the voltage is below 2V, it indicates the battery is empty.
  ```python

  adc = ADC(28)
  sensor_value = adc.read_u16()
  voltage = sensor_value * (3.3 / 65535)
  if voltage < 2:
      print("Battery empty: " + str(voltage) + "V")
  print("Battery Voltage: " + str(voltage) + "V")
  del adc
  ```

- **Custom Methods**:
  - **refresh_framebuffer**: Updates the framebuffer with the current data from the tiles.
    ```python
    def refresh_framebuffer(self):
        self.imageblack.fill(0xff)
        self.imagered.fill(0x00)
        
        for tile in self.tiles:
            if not isinstance(tile, tiles.Tile):
                raise TypeError("Invalid tile type passed.")
            tile.draw_canvas(self)
    ```

  - **init**: Customizations to initialize the Wi-Fi connection and retrieve configuration data.
    ```python
    def init(self):
        ...
        result = connect_to_wifi(config.wifi_config['ssid'], config.wifi_config['password'])
        print("Connection status:", result)
        ...
  - **Power Management**: Includes methods to turn off the display and put it into a deep sleep state to conserve energy:
    ````python
    def sleep(self):
      self.send_command(0x02) # power off
      self.WaitUntilIdle()
      self.send_command(0x07) # deep sleep
      self.send_data(0xa5)
    ```
### icons.py

This file defines various icons that can be displayed on the E-Ink display.

- **Icon Class**: Contains byte arrays for different icons (e.g., sun, cloud) used in the weather display.
  ```python
  class Icon():
      sun_data = bytearray([...])
      bewoelkt_data = bytearray([...])
      cloud_data = bytearray([...])
  ```

- **draw_icon Method**: Creates FrameBuffers for the icons.
  ```python
  def draw_icon(self, icon):
      if icon == "sun":
          icon_fb = framebuf.FrameBuffer(self.sun_data, 100, 100, framebuf.MONO_HLSB)
      elif icon == "bewoelkt":
          icon_fb = framebuf.FrameBuffer(self.bewoelkt_data, 100, 100, framebuf.MONO_HLSB)
      elif icon == "wolke":
          icon_fb = framebuf.FrameBuffer(self.cloud_data, 100, 100, framebuf.MONO_HLSB)
      return icon_fb
  ```
### tiles.py

This file defines various types of tiles that can be displayed on the dashboard.

- **Tile Class**: Base class for all tile types, defining basic functions.
  ```python
  class Tile():
      width = 100
      height = 100
      
      def __init__(self, x, y):
          self.y = y
          self.x = x
          
      def draw_canvas(self, can):
          can.imagered.fill(0x00)
          can.imageblack.fill(0xff)
  ```

- **tile_gallery Class**: Tile to display a gallery of sub-tiles.
  ```python
  class tile_gallery(Tile):
      def __init__(self, tiles):
          self.tiles= tiles
          self.num_tiles = len(tiles)
          self.current_index = 0
  ```

- **Clock_Tile_s Class**: Tile to display the time.
  ```python
  class Clock_Tile_s(Tile):
      width = 240
      height = 240
      
      def draw_canvas(self, can):
          ...
  ```

- **Weather_Tile_s and Weather_Tile_l Classes**: Tiles to display weather information.
  ```python
  class Weather_Tile_s(Tile):
      width = 240
      height = 240
      
      def draw_canvas(self, can):
          ...
  
  class Weather_Tile_l(Tile):
      width = 560
      height = 480
      
      def draw_canvas(self, can):
          ...
  ```

- **ToDo_Tile Class**: Tile to display to-do lists.
  ```python
  class ToDo_Tile(Tile):
      width = 560
      height = 480
      
      def draw_canvas(self, can):
          ...
  ```

- **Calendar_Tile Class**: Tile to display calendar events.
  ```python
  class Calendar_Tile(Tile):
      width = 560
      height = 480
      
      def draw_canvas(self, can):
          ...
  ```

- **News_Tile Class**: Tile to display news and stock prices.
  ```python
  class News_Tile(Tile):
      width = 560
      height = 480
      
      def draw_canvas(self, can):
          ...
  ```



