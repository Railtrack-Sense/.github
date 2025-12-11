# Welcome to the Railtrack Sense project!

![Transware](https://pride-badges.pony.workers.dev/static/v1?label=Transware&stripeWidth=8&stripeColors=5BCEFA,F5A9B8,FFFFFF,F5A9B8,5BCEFA)

This project aims to use IoT inertial sensing to monitor the quality of railway lines, enabling easy and automated detection of localised anomalies.

This project was started as a university project (DE4 Sensing and IoT, Dyson School of Design engineering, Imperial College London, 2025).

It comprises of 3 main parts, the sensing device, the backend and the dashboard / portal thingy.

## Sensing device:
ESP32 (lilygo T display - why? Because thats what I had laying around >u<) with an invensense LCM20948 stuck onto it.
This is ofcourse highly temporarily and is only meant as a proof of concept. In practise, a purpose designed PCB should be used.

## The backend:
The backend is of a distributed monolith architecture, comprising of two main sub-parts, a REST api written completely in C++ with uWebsockets for maximum performance and an Apache Cassandra cluster (probably should switch to scyllaDB soon for better performance).

The backend services depend on [this Cassandra C++ driver wrapper](https://github.com/MacroMelon/cassandra_cpp_wrapper) that I made to help a bit with the memory saftey of the datastax C++ driver. (Did it help? uhhhhh....)

This whole thing is deployed on a kubernetes cluster for robust fault tolerance and ease of deployment / configuration.

### Why so obsessed with performance?
Each sensing device samples inertial data at above 200Hz. Potentially having multiple devices per train, the amount of data being flung at the backend from across an entire rail network could get pretty huge! Also more performance means less rescources used to accomplish a given task, so its more sustainable too! Yay! :D

## The dashboard portal thingy
This is where you can look at the gathered data. Oauth authentication using Asgardeo is in progress to ensure the data remains secure. Its just a standard react app using deno, vite and tanstact start (for ssr / ssg).
