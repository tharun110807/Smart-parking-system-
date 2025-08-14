from collections import deque

class SmartParkingSystem:
    def __init__(self, total_slots):
        self.total_slots = total_slots
        self.slots = {i: None for i in range(1, total_slots+1)}  # slot_no: vehicle_no
        self.waiting_queue = deque()  # vehicles waiting
    
    def park_vehicle(self, vehicle_no):
        # Check for empty slot
        for slot, vehicle in self.slots.items():
            if vehicle is None:
                self.slots[slot] = vehicle_no
                print(f"Vehicle {vehicle_no} parked in slot {slot}")
                return
        # If full, add to queue
        self.waiting_queue.append(vehicle_no)
        print(f"Parking full! Vehicle {vehicle_no} added to waiting queue")
    
    def remove_vehicle(self, vehicle_no):
        # Search in slots
        for slot, vehicle in self.slots.items():
            if vehicle == vehicle_no:
                self.slots[slot] = None
                print(f"Vehicle {vehicle_no} removed from slot {slot}")
                # Allocate slot to waiting vehicle
                if self.waiting_queue:
                    next_vehicle = self.waiting_queue.popleft()
                    self.slots[slot] = next_vehicle
                    print(f"Vehicle {next_vehicle} from waiting queue parked in slot {slot}")
                return
        print(f"Vehicle {vehicle_no} not found in parking lot")
    
    def display_parking(self):
        print("\n--- Parking Slots ---")
        for slot, vehicle in self.slots.items():
            status = vehicle if vehicle else "Empty"
            print(f"Slot {slot}: {status}")
        print("\n--- Waiting Queue ---")
        if self.waiting_queue:
            print(", ".join(self.waiting_queue))
        else:
            print("No vehicles waiting")
        print("---------------------")


# --- Example Usage ---
parking = SmartParkingSystem(total_slots=3)

parking.park_vehicle("TN10A1234")
parking.park_vehicle("TN45B5678")
parking.park_vehicle("TN66C9999")
parking.park_vehicle("TN22D1111")  # Goes to queue

parking.display_parking()

parking.remove_vehicle("TN45B5678")  # Frees a slot
parking.display_parking()
