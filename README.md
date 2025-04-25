# Puppy Bank Documentation

## Introduction

### Project Description
The **"Puppy Bank"** is an interactive, playful savings system where users can deposit coins into a physical dog-shaped bank. The dog in the user interface reacts joyfully to the deposit and shows floating money as a visual reward. This project combines physical interaction with digital feedback, aimed at making the process of saving fun and engaging for children.

### Context and Use Case
This prototype is designed for children and anyone who enjoys a playful approach to saving money. The project encourages saving by integrating a joyful interface where users can interact with a digital pet. The prototype can be used in homes, schools, or even in museums for educational purposes, teaching children about saving and rewards.

### Hardware
- **Servo motor**: Controls the movement of the dog’s tail or any physical movement in the prototype.
- **Light sensor**: Detects the light level, which is used to trigger different actions in the interface based on environmental conditions.
- **Microcontroller (e.g., ATOM board)**: The brain of the system, coordinating the hardware and controlling the interaction.
- **NeoPixel LEDs**: Provides the colorful floating money effect in the interface.
- **Button (optional)**: Used to reset or start the interaction.

### Concept Sketches
![image](https://github.com/user-attachments/assets/5e9b548f-4f93-4b7e-9a69-04ef01d63393)
![image](https://github.com/user-attachments/assets/f71e5083-c1d6-40fa-8399-acbed5ebaa1d)


### Photo(s)
![image](https://github.com/user-attachments/assets/2090f8a1-86e5-4037-8a9c-f7f506fee350)
![image](https://github.com/user-attachments/assets/ee593bb8-0ba2-4938-a22f-61f0842cb3f3)
![image](https://github.com/user-attachments/assets/5618538f-f79d-484e-966b-6e6ee6b4c3b8)
![image](https://github.com/user-attachments/assets/dab6c339-91a5-470e-a35d-4161c51d39cd)
![image](https://github.com/user-attachments/assets/cf87218a-8568-44bf-9a3b-48b8b359c8e0)
![image](https://github.com/user-attachments/assets/549971d4-db25-4cca-8d7e-038d7d7ba745)


### Schematic or Wiring Diagram
ATOM board Pin 1 → Light sensor
ATOM board Pin 2 → Servo motor
ATOM board Pin 3 → NeoPixel LEDs

### Software
![image](https://github.com/user-attachments/assets/51cebbf3-8190-4e02-95e3-db3e0ff6bdf5)
![image](https://github.com/user-attachments/assets/0120111b-2677-4088-bbec-63624ee2f315)

**ProtoPie Interface**:  
ProtoPie was used to create the interactive digital interface where the dog character reacts to the deposited coins. The interface displays animations of the dog wagging its tail, along with floating money as a visual reward. The interface is connected to the hardware and triggers actions such as the dog’s reactions.

- **ProtoPie Setup**: 
![image](https://github.com/user-attachments/assets/fa0ba272-d437-4588-9b65-6d2cee6ea9a1)
![image](https://github.com/user-attachments/assets/5f6efd41-2f02-4ce1-9f8e-1f88a1ca4119)

- **Interaction Flow**:

![image](https://github.com/user-attachments/assets/413400a8-573c-419f-8e69-1fa61a791e9f)

### Integrations

**ProtoPie Integration**:  
ProtoPie is used to communicate with the hardware and trigger the animations. When a coin is inserted, the hardware detects it and sends a signal to ProtoPie to start the animation.

- The interface is connected to the hardware, so when the coin is detected by the sensor, ProtoPie triggers the animation in the interface, displaying the happy dog and the floating money effect.

### Enclosure / Mechanical Design

The physical design of the Puppy Bank is based on a small snow house with a dog inside. The mechanical design includes:

- **3D printed enclosure**: A small house that houses the dog and all the electronics.
- **Servo mount**: A mount inside the house that holds the servo motor and ensures it moves smoothly when triggered.

### Results  
The final project successfully integrates the interactive puppy character with the physical bank. The puppy reacts to each coin inserted by displaying a happy animation and showing floating money. The interface and hardware work seamlessly, providing a fun and engaging user experience.

#### Photos and Video:  

https://github.com/user-attachments/assets/78d4d37f-b40a-4281-8f69-2bc83fc23913

https://github.com/user-attachments/assets/ef94c665-b276-44e3-9916-d143710dd347

https://github.com/user-attachments/assets/cff3d9fb-c937-4907-840b-c837c7a14e18

---

### Conclusion

### Reflection  
This project was a fun way to combine hardware and software to create an engaging experience. One of the challenges I faced was fine-tuning the interaction between the physical bank and the interface. If I had more time, I would add more complex animations or extend the interaction to include sound effects for the puppy.

### Future Improvements  
If I were to continue developing this project, I would consider adding more complex interactions, such as voice recognition or integrating a larger variety of animations.


### Firmware

### MicroPython Code for Puppy Bank Interaction
```python
from machine import Pin, ADC, PWM
from neopixel import NeoPixel
from time import sleep_ms

# === 光线感应器 ===
adc = ADC(Pin(1))  # Pin for light sensor
adc.atten(ADC.ATTN_11DB)

# === RGB 灯带 ===
NUM_PIXELS = 30
np = NeoPixel(Pin(38), NUM_PIXELS)  # Pin for NeoPixels

# === 360° Servo ===
pwm = PWM(Pin(7), freq=50)  # Pin for servo motor
pwm.duty(75)  # Initial position

# === 控制变量 ===
dark_threshold = 3000  # Light threshold for interaction
coin_count = 0  # Number of coins inserted

def rotate_servo():
    pwm.duty(90)  # Move servo to simulate happy reaction
    sleep_ms(2000)
    pwm.duty(75)  # Reset position

def set_led_color(r, g, b):
    for i in range(NUM_PIXELS):
        np[i] = (r, g, b)
    np.write()

# === 主循环 ===
while True:
    light_val = adc.read()  # Read the sensor value
    print(f"Light Value: {light_val}, Coins Inserted: {coin_count}")

    if light_val > dark_threshold:
        coin_count += 1
        print(f"Coin inserted! Current count: {coin_count}")
        rotate_servo()  # Simulate the puppy's reaction

        if coin_count >= 3:
            set_led_color(0, 255, 0)  # Green light for a happy puppy
        else:
            set_led_color(255, 0, 0)  # Red light for awaiting coin

    sleep_ms(100)  # Delay before checking the sensor again


