# OOP-Project
Travel Agency Management System (OOP C++)
## Responsibilities

### Travel Management System Implementation

This project implements a travel agency management system with the following responsibilities:

- Manage customers, cabs, hotels, bookings, and billing.
- Save and load data for customers and bookings from files.
- Provide classes for different types of vehicles (standard, luxury), hotel rooms, and packages.
- Handle booking, billing, and receipt generation.
- Display menus and manage user interaction for bookings and cancellations.


### Travel Management System Implementation

This project implements a travel agency management system with the following responsibilities:

- Manage customers, cabs, hotels, bookings, and billing.
- Save and load data for customers and bookings from files.
- Provide classes for different types of vehicles (standard, luxury), hotel rooms, and packages.
- Handle booking, billing, and receipt generation.
- Display menus and manage user interaction for bookings and cancellations.

#### Main Implementation Code

          // ==================== ROOM AND HOTEL CLASSES IMPLEMENTATION ====================

          Room::Room() : roomId(""), roomType(""), pricePerNight(0.0), isBooked(false) {}

          Room::Room(string id, string type, double price) 
             : roomId(id), roomType(type), pricePerNight(price), isBooked(false) {}

          string Room::getRoomId() const { return roomId; }
           string Room::getRoomType() const { return roomType; }
           double Room::getPricePerNight() const { return pricePerNight; }
           bool Room::getBookingStatus() const { return isBooked; }
           void Room::setBookingStatus(bool status) { isBooked = status; }

            void Room::displayRoom() {
            cout << "Room ID: " << roomId << endl;
            cout << "Room Type: " << roomType << endl;
            cout << "Price per Night: $" << pricePerNight << endl;
            cout << "Status: " << (isBooked ? "Booked" : "Available") << endl;
          }

         HotelPackage::HotelPackage() : packageId(""), packageName(""), packagePrice(0.0) {}

         HotelPackage::HotelPackage(string id, string name, double price) 
         : packageId(id), packageName(name), packagePrice(price) {}

         string HotelPackage::getPackageId() const { return packageId; }
         string HotelPackage::getPackageName() const { return packageName; }
         double HotelPackage::getPackagePrice() const { return packagePrice; }

         void HotelPackage::displayPackage() {
         cout << "Package ID: " << packageId << endl;
         cout << "Package Name: " << packageName << endl;
         cout << "Package Price: $" << packagePrice << endl;
      }

       Hotel::Hotel() : hotelId(""), hotelName(""), location(""), rating(0), roomCount(0), packageCount(0) {}

       Hotel::Hotel(string id, string name, string loc, int rate) 
             : hotelId(id), hotelName(name), location(loc), rating(rate), roomCount(0), packageCount(0) {}

       string Hotel::getHotelId() const { return hotelId; }
       string Hotel::getHotelName() const { return hotelName; }

       void Hotel::addRoom(const Room& room) {
        if (roomCount < 5) {
        rooms[roomCount++] = room;
       }
      }

      void Hotel::addPackage(const HotelPackage& package) {
         if (packageCount < 3) {
          packages[packageCount++] = package;
       }
       }

        void Hotel::displayHotel() {
        cout << "Hotel ID: " << hotelId << endl;
        cout << "Hotel Name: " << hotelName << endl;
        cout << "Location: " << location << endl;
        cout << "Rating: " << rating << " stars" << endl;
        cout << "Available Rooms:" << endl;
        for (int i = 0; i < roomCount; i++) {
           if (!rooms[i].getBookingStatus()) {
               rooms[i].displayRoom();
               cout << "---" << endl;
        }
        }
        cout << "Available Packages:" << endl;
        for (int i = 0; i < packageCount; i++) {
           packages[i].displayPackage();
           cout << "---" << endl;
    }
      }

    Room* Hotel::findAvailableRoom(string roomType) {
        for (int i = 0; i < roomCount; i++) {
           if (rooms[i].getRoomType() == roomType && !rooms[i].getBookingStatus()) {
            return &rooms[i];
        }
    }
    return nullptr;
    }

    Room* Hotel::findRoomById(string roomId) {
        for (int i = 0; i < roomCount; i++) {
           if (rooms[i].getRoomId() == roomId) {
            return &rooms[i];
        }
    }
      return nullptr;
     }

     HotelPackage* Hotel::findPackage(string packageId) {
        for (int i = 0; i < packageCount; i++) {
           if (packages[i].getPackageId() == packageId) {
            return &packages[i];
        }
    }
    return nullptr;
     }

    // ==================== BOOKING CLASSES IMPLEMENTATION ====================

    Booking::Booking(string id, string custId, string date) 
       : bookingId(id), customerId(custId), bookingDate(date), status("Active"), totalCost(0.0) {}

    Booking::~Booking() {}

       string Booking::getBookingId() const { return bookingId; }
       string Booking::getCustomerId() const { return customerId; }
       double Booking::getTotalCost() const { return totalCost; }
       string Booking::getStatus() const { return status; }
       void Booking::setStatus(string s) { status = s; }


     Booking* Booking::fromString(const string& data, TravelAgency* agency) {
        size_t firstComma = data.find(',');
        string type = data.substr(0, firstComma);
        string remainingData = data.substr(firstComma + 1);

    if (type == "CAB") {
        size_t pos[7]; // bookingId, customerId, bookingDate, vehicleId, pickup, drop, distance, totalCost
        pos[0] = remainingData.find(',');
        pos[1] = remainingData.find(',', pos[0] + 1);
        pos[2] = remainingData.find(',', pos[1] + 1);
        pos[3] = remainingData.find(',', pos[2] + 1);
        pos[4] = remainingData.find(',', pos[3] + 1);
        pos[5] = remainingData.find(',', pos[4] + 1);
        pos[6] = remainingData.find(',', pos[5] + 1);

        if (pos[0] != string::npos && pos[1] != string::npos && pos[2] != string::npos &&
            pos[3] != string::npos && pos[4] != string::npos && pos[5] != string::npos &&
            pos[6] != string::npos) {
            
            string bookingId = remainingData.substr(0, pos[0]);
            string customerId = remainingData.substr(pos[0] + 1, pos[1] - pos[0] - 1);
            string bookingDate = remainingData.substr(pos[1] + 1, pos[2] - pos[1] - 1);
            string vehicleId = remainingData.substr(pos[2] + 1, pos[3] - pos[2] - 1);
            string pickupLocation = remainingData.substr(pos[3] + 1, pos[4] - pos[3] - 1);
            string dropLocation = remainingData.substr(pos[4] + 1, pos[5] - pos[4] - 1);
            double distance = stod(remainingData.substr(pos[5] + 1, pos[6] - pos[5] - 1));
            double totalCost = stod(remainingData.substr(pos[6] + 1));

            Vehicle* vehicle = agency->findVehicleById(vehicleId);
            if (vehicle) {
                vehicle->setAvailability(false); // Mark as booked
                return new CabBooking(bookingId, customerId, bookingDate, vehicle, pickupLocation, dropLocation, distance, totalCost);
            }
        }
    } else if (type == "HOTEL") {
        size_t pos[7]; // bookingId, customerId, bookingDate, hotelId, roomId, packageId, numberOfNights, totalCost
        pos[0] = remainingData.find(',');
        pos[1] = remainingData.find(',', pos[0] + 1);
        pos[2] = remainingData.find(',', pos[1] + 1);
        pos[3] = remainingData.find(',', pos[2] + 1);
        pos[4] = remainingData.find(',', pos[3] + 1);
        pos[5] = remainingData.find(',', pos[4] + 1);
        pos[6] = remainingData.find(',', pos[5] + 1);

        if (pos[0] != string::npos && pos[1] != string::npos && pos[2] != string::npos &&
            pos[3] != string::npos && pos[4] != string::npos && pos[5] != string::npos &&
            pos[6] != string::npos) {
            
            string bookingId = remainingData.substr(0, pos[0]);
            string customerId = remainingData.substr(pos[0] + 1, pos[1] - pos[0] - 1);
            string bookingDate = remainingData.substr(pos[1] + 1, pos[2] - pos[1] - 1);
            string hotelId = remainingData.substr(pos[2] + 1, pos[3] - pos[2] - 1);
            string roomId = remainingData.substr(pos[3] + 1, pos[4] - pos[3] - 1);
            string packageId = remainingData.substr(pos[4] + 1, pos[5] - pos[4] - 1);
            int numberOfNights = stoi(remainingData.substr(pos[5] + 1, pos[6] - pos[5] - 1));
            double totalCost = stod(remainingData.substr(pos[6] + 1));

            Hotel* hotel = agency->findHotel(hotelId);
            Room* room = nullptr;
            if (hotel && roomId != "None") {
                room = hotel->findRoomById(roomId);
            }
            HotelPackage* package = nullptr;
            if (hotel && packageId != "None") {
                package = hotel->findPackage(packageId);
            }

            if (hotel && (room || package)) {
                if (room) room->setBookingStatus(true); // Mark as booked
                return new HotelBooking(bookingId, customerId, bookingDate, hotel, room, package, numberOfNights, totalCost);
            }
        }
    }
    return nullptr;
    }
```

