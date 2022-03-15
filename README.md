Readme

From the central IoT hub, send the medication_delivery request  which includes the room number to which the robot needs to go to deliver the medicine.To initiate the flow,  one needs to press the inject button. This instruction will be published using MQTT protocol and can be available in hiveMQ broker.

Robot will consume the request and then start moving and publish all the movement details to the robot_movement topic.
Central iot hub will subscribe the topic and update the dashboard with robot movement details

At the same time the robot will subscribe to the instruction and search for that particular room. It will capture the image and send it to the central iot hub for validation

Central iot hub will subscribe the message with room number and validate the room. If the room number does not match with the expected one(which has been sent at the start), then the robot will keep moving and capture the room image. This activity continues until the image matches with the expected room number.

Here the central iot hub leverages the azure cognitive model(custom vision) to predict the room image.

Once the robot finds the expected room, it captures the patient image and sends it back to the central iot hub. Then the central iot hub sends the patient image  to the azure cognitive model(custom vision) to predict and validate the patient image and publish the response to the message broker.

If the captured image is not the valid one, then the robot will not deliver the medicine to the patient and it will send the delivery_status as “failed” to the central iot hub and then the deliveryStatus table gets updated and it measures the accuracy of the RoboNurse delivery project prototype.Both delivery status data and accuracy rate will be displayed in the dashboard. For invalid images, the central hub sends the response as “Not Patient”.

But for the valid patient image, the buzzer will start ringing and the robot will deliver the medicine to the patient and then send the delivery  status as “success”. Rest of the process would be the same for the central iot hub.



Central hub node details:

It uses sqliteDB for all the DDL and DML operations. UML diagram of sqlliteDB is as follows
<img width="735" alt="Screen Shot 2022-03-14 at 8 21 43 PM" src="https://user-images.githubusercontent.com/55467163/158308852-3e06f31a-61f9-4d07-89fe-0f9eaa9b6f64.png">



We have used MQTT protocol for commuting to other raspberry pi. And we tested publishing/subscribing messages using hiveMQ cloud broker.



