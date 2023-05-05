# Vaseem-Ahmed
import paho.mqtt.client as mqtt
import ssl

# MQTT Broker Configuration
mqtt_broker = "iot.example.com"
mqtt_port = 8883
mqtt_username = "username"
mqtt_password = "password"
mqtt_ca_cert = "/path/to/ca.crt"
mqtt_client_cert = "/path/to/client.crt"
mqtt_client_key = "/path/to/client.key"

# MQTT Client Configuration
mqtt_client = mqtt.Client("iot-client")
mqtt_client.tls_set(ca_certs=mqtt_ca_cert,
                    certfile=mqtt_client_cert,
                    keyfile=mqtt_client_key,
                    cert_reqs=ssl.CERT_REQUIRED,
                    tls_version=ssl.PROTOCOL_TLSv1_2)

# Callback function for successful MQTT connection
def on_connect(client, userdata, flags, rc):
    print("Connected with result code " + str(rc))
    # Subscribe to MQTT topics
    client.subscribe("iot/temperature")
    client.subscribe("iot/humidity")

# Callback function for MQTT messages
def on_message(client, userdata, message):
    print("Received message on topic " + message.topic + " with payload " + message.payload)

# Set the callback functions
mqtt_client.on_connect = on_connect
mqtt_client.on_message = on_message

# Connect to MQTT broker
mqtt_client.connect(mqtt_broker, mqtt_port, 60)

# Start the MQTT loop
mqtt_client.loop_forever()
```
