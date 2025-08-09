This page outlines the basics of WebUSB, and how I set it up and tested for basic functionality. It also outlines my conclusion, which was presented on the [Architecture](architecture.md) page.

WebUSB is an API that allows certain compatible microcontrollers to be able to communicate to a web browser. This would open up the potential for users to be able to interact with the Arduino using an HTML file. 

This relates to the project, as it is possible to be able to have a videofeed and have the user see what is going on and allow for a bit of manual control.

This guide has some demos as well as code that can be ran to ensure that WebUSB is working as expected. The following is an adapted version of the tutorials presented in [this video](https://www.youtube.com/watch?v=G8AUgwZCe_Q). For more information about the code and details about WebUSB, watch the linked video. 

## **Outline of Demos**

1. USB Recognition
2. Microcontroller to Web Browser Communication
3. Web Browser to Microcontroller Communication

### **USB Recognition**
The goal of this demo is to create a basic website that will allow the user to plug in a USB device and obtain important details to the console. This demo does not require the use of the WebUSB library, and no code needs to be uploaded to the microcontroller. This means that ANY device, regardless of whether it supports WebUSB or not, can run this test successfully, as this relies on the browser picking up the fact that a USB was plugged in rather than communicating with it.

``` html title="USB_recog.html" linenums="1"
<a href="#" id="click">Connect to USB Device</a>

<script>
click.onclick = function() {
    navigator.usb.requestDevice( {filters: [
        {'vendorId': 0x2341}
    ]})
    .then(device => {
        console.log(device)
        console.log("Product Name: " + device.productName.toString(16))
        console.log("Product ID: " + device.productId.toString(16))
        console.log("Vendor ID: "+ device.vendorId.toString(16))
    })
    .catch(error => {
        console.log(error)
    })
}
</script>
```

Images below show output of html code and how it works when an Arduino Leonardo is connected:

![HTML Output](./assets/Screenshot%202024-07-23%20200054.png)

![HTML Output with Serial](./assets/Screenshot%202024-07-23%20200223.png)

Note that no code is needed for the Arduino.

### **Microcontroller to Web Browser Communication**
The goal of this demo is to be able to send data from the microcontroller and the website. While this demo will work in this particular way, it sets up the backbone for establishing a 2 way communication between the microcontroller and the website.

HTML Code:

``` html title="Leo_to_Web.html" linenums="1"
<!DOCTYPE html>
<html>
    <head>
        <title>USB Communication Test</title>
    </head>
    <body>
        <a href="#" id="click">Listen to Leo</a>
        <button id="connect">Connect</button>
        
        <script src="serial.js"></script>
        
        <script>
            document.getElementById('click').onclick = function() {
                navigator.usb.requestDevice({ filters: [{ 'vendorId': 0x2341 }] })
                    .then(device => {
                        console.log(device);
                        console.log("Product Name: " + device.productName);
                        console.log("Product ID: " + device.productId.toString(16));
                        console.log("Vendor ID: " + device.vendorId.toString(16));
                        console.log("Device Found! Awaiting messages...");
                    })
                    .catch(error => {
                        console.log(error);
                    });
            };

            var port;
            var connectButton = document.getElementById('connect');
            var textDecoder = new TextDecoder();
            var textEncoder = new TextEncoder();

            connectButton.addEventListener('click', function() {
              if (port) {
                    // If port is already connected, disconnect it
                    connectButton.textContent = 'Connect';
                    port.disconnect();
                    port = null;
                    console.log('Device is disconnected.');
                } else {
                    // If there is no port, then connect to a new port
                    serial.requestPort().then(selectedPort => {
                        port = selectedPort;
                        port.connect().then(() => {
                            console.log('Device is connected to Product ID: ' + port.device_.productId.toString(16) + ' and Vendor ID: ' + port.device_.vendorId.toString(16));

                            connectButton.textContent = 'Disconnect';
                            port.onReceive = data => {
                                console.log(textDecoder.decode(data));
                            };
                            port.onReceiveError = error => {
                                console.log('Receive error: ' + error);
                            };
                        }, error => {
                            console.log('Connection error: ' + error);
                        });
                    }).catch(error => {
                        console.log('Connection error: ' + error);
                    });
                }
            });

            serial.requestPort = function() {
                const filters = [
                    { 'vendorId': 0x2341 }
                ];
                return navigator.usb.requestDevice({ 'filters': filters }).then(
                    device => new serial.Port(device)
                );
            };
        </script>
    </body>
</html>
```

Arduino Code:

``` cpp title="Leo2.ino" linenums="1"
#include <WebUSB.h>

// Modified from example https://webusb.github.io/arduino/demos/console
WebUSB WebUSBSerial(1, "webusb.github.io/arduino/demos/console");
int c = 0;

void setup() {
  WebUSBSerial.begin(9600);
  while (!WebUSBSerial) {}
  delay(100);
}

void loop() {
  c = c + 1;
  if (WebUSBSerial){
    WebUSBSerial.println(c);
    WebUSBSerial.flush();    
  } else {
    c = 0;
  }

  delay(1000);
}
```

Serial.js code:

``` js title="serial.js" linenums="1"
var serial = {};

(function() {
  'use strict';

  serial.getPorts = function() {
    return navigator.usb.getDevices().then(devices => {
      return devices.map(device => new serial.Port(device));
    });
  };

  serial.requestPort = function() {
    const filters = [
      { 'vendorId': 0x2341, 'productId': 0x8036 }, // Arduino Leonardo
      { 'vendorId': 0x2341, 'productId': 0x8037 }, // Arduino Micro
      { 'vendorId': 0x2341, 'productId': 0x804d }, // Arduino/Genuino Zero
      { 'vendorId': 0x2341, 'productId': 0x804e }, // Arduino/Genuino MKR1000
      { 'vendorId': 0x2341, 'productId': 0x804f }, // Arduino MKRZERO
      { 'vendorId': 0x2341, 'productId': 0x8050 }, // Arduino MKR FOX 1200
      { 'vendorId': 0x2341, 'productId': 0x8052 }, // Arduino MKR GSM 1400
      { 'vendorId': 0x2341, 'productId': 0x8053 }, // Arduino MKR WAN 1300
      { 'vendorId': 0x2341, 'productId': 0x8054 }, // Arduino MKR WiFi 1010
      { 'vendorId': 0x2341, 'productId': 0x8055 }, // Arduino MKR NB 1500
      { 'vendorId': 0x2341, 'productId': 0x8056 }, // Arduino MKR Vidor 4000
      { 'vendorId': 0x2341, 'productId': 0x8057 }, // Arduino NANO 33 IoT
      { 'vendorId': 0x239A }, // Adafruit Boards!
    ];
    return navigator.usb.requestDevice({ 'filters': filters }).then(
      device => new serial.Port(device)
    );
  }

  serial.Port = function(device) {
    this.device_ = device;
    this.interfaceNumber_ = 2;  // original interface number of WebUSB Arduino demo
    this.endpointIn_ = 5;       // original in endpoint ID of WebUSB Arduino demo
    this.endpointOut_ = 4;      // original out endpoint ID of WebUSB Arduino demo
  };

  serial.Port.prototype.connect = function() {
    let readLoop = () => {
      this.device_.transferIn(this.endpointIn_, 64).then(result => {
        this.onReceive(result.data);
        readLoop();
      }, error => {
        this.onReceiveError(error);
      });
    };

    return this.device_.open()
        .then(() => {
          if (this.device_.configuration === null) {
            return this.device_.selectConfiguration(1);
          }
        })
        .then(() => {
          var configurationInterfaces = this.device_.configuration.interfaces;
          configurationInterfaces.forEach(element => {
            element.alternates.forEach(elementalt => {
              if (elementalt.interfaceClass==0xff) {
                this.interfaceNumber_ = element.interfaceNumber;
                elementalt.endpoints.forEach(elementendpoint => {
                  if (elementendpoint.direction == "out") {
                    this.endpointOut_ = elementendpoint.endpointNumber;
                  }
                  if (elementendpoint.direction=="in") {
                    this.endpointIn_ =elementendpoint.endpointNumber;
                  }
                })
              }
            })
          })
        })
        .then(() => this.device_.claimInterface(this.interfaceNumber_))
        .then(() => this.device_.selectAlternateInterface(this.interfaceNumber_, 0))
        // The vendor-specific interface provided by a device using this
        // Arduino library is a copy of the normal Arduino USB CDC-ACM
        // interface implementation and so reuses some requests defined by
        // that specification. This request sets the DTR (data terminal
        // ready) signal high to indicate to the device that the host is
        // ready to send and receive data.
        .then(() => this.device_.controlTransferOut({
            'requestType': 'class',
            'recipient': 'interface',
            'request': 0x22,
            'value': 0x01,
            'index': this.interfaceNumber_}))
        .then(() => {
          readLoop();
        });
  };

  serial.Port.prototype.disconnect = function() {
    // This request sets the DTR (data terminal ready) signal low to
    // indicate to the device that the host has disconnected.
    return this.device_.controlTransferOut({
            'requestType': 'class',
            'recipient': 'interface',
            'request': 0x22,
            'value': 0x00,
            'index': this.interfaceNumber_})
        .then(() => this.device_.close());
  };

  serial.Port.prototype.send = function(data) {
    return this.device_.transferOut(this.endpointOut_, data);
  };
})();
```

Images below show output and how it works when an Arduino Leonardo is connected:

![Output](./assets/Screenshot%202024-07-24%20211718.png)

![Output with Serial](./assets/Screenshot%202024-07-24%20211750.png)

### **Web Browser to Microcontroller Communication &rarr; Blinky**

The goal of this demo is to be able to control a microcontroller by having the user interface with a website. For the project, it would open up many possibilities for human interaction as the sorter is working through sorting. As of today, the only thing that makes sense would be to have a manual override in case something goes wrong, and maybe a stop and start function. An HTML would also be able to display possible metrics that are coming in from the microcontroller. This would include how many items were sorted, a running count of each type of item sorted, etc. Metrics are something that I would consider desirable and not required. The biggest use case of the website would be to train the ML model that would be used to then sort. This would be something very similar to what is implemented on Teachable Machine by Google. 

This demo uses an Arduino Leonardo and an HTML file to turn on and off the builtin LED. 

HTML Code:

``` html title="Leo_to_Web_BLINKY.html" linenums="1"
<a href="#" id="connect">Connect</a>

<p>
    <button id="on">LED ON</button>
    <button id="off">LED OFF</button>
</p>

<script src="serial.js"></script>

<script>
    serial.requestPort = function() {
        const filters = [
            {'vendorId': 0x2341}
        ];
        return navigator.usb.requestDevice({'filters': filters}).then(
            device => new serial.Port(device)
        );
    }

    var port;
    var connectButton = document.getElementById('connect');
    var textDecoder = new TextDecoder();
    var textEncoder = new TextEncoder();

    document.querySelector('#on').addEventListener('click', function() {
        if (port !== undefined) {
            port.send(textEncoder.encode('H')).catch(error => {
                console.log("Error: " + error)
            })
            console.log("HTML: Turning on the LED!")
        }
    })

    document.querySelector('#off').addEventListener('click', function() {
        if (port !== undefined) {
            port.send(textEncoder.encode('L')).catch(error => {
                console.log("Error: " + error)
            })
            console.log("HTML: Turning off the LED!")
        }
    })
    

    connectButton.addEventListener('click', function() {
              if (port) {
                    // If port is already connected, disconnect it
                    connectButton.textContent = 'Connect';
                    port.disconnect();
                    port = null;
                    console.log('Device is disconnected.');
                } else {
                    // If there is no port, then connect to a new port
                    serial.requestPort().then(selectedPort => {
                        port = selectedPort;
                        port.connect().then(() => {
                            console.log('Device is connected to Product ID: ' + port.device_.productId.toString(16) + ' and Vendor ID: ' + port.device_.vendorId.toString(16));

                            connectButton.textContent = 'Disconnect';
                            port.onReceive = data => {
                                console.log(textDecoder.decode(data));
                            };
                            port.onReceiveError = error => {
                                console.log('Receive error: ' + error);
                            };
                        }, error => {
                            console.log('Connection error: ' + error);
                        });
                    }).catch(error => {
                        console.log('Connection error: ' + error);
                    });
                }
            });
</script>
```

Arduino Code:

``` cpp title="Blinky.ino" linenums="1"
#include <WebUSB.h>
WebUSB WebUSBSerial(1, "webusb.github.io/arduino/demos/console");

const int ledPin = 13;

void setup() {
  WebUSBSerial.begin(9600);
  while (!WebUSBSerial) {}
  delay(100);

  SerialUSB.begin(9600);
  delay(100);

  WebUSBSerial.write("Starting blinky!");
  WebUSBSerial.flush();
  SerialUSB.println("Starting...");

  pinMode(ledPin, OUTPUT);
}

void loop() {
  if (WebUSBSerial && WebUSBSerial.available()) {
    char byte = WebUSBSerial.read();  // Read the incoming byte as a character
    WebUSBSerial.write(byte);  // Echo the received byte

    if (byte == 'H') {  // Compare the byte with the character 'H'
      SerialUSB.println("Received H!");
      WebUSBSerial.write("\nTurning on LED");
      digitalWrite(ledPin, HIGH);  // Turn the LED on
    } else if (byte == 'L') {  // Compare the byte with the character 'L'
      SerialUSB.println("Received L!");
      WebUSBSerial.write("\nTurning off LED");
      digitalWrite(ledPin, LOW);  // Turn the LED off
    }

    WebUSBSerial.flush();
  }
}
```

Serial.js code (same as before, provided for reference):

``` js title="serial.js" linenums="1"
var serial = {};

(function() {
  'use strict';

  serial.getPorts = function() {
    return navigator.usb.getDevices().then(devices => {
      return devices.map(device => new serial.Port(device));
    });
  };

  serial.requestPort = function() {
    const filters = [
      { 'vendorId': 0x2341, 'productId': 0x8036 }, // Arduino Leonardo
      { 'vendorId': 0x2341, 'productId': 0x8037 }, // Arduino Micro
      { 'vendorId': 0x2341, 'productId': 0x804d }, // Arduino/Genuino Zero
      { 'vendorId': 0x2341, 'productId': 0x804e }, // Arduino/Genuino MKR1000
      { 'vendorId': 0x2341, 'productId': 0x804f }, // Arduino MKRZERO
      { 'vendorId': 0x2341, 'productId': 0x8050 }, // Arduino MKR FOX 1200
      { 'vendorId': 0x2341, 'productId': 0x8052 }, // Arduino MKR GSM 1400
      { 'vendorId': 0x2341, 'productId': 0x8053 }, // Arduino MKR WAN 1300
      { 'vendorId': 0x2341, 'productId': 0x8054 }, // Arduino MKR WiFi 1010
      { 'vendorId': 0x2341, 'productId': 0x8055 }, // Arduino MKR NB 1500
      { 'vendorId': 0x2341, 'productId': 0x8056 }, // Arduino MKR Vidor 4000
      { 'vendorId': 0x2341, 'productId': 0x8057 }, // Arduino NANO 33 IoT
      { 'vendorId': 0x239A }, // Adafruit Boards!
    ];
    return navigator.usb.requestDevice({ 'filters': filters }).then(
      device => new serial.Port(device)
    );
  }

  serial.Port = function(device) {
    this.device_ = device;
    this.interfaceNumber_ = 2;  // original interface number of WebUSB Arduino demo
    this.endpointIn_ = 5;       // original in endpoint ID of WebUSB Arduino demo
    this.endpointOut_ = 4;      // original out endpoint ID of WebUSB Arduino demo
  };

  serial.Port.prototype.connect = function() {
    let readLoop = () => {
      this.device_.transferIn(this.endpointIn_, 64).then(result => {
        this.onReceive(result.data);
        readLoop();
      }, error => {
        this.onReceiveError(error);
      });
    };

    return this.device_.open()
        .then(() => {
          if (this.device_.configuration === null) {
            return this.device_.selectConfiguration(1);
          }
        })
        .then(() => {
          var configurationInterfaces = this.device_.configuration.interfaces;
          configurationInterfaces.forEach(element => {
            element.alternates.forEach(elementalt => {
              if (elementalt.interfaceClass==0xff) {
                this.interfaceNumber_ = element.interfaceNumber;
                elementalt.endpoints.forEach(elementendpoint => {
                  if (elementendpoint.direction == "out") {
                    this.endpointOut_ = elementendpoint.endpointNumber;
                  }
                  if (elementendpoint.direction=="in") {
                    this.endpointIn_ =elementendpoint.endpointNumber;
                  }
                })
              }
            })
          })
        })
        .then(() => this.device_.claimInterface(this.interfaceNumber_))
        .then(() => this.device_.selectAlternateInterface(this.interfaceNumber_, 0))
        // The vendor-specific interface provided by a device using this
        // Arduino library is a copy of the normal Arduino USB CDC-ACM
        // interface implementation and so reuses some requests defined by
        // that specification. This request sets the DTR (data terminal
        // ready) signal high to indicate to the device that the host is
        // ready to send and receive data.
        .then(() => this.device_.controlTransferOut({
            'requestType': 'class',
            'recipient': 'interface',
            'request': 0x22,
            'value': 0x01,
            'index': this.interfaceNumber_}))
        .then(() => {
          readLoop();
        });
  };

  serial.Port.prototype.disconnect = function() {
    // This request sets the DTR (data terminal ready) signal low to
    // indicate to the device that the host has disconnected.
    return this.device_.controlTransferOut({
            'requestType': 'class',
            'recipient': 'interface',
            'request': 0x22,
            'value': 0x00,
            'index': this.interfaceNumber_})
        .then(() => this.device_.close());
  };

  serial.Port.prototype.send = function(data) {
    return this.device_.transferOut(this.endpointOut_, data);
  };
})();
```

Images below show output and how it works:

![Output](./assets/Screenshot%202024-07-23%20202604.png)

![Output with Serial](./assets/Screenshot%202024-07-23%20202706.png)

## Conclusion

WebUSB is not ideal (from a simplicity point of view) due to the communication protocol that needs to be put in place. With any webpage, a client and server is needed. However, this communication would have to be extended to the Arduino as well. With a large amount of communication happening between the user, client, server, and Arduino, there is bound to be a loss of data somewhere due to data being sent and recieved from multiple sources, essentially overloading the system. While the ideal scenario would be to run something like this, my prototype will not be using a web-based system to reduce the complexity of communicating information.