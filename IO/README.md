# Guidelines to use I/O on Ultra96v2 PYNQ including UART GPIO and I2C

## 1. GPIO

TO use GPIO and Ultra96v2, the first thing to note is that there are two types of GPIO. PS GPIO and PL GPIO. 

PS GPIO runs on the Processing system (PS) and thus is accessable without an overlay.

PL GPIO runs on the Programmable Logic (PL) and thus would require an overlay to utilize. Instructions to build the overlay can be found on the Vivado tab.

TO utilize PS GPIO on PYNQ, it is similar to how you would run it on a linux embedded platform like Raspberry Pi. 

PYNQ provides some drivers such that you can use python code to drive the PS GPIO.

First import the libraries for pynq and some time libraries

```python
from pynq import GPIO
import time
```

This is so that you can add a delay before turning on and off the GPIO such that you can manipulate external electronic devices.

Then get the base number of the GPIO using

```python
gpio_base = GPIO.get_gpio_base()
```

By calling the base, we can easaily define what pins we would like to use later on based on their number instead of having to calculate the number which maps to the coressponding pin.

Now we have to decide which pin number to use as well as the direction of the pin (whether it is an input or output). This can be done using the following code

```python
gpio1 = GPIO(gpio_base+36, 'out') # write an output pin at pin 36
gpio2 = GPIO(gpio_base+37, 'in')
#write an input pin at pin 37
```

Once this is done we can decide to read or write on the coresponding pin. 

```python
gpio1.write(1)
# writes a high signal out of 1.8V
gpio1.write(0)
#writes a low signal out of 0V
gpio1.read()
#reads the voltage level at the pin when run if 1.8V will return 1 else if 0V will return 0.
#Has a threshold value for 1 and 0
```

Note you cannot write on a read pin or read on a write pin. You have to release the pin using the below before changing the direction of the pin

```python
gpio1.release()
# gpio1 should be replaced with the variable you assigned your pin name
```

Now that PS GPIO has been covered, to use them to create a PWM signal, an easy method would be to use time.sleep() and a while loop. For example

```python
while(True):
    gpio1.write(1)
    time.sleep(0.1)
    gpio1.write(0)
    time.sleep(0.9)
    # provides a pulse every 1 second
    # 10% duty cycle with 1Hz
    # You can adjust the ratio of time.sleep() to increase the duty cycle and the reduce the time.sleep() duration to increase its frequency based on what you would like it to be
```

Just note since ultra96v2 low speed expansion header only supports 1.8V input and output so a level shifter may be required for higher voltage levels.

For more details on which Pins are in the PS please check the official documentation. It should be the MIO pins 36,37,39,40,44,45.

## 2.PL GPIO

Now that we know how to use basic GPIO, let's utilize the PL GPIO. Although AXI GPIO can also be used, in this guide it will not be covered as we can directly map the PL GPIO to the PS without using the AXI block. There are many guides explaining how to use AXI GPIO library from PYNQ.

First we will have to load the overlay. So make sure to add the tcl hwh and bit files to the correct directory before starting.

Then we can load the overlay by calling the Overlay helper from pynq library. Here's an example

```python
from pynq import Overlay
ol = Overlay("pl.bit")
# the function helps us to load the pl overlay)
```

Like PS, the PL gpio also requires a pin number but unlike PS we will directly obtain the pin number without using a base function. 

```python
gpio0 = GPIO.get_gpio_pin(gpio_user_index= 6)
# the function GPIO.get_gpio_pin(gpio_user_index=6) will helps us get the pin number for pin IO6 from the PL)
```

After that the process is the same as PS GPIO. First you have to define the pin direction. Then the read and write. Here is an example.

```python
gpio0 = GPIO.get_gpio_pin(gpio_user_index= 6)
# define pin direction
gpio0 = GPIO(gpio0, 'out')
gpio0.write(1)
time.sleep(1)
gpio0.write(0)

#release the gpio so that we can read
gpio0.release()

#recall the pin number as the variable got overwrote you can assign a different variable if you will reuse it in the same block.

gpio0 = GPIO.get_gpio_pin(gpio_user_index= 6)

#set direction as in
gpio0 = GPIO(gpio0, 'in')
gpio0.read()
```

That is all for GPIO do experiment with all the options to figure out how many gpio you really need thus to allocate some pins for uart i2c etc.

## 3. UART

Follow the steps in Vivado to make the Overlay for UART.

Once the overlay has been made, import it to PYNQ.

Then make sure to import the Overlay function from the pynq library like for PL GPIO

Also import tine

```python
from pynq import Overlay
from time import sleep, time
```

Then load the overlay and check what's inside the overlay by

```python
ol = Overlay('pl_gpio.bit')
print(ol.ip_dict.keys()) # prints out all the available ip 
```

To access the uart0, make sure to first find the correct dict key then

```python
# Access the AXI UART IP
uart = ol.axi_uart16550_0
```

The Uart IP has been loaded onto the uart variable

To write a word (Ascii) to the UART, we will define a write function.

```python
def write(word): # for ascii
    for char in word:
        byte = ord(char)  # Convert ASCII character to byte
        uart.write(0x0, byte)  # Write byte 
        sleep(0.00075)  # Wait for register to be ready before writing the next byte
        print(hex(byte))

# Example usage
write("Hello, World!")
```

Try connecting the UART to any serial device on your PC to test if it works before proceeding, otherwise you may need to check if the level shifters are working.

Next, to write raw bytes to the UART, we can define another function

```python
# write to uart
def write(word): # for hex/
    # Convert the hex string to bytes
    byte_list = bytes.fromhex(word)
    
    for byte in byte_list:
        uart.write(0x0, byte)# write byte 
        sleep(0.00075) # wait for register to be ready before writing next byte
        print(hex(byte))

# Example usage
write("1234567890")
```

The output will be 

```
0x12
0x34
0x56
0x78
0x90
```

To read drom the UART, we can define a read function to help us read

```python
# used to post process uart outputs
def uartf(my_list):
    consecutive_count = 0
    consecutive_element = None
    new_list = []

    for element in my_list:
        if consecutive_count == 0:
            consecutive_element = element
            consecutive_count += 1
        elif element == consecutive_element:
            consecutive_count += 1
        else:
            if consecutive_count >= 100:
                for _ in range(consecutive_count // 100):
                    new_list.append(consecutive_element)
                    print(new_list)
            consecutive_element = element
            consecutive_count = 1

    if consecutive_count >= 100:
        for _ in range(consecutive_count // 100):
            new_list.append(consecutive_element)
    return new_list
    print(new_list)
```

This helps to post process the uart read list.

Them we can keep reading from the uart until the end bit has reached and store it in a list.

```python
test = []
while (True):
    red = uart.read(0)
    if(red==97):
        continue
    test.append(red)
    if red == 13 or red==10:
        break
print(uartf(test))
```

The output is

```
[1, 2, 141, 163, 226, 255]
```

## I2C usage

To have the tools to check the i2c bus, go to the linux shell and install i2c-tools. This will allow us to check the bus lanes of ultra96v2 to help determine which bus we are required to connect to to use i2c.

```bash
sudo apt install i2c-tools
```

We will also require smbus library in python in order to read and write to the i2c

To install it you can either go to the linux shell and install it like this

```bash
sudo apt install python3-smbus
```

or

go to jupyternotebook and run

```python
!pip install smbus
```

Through the linux shell, determine which i2c bus is connected to the low speed expansion header through the following

```bash
i2cdetect -1
```

This will show you all the i2c bus available. Determine the i2c bus number which has a chan_id 0 for scl sda 0 or  chan id 1 for scl1 sda1.

Here is a sample picture


Also, once you have determined the correct bus, please find out the address of your i2c device by running the following in the linux shell.

```bash
i2cdetect -r -y 2
```

Note replace 2 with the correct i2c bus number you are using.

Here is a sample photo of the output. Note your address number will differ based on your i2c device.

To read and write the i2c device, you can look for a python based driver online which uses smbus for your particular device and it should work. Here's an example for the TCS34725 color sensor.

```python
import smbus
import time

# TCS34725 I2C address
TCS34725_ADDRESS = 0x29

# TCS34725 registers
TCS34725_COMMAND_BIT = 0x80
TCS34725_ENABLE = 0x00
TCS34725_ATIME = 0x01
TCS34725_CONTROL = 0x0F
TCS34725_CDATAL = 0x14
TCS34725_CDATAH = 0x15
TCS34725_RDATAL = 0x16
TCS34725_RDATAH = 0x17
TCS34725_GDATAL = 0x18
TCS34725_GDATAH = 0x19
TCS34725_BDATAL = 0x1A
TCS34725_BDATAH = 0x1B

# Initialize I2C bus (change the bus number if necessary)
bus = smbus.SMBus(3)

def write_byte(reg, value):
    bus.write_byte_data(TCS34725_ADDRESS, reg | TCS34725_COMMAND_BIT, value)

def read_word(reg):
    data_low = bus.read_byte_data(TCS34725_ADDRESS, reg | TCS34725_COMMAND_BIT)
    data_high = bus.read_byte_data(TCS34725_ADDRESS, reg+1 | TCS34725_COMMAND_BIT)
    return (data_high << 8) | data_low

def read_data():
    clear = read_word(TCS34725_CDATAL)
    red = read_word(TCS34725_RDATAL)
    green = read_word(TCS34725_GDATAL)
    blue = read_word(TCS34725_BDATAL)
    return clear, red, green, blue

def enable():
    # Enable the sensor
    write_byte(TCS34725_ENABLE, 0x03)  # Power ON and enable RGBC

def disable():
    # Disable the sensor (standby mode)
    write_byte(TCS34725_ENABLE, 0x00)

def set_integration_time(time):
    # Set the integration time (time should be a value between 0 and 255)
    write_byte(TCS34725_ATIME, time)

def set_gain(gain):
    # Set the gain (gain should be a value between 0 and 3)
    write_byte(TCS34725_CONTROL, gain << 4)

def normalize(value, minimum, maximum):
    # Normalize a value to the range of 0 to 255
    normalized = int((value - minimum) / (maximum - minimum) * 255)
    return max(0, min(normalized, 255))

# Example usage
enable()
set_integration_time(100)  # Set integration time to 100ms
set_gain(1)  # Set gain to 4x

while True:
    clear, red, green, blue = read_data()
    
    # Normalize clear and RGB values
    clear_normalized = normalize(clear, 0, 4096)
    red_normalized = normalize(red, 0, 4096)
    green_normalized = normalize(green, 0, 4096)
    blue_normalized = normalize(blue, 0, 4096)
    
    print("Clear: {}, Red: {}, Green: {}, Blue: {}".format(clear_normalized, red_normalized, green_normalized, blue_normalized))
    time.sleep(1)
```

That is all for this notebook