# python-script-to-chrome-browser-history
python script to Chrome browser history and save it in a file 

This Python script is designed to extract and display information about the browsing history from the Chrome browser. Let's break down the code step by step:

1. **Importing modules:**
   ```python
   import os
   import sqlite3
   import datetime
   ```

   - `os`: Provides a way to interact with the operating system, used here to build the path to the Chrome history database.
   - `sqlite3`: Enables interaction with SQLite databases, used for querying the Chrome history database.
   - `datetime`: Allows working with dates and times, used for converting timestamps in the Chrome history to readable date and time.

2. **Setting up paths:**
   ```python
   data_path = os.path.expanduser('~') + r"\AppData\Local\Google\Chrome\User Data\Default"
   history_db = os.path.join(data_path, 'History')
   ```

   - `data_path`: Constructs the path to the Chrome user data directory.
   - `history_db`: Specifies the path to the Chrome history database file.

3. **Timestamp conversion function:**
   ```python
   def convert_to_date_time(timestamp):
       return datetime.datetime(1601, 1, 1) + datetime.timedelta(microseconds=timestamp)
   ```

   - `convert_to_date_time`: Converts a timestamp (used in the Chrome history) to a human-readable date and time.

4. **Database querying function:**
   ```python
   def get_chrome_history(history_db):
       connection = sqlite3.connect(history_db)
       cursor = connection.cursor()
       select_statement = "SELECT urls.url, urls.visit_count, urls.last_visit_time FROM urls, visits WHERE urls.id = visits.url;"
       cursor.execute(select_statement)
       history = cursor.fetchall()

       with open("chrome_history.txt", "w") as file:
           for data in history:
               url = data[0]
               visit_count = data[1]
               last_visit_time = convert_to_date_time(data[2])
               file.write(f'URL: {url}, Visit Count: {visit_count}, Last Visit Time: {last_visit_time}\n')
   ```

   - `get_chrome_history`: Connects to the Chrome history database, executes a SQL query to retrieve URL, visit count, and last visit time, and writes the results to a file named "chrome_history.txt."

5. **Running the function:**
   ```python
   get_chrome_history(history_db)
   ```

   - Calls the `get_chrome_history` function with the specified history database path, initiating the extraction and writing process.

In summary, this script fetches data from the Chrome browsing history database, including URLs, visit counts, and last visit times, and writes the information to a text file for easy inspection.
