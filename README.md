# repoone
import serial
import time
import requests
import json
firebase_url = 'https://proj-d34bf.firebaseio.com/'

ser = serial.Serial('COM3', 9600, timeout=0)

fixed_interval = 10
while 1:
  try:
         
    temperature_c = ser.readline()
    
    
    time_hhmmss = time.strftime('%H:%M:%S')
    date_mmddyyyy = time.strftime('%d/%m/%Y')
    
    
    temperature_location = 'Mumbai-Kandivali';
    print (temperature_c + ',' + time_hhmmss + ',' + date_mmddyyyy + ',' + temperature_location)
    
    
    data = {'date':date_mmddyyyy,'time':time_hhmmss,'value':temperature_c}
    result = requests.post(firebase_url + '/' + temperature_location + '/temperature.json', data=json.dumps(data))
    
    print ('Record inserted. Result Code = ' + str(result.status_code) + ',' + result.text)
    time.sleep(fixed_interval)
  except IOError:
    print('Error! Something went wrong.')
  time.sleep(fixed_interval)
