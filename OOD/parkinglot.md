Design a parking lot

system requirements:    
multiple floors, each floor have many parking spots,    
multiple entry and exit points,     
support multiple types of parking spots such as Compact, Large, Handicapped, Mortorcycle,   
spots for electric cars? have an electric panel through which customers can pay and charge their vehicles,  
customers can collect ticket from entry points and pay at exit points,   
customers can pay at the exit panel or the customer’s info portal, or pay to parking attendant,  
pay via cash or credit cards,  
if the parking lot is full, show full message at the entrance panel, not allow more vehicles,  
each floor has a dispaly board showing free parking spot for each spot type;  
parking fee model. 

Actors:  
Admin: add or modifying floors, parking spots, entrance, exit panels, parking attendants. 
Customer: get parking tickets and pay for it. 
Parking attendant: can do everything customer can do for customer, can take cash for ticket payment. 
System: display messages on info panels, assign and remove a vehicle from a parking lot. 
 


Use cases:  
add/remove/edit  parking floor. 
add/remove/edit parking spot. 
add/remove a parking attendant. 
provide ticket. 
scan ticket. 
credit card payment. 
cash payment. 
add/modify  parking rate. 




main classes:  

ParkingLot: have multiple parking floors. 
attributes: name, address. 

ParkingFloor: have many parking spots. 

ParkingSpot:  
type: Handicapped, Compact, Large, Mortorcycle, Elecric. 

Account:  
type: Admin, parking attendant.   

Parking ticket:    
assign ticket when customer enters the parking lot    
 
Vehicle:  
type: car, truck, electric, van, motorcycle.    

EntrancePanel and ExitPanel: EntrancePanel print ticksts, ExictPanel can be used to pay ticket fee  

Payment: pay with cash or credit card.    

ParkingRate: keep track of the hourly parking rate.   

ParkingDisplayBoard: show available parking spots for each spot type, each floor has one. 

ParkingAttendantPortal: encapsulate operations an attendant can perform like scanning tickets and processing payments. 

CustomerInfoPortal: encapsulate the info portal that customers use to pay for ticket.
Once paid, the info portal will update the ticket to keep track of the payment. 

ElectricPanel: useed to charge vehicles and pay.  

```python

Code:
Enums and Constants. 

class VehicleType(Enum):
	CAR, TRUCK, VAN, MOTORBIKE, ELECTRIC = 1, 2, 3, 4, 5

class ParkingSpotType(Enum):
	HANDCAPPED, COMPCT, LARGE, MOTORBIKE, ELETRIC = 1, 2, 3, 4, 5

class AccountStatus(Enum):
	ACTIVE, BLOCKED, BANNED = 1, 2, 3

class ParkingTicketStatus(Enum):
	ACTIVE, PAID, LOST = 1, 2, 3

class Address:
	def __init__(self, street, city, state, zip_code, country):
		self.__street_address = stree
		self.__city = city
		self.__state = state
		self.__zip_code = zip_code
		self.__country = country

class Person():
	def __init__(self, name, email, phone,address):
		self.__name = name
		self.__email = email
		self.__phone = phone
		self.__address = address  # address object
	

===================================================================
class Account:
  def __init__(self, user_name, password, person, status=AccountStatus.Active):
    self.__user_name = user_name
    self.__password = password
    self.__person = person  # person object
    self.__status = status

  def reset_password(self):
    None

# extend Account
class Admin(Account):  
  def __init__(self, user_name, password, person, status=AccountStatus.Active):  
    super().__init__(user_name, password, person, status)

  def add_parking_floor(self, floor):
    None

  def add_parking_spot(self, floor_name, spot):
    None

  def add_parking_display_board(self, floor_name, display_board):
    None

  def add_customer_info_panel(self, floor_name, info_panel):
    None

  def add_entrance_panel(self, entrance_panel):
    None

  def add_exit_panel(self, exit_panel):
    None

# extend Account
class ParkingAttendant(Account):
  def __init__(self, user_name, password, person, status=AccountStatus.Active):
    super().__init__(user_name, password, person, status)

  def process_ticket(self, ticket_number):
    None
    
======================================================================================
class ParkingSpot(ABC):
  def __init__(self, identifier, parking_spot_type):
    self.__identifier = identifier
    self.__free = True
    self.__vehicle = None
    self.__parking_spot_type = parking_spot_type

  def is_free(self):
    return self.__free

  def assign_vehicle(self, vehicle):
    self.__vehicle = vehicle
    free = False

  def remove_vehicle(self):
    self.__vehicle = None
    free = True


# extend ParkingSpot
class HandicappedSpot(ParkingSpot):
  def __init__(self, identifier):
    super().__init__(identifier, ParkingSpotType.HANDICAPPED)


class CompactSpot(ParkingSpot):
  def __init__(self, identifier):
    super().__init__(identifier, ParkingSpotType.COMPACT)


class LargeSpot(ParkingSpot):
  def __init__(self, identifier):
    super().__init__(identifier, ParkingSpotType.LARGE)


class MotorbikeSpot(ParkingSpot):
  def __init__(self, identifier):
    super().__init__(identifier, ParkingSpotType.MOTORBIKE)


class ElectricSpot(ParkingSpot):
  def __init__(self, identifier):
    super().__init__(identifier, ParkingSpotType.ELECTRIC)


===================================================================================
from abc import ABC, abstractmethod

class Vehicle(ABC):
  def __init__(self, license_number, vehicle_type, ticket=None):
    self.__license_number = license_number
    self.__type = vehicle_type
    self.__ticket = ticket

  def assign_ticket(self, ticket):
    self.__ticket = ticket


class Car(Vehicle):
  def __init__(self, license_number, ticket=None):
    super().__init__(license_number, VehicleType.CAR, ticket)


class Van(Vehicle):
  def __init__(self, license_number, ticket=None):
    super().__init__(license_number, VehicleType.VAN, ticket)


class Truck(Vehicle):
  def __init__(self, license_number, ticket=None):
    super().__init__(license_number, VehicleType.TRUCK, ticket)


# Similarly we can define classes for Motorcycle and Electric vehicles

==============================================================================
class ParkingDisplayBoard:
  def __init__(self, id):
    self.__id = id
    self.__handicapped_free_spot = None
    self.__compact_free_spot = None
    self.__large_free_spot = None
    self.__motorbike_free_spot = None
    self.__electric_free_spot = None

  def show_empty_spot_number(self):
    message = ""
    if self.__handicapped_free_spot.is_free():
      message += "Free Handicapped: " + self.__handicapped_free_spot.get_number()
    else:
      message += "Handicapped is full"
    message += "\n"

    if self.__compact_free_spot.is_free():
      message += "Free Compact: " + self.__compact_free_spot.get_number()
    else:
      message += "Compact is full"
    message += "\n"

    if self.__large_free_spot.is_free():
      message += "Free Large: " + self.__large_free_spot.get_number()
    else:
      message += "Large is full"
    message += "\n"

    if self.__motorbike_free_spot.is_free():
      message += "Free Motorbike: " + self.__motorbike_free_spot.get_number()
    else:
      message += "Motorbike is full"
    message += "\n"

    if self.__electric_free_spot.is_free():
      message += "Free Electric: " + self.__electric_free_spot.get_number()
    else:
      message += "Electric is full"

    print(message)

========================================================================================

class ParkingFloor:
  def __init__(self, name):
    self.__name = name
    self.__handicapped_spots = {}
    self.__compact_spots = {}
    self.__large_spots = {}
    self.__motorbike_spots = {}
    self.__electric_spots = {}
    self.__info_portals = {}
    self.__display_board = ParkingDisplayBoard()

  def add_parking_spot(self, spot_identifier, spot_type):
  	if spot_type == "Handicapped":
		self.__handicapped_spots[spot_identifier] = HandicappedSpot(spot_identifier)
	elif spot_type == "Compact":
		self.__compact_spots[spot_identifier] = CompactSpot(spot_identifier)
	...

# after assign vehicle, need to update dispaly board
  def assign_vehicleToSpot(self, vehicle):  
    if vehicle == VehicleType.CAR:
    	
    spot.assign_vehicle(vehicle)
    update_display_board(spot)
    self.__display_board.show_empty_spot_number()


  def update_display_board_for_handicapped(self, spot):


      self.__display_board.show_empty_spot_number()


  def free_spot(self, spot):
    spot.remove_vehicle()
    switcher = {
      ParkingSpotType.HANDICAPPED: self.__free_handicapped_spot_count += 1,
      ParkingSpotType.COMPACT: self.__free_compact_spot_count += 1,
      ParkingSpotType.LARGE: self.__free_large_spot_count += 1,
      ParkingSpotType.MOTORBIKE: self.__free_motorbike_spot_count += 1,
      ParkingSpotType.ELECTRIC: self.__free_electric_spot_count += 1,
    }

    switcher(spot.get_type(), 'Wrong parking spot type!')
 


```
