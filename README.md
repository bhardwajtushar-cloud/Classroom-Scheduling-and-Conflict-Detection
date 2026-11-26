# Classroom-Scheduling-and-Conflict-Detection
rooms = ["R101", "R102", "Lab-1"]
valid_days = ["Mon", "Tue", "Wed", "Thu", "Fri"]
bookings = []
def check_conflict(day, room, teacher, start, end):
    for b in bookings:
        if b["day"] == day and b["start"] < end and start < b["end"]:
            if b["room"] == room:
                return "Room " + room + " is already booked for " + b["course"] + " at that time."
            if b["teacher"].lower() == teacher.lower():
                return teacher + " is already teaching " + b["course"] + " at that time."
    return None
def add_booking():
    course = input("Enter course name: ")
    teacher = input("Enter teacher name: ")
    room = input("Enter room (R101/R102/Lab-1): ")
    day = input("Enter day (Mon-Fri): ")
    start = int(input("Enter start hour (9-17): "))
    end = int(input("Enter end hour (10-18): "))
    if room not in rooms:
        print("Invalid room.")
        return
    if day not in valid_days:
        print("Invalid day.")
        return
    if start >= end or start < 9 or end > 18:
        print("Invalid time slot.")
        return
    problem = check_conflict(day, room, teacher, start, end)
    if problem:
        print("CONFLICT:", problem)
    else:
        bookings.append({"course": course, "teacher": teacher, "room": room,
                         "day": day, "start": start, "end": end})
        print("Class scheduled successfully.")
def view_bookings():
    if len(bookings) == 0:
        print("No classes scheduled.")
        return
    for i, b in enumerate(bookings, 1):
        print(str(i) + ". " + b["course"] + " | " + b["teacher"] + " | " + b["room"] + " | " + b["day"] + " | " + str(b["start"]) + ":00-" + str(b["end"]) + ":00")
def find_free_rooms():
    day = input("Enter day (Mon-Fri): ")
    start = int(input("Enter start hour: "))
    end = int(input("Enter end hour: "))
    free = []
    for room in rooms:
        busy = False
        for b in bookings:
            if b["room"] == room and b["day"] == day and b["start"] < end and start < b["end"]:
                busy = True
        if not busy:
            free.append(room)
    if len(free) == 0:
        print("No free rooms in this slot.")
    else:
        print("Free rooms:", ", ".join(free))
def search_teacher():
    name = input("Enter teacher name: ")
    found = False
    for b in bookings:
        if b["teacher"].lower() == name.lower():
            print(b["course"] + " | " + b["room"] + " | " + b["day"] + " | " + str(b["start"]) + ":00-" + str(b["end"]) + ":00")
            found = True
    if not found:
        print("No classes found for this teacher.")
def cancel_booking():
    view_bookings()
    if len(bookings) == 0:
        return
    number = int(input("Enter booking number to cancel: "))
    if 1 <= number <= len(bookings):
        removed = bookings.pop(number - 1)
        print("Cancelled:", removed["course"])
    else:
        print("Invalid booking number.")
def main():
    while True:
        print("\n===== CLASSROOM SCHEDULER =====")
        print("1. Schedule a class")
        print("2. View all classes")
        print("3. Find free rooms")
        print("4. Search by teacher")
        print("5. Cancel a class")
        print("6. Exit")
        choice = input("Enter your choice: ")
        if choice == "1":
            add_booking()
        elif choice == "2":
            view_bookings()
        elif choice == "3":
            find_free_rooms()
        elif choice == "4":
            search_teacher()
        elif choice == "5":
            cancel_booking()
        elif choice == "6":
            print("Exiting. Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")
main()
