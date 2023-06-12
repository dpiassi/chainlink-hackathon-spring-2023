# Shipchain • Send and track deliveries all around the world!
> Empower decentralized sharing economy organizations by automating the shipping process of packages in the real world. We use smart contracts and IoT devices to ensure reliability and safety.

This repository contains 4 submodules:
1. `arduino` aka _IoT layer_
2. `backoffice` aka _UI layer_
3. `contracts` aka _Smart Contract layer_
4. `server` aka _Back-end layer_

![Solution flowchart showcasing the integrations among all the four layers](/shipchain-flowchart.jpeg "Shipchain flowchart")

## Inspiration

We genuinely believe that smart contracts can empower actual sharing economy societies to rise and flourish. Everything that happened recently in the Web2 scenario regarding sharing economy came with centralization. New big techs, such as Uber and AirBnB, began as startups a decade ago and are now worth more than seventy billion dollars each. They charge fees over drivers and hosts services, taking up a part of what is paid by their riders and guests. Even though these companies started providing new sources of revenue for many different people and families, allowing them to change their lives, nowadays social and economic issues on these platforms are more common and doubts arises if their business model is profitable. To be honest, what’s the point of having a centralized organization charging fees on services that are decentralized by nature and could be more secure and reliable if they embrace smart contracts instead of the current solutions? Wouldn’t it be better for both drivers and riders, hosts and guests to just connect themselves directly, skipping fees and counting on the safety of blockchains? Wouldn’t these stakeholders reach a greater quality of life and income this way?

Questions like that inspired us to build “Ship Chain” project as a minimum-viable product of what sharing economy might become in the next few years. So, instead of rides and rooms, we choose to start by "smartening" shipping and delivery services.

By automating the shipping process of any kind of package in the real world, using smart contracts and IoT devices, our goal is to bring senders, receivers, and deliveries together in an economic arrangement that is healthy and wealthy for them all. We're empowering social change by giving bikers, riders, drivers, truckers, etc., a new way of life, by reducing fees and breaking barriers in commerce logistics.

## What it does

The idea for making item tracking more convenient, intelligent, and secure was to use the blockchain to store and coordinate the main operations of the service. So, we decided to build a cargo tracking service with information processing on the blockchain. Its differential would be exactly this intelligent processing using the blockchain, which makes the creation of follow-up orders, monitoring, and receipt extremely secure, fast, automated, and reliable.

Our current solution allows two parties — "sender" and "receiver" — to create an agreement about the delivery of a certain item. This delivery has an associated IoT device that tracks its location, and the smart contracts will ensure that the delivery is completed, reducing the fees involved in these Web2 services. In addition, the customer can track all information about the shipment in real time, removing the need to contact the company responsible for the tracking service. The goal in the future is to allow any physical delivery service to be performed on the Web3, as well as any other sharing economy arrangement.

## How we built it

Our solution can be divided into four layers:

- **UI layer:** The front-end, where users can create, follow, and confirm their delivery orders.
- **Smart Contract layer:** Create and handle the orders on the blockchain, uses Chainlink services to communicate with the Back-end layer to get the position of the delivery and compares with the destination to verify if the delivery was successful.
- **IoT layer:** The physical device that tracks the locations of each delivery and sends over to the Back-end service.
- **Back-end layer:** Handles the information sent by the IoT device and provides endpoints for integrating among the Smart Contracts and the front-end layers.

Here is a summary of each layer. More details are available in the readme of git repositories.

### UI layer

Technologies:

- Typescript → Language
- Remix → Full-stack framework
- [Fly.io](http://fly.io/) → Host and deploy the service
- Google Maps API → To visualize and follow the delivery

The UI layer is the first view for the user, where they can create, confirm, and follow their orders. The orders are sent over to the smart contract. The users can visualize the delivery position in the map and know if the order is close to the destination location.

### Smart Contract layer

Contracts:

- Shipping → It handles the process of sending any kind of package in the real world. It gathers location data from IoT devices and stores it in the blockchain.
- Order → stores the status of shipping orders
- ConvertLatLng → a helper used to validate serialization/deserialization of `LatLng` data as `int256`.

During the hackathon challenge weeks, we studied three different main approaches when developing our contracts, each one based on different Chainlink services:

- Shipping → Implementation using external Chainlink API calls
- ShippingFunctionsConsumer → Implementation using the Chainlink Functions framework
- ShippingAutomatedFunctionsConsumer → Implementation using Chainlink functions and the automate framework

Finally, we decided to go ahead with the API Consumer option due to the simplicity of its implementation, which is extremely practical. The lack of reasons to automate some of the contract callbacks also led us to this decision. Currently, all functions in the contracts are triggered based on user input/interaction via the Web Dashboard. The delivery contract is responsible for handling all such requests, which can change contract state variables, read data from sources outside the chain, create new order contracts, and update order states.

### IoT layer

Materials:

- GPS module: GY-GPS6MV1
- Arduino Uno R3
- Any computer to upload the script

Technologies:

- Python
- Ino: Ino is a language to code with Arduino that is compiled to C++

Our purpose was to create a device with the ESP8266-01 that can connect to wi-fi and make HTTP requests, but we couldn’t do it due to the limitations of the serial communications of the Arduino board. As we didn't have time to buy another board, such as a NodeMcu that would be able to properly handle the requests, we used a Python script to get the locations sent by the Arduino via serial communication and send this data to our backend.

With our Arduino code we get the latitude and longitude of the device and send them over to the computer through serial communication. Then the Python script reads and serializes the data to send to the Back-end server.

### Back-end layer

Technologies:

- NodeJs → Javascript runtime.
- Typescript → Language
- MySql → Database
- [Fly.io](http://fly.io/) → Host and deploy the service
- PlanetScale → Host the database

We’ve built the backend using NodeJS and TypeScript. This service handles the information about three entities: orders, locations, and devices. Besides, it integrates with the Front-end layer to send the information about the orders locations, with the IoT device to get the latitude and longitude of the delivery and with the Smart Contract layer to send the last known location of the order.

## Challenges we ran into

In the beginning, we tried to use the "Space and Time" solution to store all the business data — orders, locations, and devices — and to read from our smart contract's registries. However, the complexity of the solution to create tables, the need to authenticate every 30 minutes and the fact that the beta dashboard was not available for hackathon participants lead us to rethink and change the strategy. As our purpose was to create a prototype, a proof-of-concept, we decided to store this information in a MySql database. Regarding this topic, SxT, we hope that in the next hackathons the beta dashboard and direct access to dApp would be already available for everyone, at least for trial purposes.

During the construction of the IoT device we had several issues. At first, we bought the ESP8266-01 that connects to the wi-fi to get the location data from the Arduino via serial communication and send it via HTTP request to our server. However, our Arduino does not allow 2-channel serial communication (we needed one channel for the GPS and another for the ESP). The viable alternative was to build using a NodeMcuESP8266, but the board did not arrive in time. That's why we took the strategy to serialize the data through a Python script that sends it to our off-chain server.

## Accomplishments that we're proud of

It was quite easy to study and experience the concepts behind Chainlink and Oracles. These concepts were difficult to understand at first glance. However, everything became clear when we started to write, test, and deploy our own oracle contracts. So, we’re proud of achieving a functional PoC which integrates off-chain and on-chain resources seamlessly!

## What we learned

For the team, this was an extremely challenging project, mainly because we had never developed a Web3 application or one that involved IoT, so many blocking points were found and the best was done to overcome them and deliver a functional solution.

About the Client for the user, the main learnings were in the UI/UX aspect, being all members of the team Back-End engineers. Designing an interactive UI and a pleasant user experience proved to be a good challenge. Also, React skills were improved, and new APIs were learned during the process.

On the overall contract side, the learning focused on how the creation of a contract works, learning the Solidity language and how to integrate the contract with numerous services.

Lastly, there is the Iot part. It was the first project that this team created that involved both hardware and software. With this, the learning focused on how to program and assemble the Arduino board so that it would operate as desired. Also, how to make the back-end integrations with the board, so it could communicate with dashboard and with the contract.

## What's next for Ship Chain
- Improvement of the contracts, allowing other segments like mail and food delivery to use the solution.
- Deep research of the markets we can enter and the public.
- Complete the IoT device using a NodeMCU ESP8266 that doesn’t need an Arduino and make HTTP request easier.
- Upgrade the smart contract to allow transferring the combined amount to the person/company who made the delivery, as well as locking the upfront payment as collateral.

## Related links

- [Public submission on DevPost](https://devpost.com/software/ship-chain)
- [Public pitch deck that summarizes the proposal](https://tome.app/ship-chain/shipchain-or-chainlink-hackathon-spring-2023-cliqrszny1islmt3day33kz9u)
