# python-script-to-chrome-browser-history
python script to Chrome browser history and save it in a file 

import os
import sqlite3
import datetime

# Path to user's history database (Chrome)
data_path = os.path.expanduser('~') + r"\AppData\Local\Google\Chrome\User Data\Default"
history_db = os.path.join(data_path, 'History')

# API to convert timestamp to readable date_time
def convert_to_date_time(timestamp):
    return datetime.datetime(1601, 1, 1) + datetime.timedelta(microseconds=timestamp)

# Querying the DB
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

# Running the function
get_chrome_history(history_db)
