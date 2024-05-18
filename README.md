# QR-Based-Bus-Ticketing-System
Developed a QR-based bus ticketing system to enhance hygiene and security in public transportation. Utilizing ESP32-S3 Eye, Arduino Uno, and ultrasonic sensors, the system detects approaching passengers and displays a QR code for ticket validation, allowing contactless interaction.

[PYTHON CODE]
C:\Users\KIIT\AppData\Local\Programs\Python\Python37-32\Diyaapp.py
    

[KV FILE]
C:\Users\KIIT\AppData\Local\Programs\Python\Python37-32\Diyaapp.kv


[HARDWARE PYTHON CODE]
C:\Users\KIIT\AppData\Local\Programs\Python\Python37-32\iot.py


import serial
import time

# Connect to Arduino via serial port
ser = serial.Serial('COM3', 9600)  # Replace 'COM6' with your Arduino's serial port

# Wait for Arduino to initialize
time.sleep(2)

# Function to set servo position for a specific pin
def set_servo_position(pin, angle):
    # Send the pin number and angle to Arduino
    command = f"{pin}:{angle}\n"
    ser.write(command.encode())
    print(f"Set servo position: Pin {pin}, Angle {angle}")
    time.sleep(1)

# Example usage
servo_pin = 6  # Specify the pin number your servo is connected to

for i in range(0, 10):
    set_servo_position(servo_pin, -90)  # Set the servo to -90 degrees
    time.sleep(1)
    set_servo_position(servo_pin, 90)   # Set the servo to 90 degrees
    time.sleep(1)

# Close the serial connection
ser.close()
