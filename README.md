# RoboNurse
Build a robonurse application 

1. From the central IoT hub, send the medication_delivery request  which includes the room number to which the robot needs to go to deliver the medicine.To initiate the flow,  one needs to press the inject button. This instruction will be published using MQTT protocol and can be available in hiveMQ broker.


2. Robot will consume the request and then start moving and publish all the movement details to the robot_movement topic.
3. Central iot hub will subscribe the topic and update the dashboard with robot movement details


4. At the same time the robot will subscribe to the instruction and search for that particular room. It will capture the image and send it to the central iot hub for validation


5. Central iot hub will subscribe the message with room number and validate the room. If the room number does not match with the expected one(which has been sent at the start), then the robot will keep moving and capture the room image. This activity continues until the image matches with the expected room number.


6. Here the central iot hub leverages the azure cognitive model(custom vision) to predict the room image.


7. Once the robot finds the expected room, it capture the patient image and send it back to the central iot hub. Then the central iot hub sends the patient image  to the azure cognitive model(custom vision) to predict and validate the patient image and publish the response to the message broker.


8. If the captured image is not the valid one, then the robot will not deliver the medicine to the patient and it will send the delivery_status as “failed” to the central iot hub and then the deliveryStatus table gets updated and it measures the accuracy of the RoboNurse delivery project prototype.Both delivery status data and accuracy rate will be displayed in the dashboard. For invalid images, the central hub sends the response as “Not Patient”.


9. But for the valid patient image, the buzzer will start ringing and the robot will deliver the medicine to the patient and then send the delivery  status as “success”. Rest of the process would be the same for the central iot hub.
