# -*- coding: utf-8 -*-
"""
Created on Wed Jan  4 14:05:31 2023

@author: PC
"""

class Transceiver (object):
    count = 0
    time = 0
    def __init__(self, x, y, tpower, rpower = pow(10, -3)):
        self._x = x
        self._y = y
        self._tpower = tpower
        self._rpower = rpower
        self.id = Transceiver.count
        Transceiver.count += 1
        #self.localtime = Transceiver.time
    def get_coordinate_x(self):
        return self._x
    def get_coordinate_y(self):
        return self._y
    def get_coordinates(self):
        self.coordinates = (self._x, self._y)
        return self.coordinates
    def get_tpower(self):
        return self._tpower
    def get_rpower(self):
        return self._rpower
    def get_id(self):
        return self.id
    def get_localtime(self):
        self.localtime = Transceiver.time
        return self.localtime
    def set_coordinates(self, x, y):
        self._x = x
        self._y = y
    def set_transmitting_power(self, tpower):
        self._tpower = tpower
    def set_receiving_power(self, rpower):
        self._rpower = rpower
    def update_local_time(self): #??
        self.localtime = Transceiver.time
    def distance(self, m):
        self.diff_x_sq = abs(self.get_coordinate_x() - m.get_coordinate_x())**2
        self.diff_y_sq = abs(self.get_coordinate_y() - m.get_coordinate_y())**2
        self.distance = (self.diff_x_sq + self.diff_y_sq)**(0.5)
        return self.distance
    def transmitted_power(self, x_y_tuple):
        (self.new_x, self.new_y) = x_y_tuple
        if self.get_coordinate_x() == self.new_x and self.get_coordinate_y() == self.new_y:
            return self._tpower
        else:
            self.diff_x_sq_new = abs(self.get_coordinate_x() - self.new_x)**2
            self.diff_y_sq_new = abs(self.get_coordinate_y() - self.new_y)**2
            self.distance_new = (self.diff_x_sq_new + self.diff_y_sq_new)**(0.5)
            self.transmitted_power = self._tpower / self.distance_new
            return self.transmitted_power
    # def __eq__(self):
    #     return t2.transmitted_power(t1.get_coordinates())
    def __str__(self):
        if (self.get_tpower() >= 1000):
            self.str_tpower = str(float(self.get_tpower() / 1000)) + 'kW'
        elif (1 <= self.get_tpower() < 1000):
            self.str_tpower = str(self.get_tpower()) + 'W'
        elif (self.get_tpower() < 1):
            self.str_tpower = str(float(self.get_tpower()*1000)) + 'mW'
        
        if (self.get_rpower() >= 1000):
            self.str_rpower = str(float(self.get_rpower() / 1000)) + 'kW'
        elif (1 <= self.get_rpower() < 1000):
            self.str_rpower = str(self.get_rpower()) + 'W'
        elif (self.get_rpower() < 1):
            self.str_rpower = str(float(self.get_rpower()*1000)) + 'mW'
            
        return ('Class: Tower\n' + 
                'Tower number: ' + str(self.id) + '\n'
                'Coordinates: ' + '<' + str(self.get_coordinate_x()) + ',' + str(self.get_coordinate_y()) + '>\n' + 
                'Transmitting Power: ' + self.str_tpower + '\n' + 
                'Min. Receiving Power: ' + self.str_rpower + '\n')
  

    
    
class Robot(Transceiver):
    robot_count = 0
    robot_ins = 0
    def __init__(self, x, y, vx, vy):
        Transceiver.__init__(self, x, y, tpower = 1, rpower = pow(10, -2))
        self._vx = vx
        self._vy = vy
        self._disconnect_time = Robot.robot_count
        self._robot_ins = Robot.robot_ins
        Robot.robot_count += 1
        Robot.robot_ins += 1
    def get_id(self):
        return self._robot_ins
    def get_velocity(self):
        return (self._vx, self._vy)
    def get_status(self):
        if self._status == True:
            return 'Alive'
        elif self._status == False:
            return 'Dead'
    def get_disconnet_time(self):
        return self._disconnect_time 
    def set_velocity(self, vx, vy):
        self._vx = vx
        self._vy = vy
    def set_status(self, newstatus = True):
        self._status = newstatus
    def update_disconnect(self):
        self._disconnect_time += 1
        return self._disconnect_time
    def set_disconnet_time(self):
        self._disconnect_time = 0
    def update_location(self):   
        Transceiver.time += 1 
        self._x = self.get_coordinate_x() + self._vx
        self._y = self.get_coordinate_y() + self._vy

    def __str__(self):
         if (self.get_tpower() >= 1000):
             self.str_tpower = str(float(self.get_tpower() / 1000)) + 'kW'
         elif (1 <= self.get_tpower() < 1000):
             self.str_tpower = str(self.get_tpower()) + 'W'
         elif (self.get_tpower() < 1):
             self.str_tpower = str(float(self.get_tpower()*1000)) + 'mW'
         
         if (self.get_rpower() >= 1000):
             self.str_rpower = str(float(self.get_rpower() / 1000)) + 'kW'
         elif (1 <= self.get_rpower() < 1000):
             self.str_rpower = str(self.get_rpower()) + 'W'
         elif (self.get_rpower() < 1):
             self.str_rpower = str(float(self.get_rpower()*1000)) + 'mW'
     
             
         return ('Class: Robot\n' + 
                'Robot number: ' + str(self._robot_ins) + '\n' +
                'Current Coordinates: ' + '<' + str(self.get_coordinate_x()) + ',' + str(self.get_coordinate_y()) + '>\n' +
                'Current Velocity: ' + '<' + str(self._vx) + ',' + str(self._vy) + '>\n' +
                'Transmitting Power: ' + self.str_tpower + '\n' +
                'Min. Receiving Power: ' + self.str_rpower + '\n' +
                'Status: ' + str(self.get_status()))
        
    
class Guard(Robot):
    def __init__(self, x, y, vx, vy, period = 60, localtime = 0):
        Robot.__init__(self, x, y, vx, vy)
        self._period = period
        self._localtime = localtime
        # localtime += 1
    def get_period(self):
        return self._period
    def set_period(self, period):
        self._period = period
    def update_location(self):
        self._localtime += 1 #Yanlış


t1 = Transceiver(0,0,100)
t2 = Transceiver(750,500,100,0.1)
print(t1.get_coordinate_x())
print(t2.get_coordinate_x())        
print(t1.get_coordinate_y())
print(t2.get_coordinate_y())  
print(t1.get_coordinates())
print(t2.get_coordinates())
print(t1.get_tpower())
print(t2.get_tpower())
print(t1.get_rpower())
print(t2.get_rpower())
# print(t1.get_id())
# print(t2.get_id())
# print(t1.get_localtime())
# print(t2.get_localtime()) #•0
t1.set_coordinates(-250, 0)
print(t1.get_coordinates())
t1.set_transmitting_power(1000)
print(t1.get_tpower())
t1.set_receiving_power(1.5)
print(t1.get_rpower())    
# t1.update_local_time(50)
# print(t1.get_localtime())    
print(t1.distance(t2))
print(t1.transmitted_power(t1.get_coordinates()))
print(t1)
print(t2)


print('Robot class: ')
r1 = Robot(10,25,10,0)
r2 = Robot(500,750,-7.5,6.8)
print(r1.get_velocity())
print(r2.get_velocity())
r1.set_status(False)
r2.set_status()
print(r1.get_status())
print(r2.get_status())

r1.update_disconnect()
print(r1.get_disconnet_time())
r1.update_disconnect()
print(r1.get_disconnet_time())
r1.set_disconnet_time()
print(r1.get_disconnet_time())

r1.set_velocity(-5, 10)
print(r1.get_velocity())



r1.update_location()
print(r1.get_coordinates())
print(r1.get_localtime())
r1.update_location()
print(r1.get_coordinates())
print(r1.get_localtime())

print(r1)
print(r2)

t3 = Transceiver(400,250,1000)
print(t1.transmitted_power(r1.get_coordinates()))
print(r1.transmitted_power(t1.get_coordinates()))
print(r1.get_coordinate_x())
print(r1.get_coordinate_y())
print(r1.get_coordinates())
print(r2.get_coordinates())
print(r1.get_tpower())
print(r1.get_rpower())
print(r1.get_id())
print(r2.get_id())



print('Guard class: ')
r3 = Guard(40,58,10,12,120,50)
r4 = Guard(750,800,-10,0,100)
r5 = Guard(600,700,-10,10)
print(r3.get_period())
print(r4.get_period())
print(r5.get_period())
r3.set_period(60)
print(r3.get_period())
r3.update_location()
print(r3.get_velocity())
print(r3.get_localtime())
