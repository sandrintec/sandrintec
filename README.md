- 👋  @sandrintec

from random import random

def getRandomHour():
  return int(24 * random())

def isTimeToWork(hour): 
  return hour <= 22

hour = getRandomHour()
while isTimeToWork(hour):
  print('It is {} o\'clock! Let\'s do something!'.format(hour))
  hour = getRandomHour()
  
print('It is {} o\'clock! It is time to sleep...'.format(hour))
<!---
sandrintec/sandrintec is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
exemplo... 
