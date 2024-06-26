## File Descriptions

### config.py

This file contains configuration parameters for the various services used by the dashboard.

- **wifi_config**: Stores the SSID and password for the Wi-Fi connection.
  ```python
  wifi_config = {
      'ssid': 'xxxxxxx',
      'password': '******'
  }
  ```

- **weather_config**: Contains the URL for fetching weather data.
  ```python
  weather_config = {
      'url': 'https://api.open-meteo.com/v1/forecast?latitude=49.1399&longitude=9.2205&daily=weather_code,temperature_2m_max,temperature_2m_min'
  }
  ```

- **calendar_config**: Contains the URL for fetching calendar events.
  ```python
  calendar_config = {
      'url': 'http://p139-caldav.icloud.com/published/2/MTE1NzQwNjE0NzQxMTU3NDmLjpu4-S1Y9s3ZY6FOrrHnIwf0-kAOhn-6sr24tcTFaqodbcvwQ-1iUIyMWm_Q-QI6qieG29wXJSUU0hdN6JI'
  }
  ```

- **toDo_config**: Stores the API token and project ID for fetching to-do lists.
  ```python
  toDo_config = {
      'api_token': 'a7f3b805fdf80789ea5953f6d4eb7a799309a9b1',
      'project_id': '2332430307'
  }
  ```

- **news_config**: Contains the URL for fetching news.
  ```python
  news_config = {
      'url': 'https://www.alphavantage.co/query?function=NEWS_SENTIMENT&tickers=COIN,CRYPTO:BTC,FOREX:USD&time_from=20220410T0130&limit=10&apikey=7I5ZTASNKQWHN3PT'
  }
  ```

- **stock_config**: Stores the stock symbol and API token for fetching stock prices.
  ```python
  stock_config = {
      'symbol': 'AAPL',
      'api_token': '7I5ZTASNKQWHN3PT'
  }
  ```

 
### main.py

The main file of the project. It initializes the system and contains the logic for displaying various tiles of information on the E-Ink display.

- **Button Initialization**: Configures buttons for navigating between tiles.
  ```python
  btn_next = Button(3)
  btn_next.when_pressed = button_next
  btn_prev = Button(2)
  btn_prev.when_pressed = button_prev
  ```

- **Layout Definition**: Defines the layout of the tiles to be displayed.
  ```python
  gallery = tiles.tile_gallery([
          tiles.Calendar_Tile(0,0),
          tiles.News_Tile(0,0),
      ])
  
  layout = [
      tiles.Clock_Tile_s(560, 0),
      tiles.Weather_Tile_s(560, 240),
      gallery,
  ]
  ```

- **Display Logic**: Contains the main loop for updating the display and handling button events.
  ```python
  while True:
      update_flag = 60
      display.init()
      display.display()
      display.delay_ms(1000)
      display.sleep()
      display.delay_ms(1000)
      while update_flag > 0:
          update_flag -= 1
          time.sleep(1)
  ```
### CustomTiles.py
  
This file defines custom tile classes for the project, including a template class for creating new tiles with specific drawing functions.
  
  - **Template_Tile Class**: The `Template_Tile` class is a subclass of `Tile` and provides a structure for drawing on a canvas. This class includes attributes for width and height and a `draw_canvas` method for custom drawing.
     ```python
    from tiles import Tile
    import framebuf
  
    class Template_Tile(Tile):
      
      width = 240
      height = 240
      
      def draw_canvas(self, can):
          # Draw a rectangle
          can.fill_rect(10, 10, 100, 50, 1)  # Parameters: x, y, width, height, color
          
          # Draw text
          can.text('Hello, World!', 20, 70, 1)  # Parameters: text, x, y, color
    ```
  - **Class Definition:**
    - `class Template_Tile(Tile):` Defines a new class `Template_Tile` that inherits from the `Tile` class.
    
  - **Attributes:**
    - `width = 240`: Sets the width of the tile to 240 pixels.
    - `height = 240`: Sets the height of the tile to 240 pixels.
  
  - **Methods:**
    - `def draw_canvas(self, can):` Defines a method for drawing on the canvas.
      - **fill_rect(x, y, width, height, color):** This method draws a filled rectangle on the canvas. The parameters specify the position (`x`, `y`), dimensions (`width`, `height`), and color of the rectangle.
      - **text(text, x, y, color):** This method draws text on the canvas. The parameters specify the text to be drawn, the position (`x`, `y`), and the color of the text.
  
