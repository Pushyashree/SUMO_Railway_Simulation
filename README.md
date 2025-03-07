# 🚆 SUMO Railway Simulation Workflow

This README provides a step-by-step workflow for setting up and running a **railway simulation** using **SUMO** (Simulation of Urban MObility). The scenario involves simulating trains running between **three main stations** and **intermediate stations**, with two different types of routes:  
- 🚉 **Trains that stop at all stations**.  
- 🚄 **Trains that skip some stations**.  

The workflow involves creating and configuring several files, including `.net`, `.add`, `.trip`, `.rou`, and `.cfg` files.

---

## 🏗️ 1. Creating the Network File (`.net.xml`)

Note: Edges and nodes data were obtained from ([Open Street Map](https://www.openstreetmap.org/#map=6/51.33/10.45)) data.

### 🎨 **Using NETEDIT to Create Nodes and Edges**  
The railway network can be created using **NETEDIT**, a graphical editor for SUMO networks. NETEDIT allows users to:  
- 🔹 Add **nodes** representing junctions or stations.  
- 🔹 Add **edges** representing railway tracks between the nodes.  

#### 📝 **Steps to Create a Network in NETEDIT**  
1️⃣ Open **NETEDIT**.  
2️⃣ Add **nodes** at the desired locations for stations and junctions.  
3️⃣ Connect nodes by adding **edges** to form railway tracks.  
4️⃣ Save the network file as **`.net.xml`**.  

📖 **More details**: [SUMO NETEDIT Documentation](https://sumo.dlr.de/docs/NETEDIT.html).

### 🚦 **Defining Rail Signals in NETEDIT**  
In a railway network, junctions can act as **block sections** controlled by rail signals.

#### 🔧 **Procedure to Define Rail Signals**  
1️⃣ Select a node in NETEDIT.  
2️⃣ Set the node type to **rail signal**. This can also be done manually by editing the `.net.xml` file and adding the attribute `junction="rail_signal"` to the relevant node.  

📖 **More details**: [Defining Rail Signals](https://sumo.dlr.de/docs/NETEDIT.html#rail_signals).

---

## 🏁 2. Creating the Additional File (`.add.xml`)

The `.add.xml` file contains **rail stops** defined as bus stops in SUMO. Rail stops are added to specify where trains will stop at stations.

### 🛠️ **Steps to Define Rail Stops in NETEDIT**  
1️⃣ Select the desired edge where the rail stop should be placed.  
2️⃣ Add a **bus stop** by specifying its length and position on the edge.  
3️⃣ Save the additional file as **`.add.xml`**.  

📖 **More details**: [Adding Rail Stops](https://sumo.dlr.de/docs/NETEDIT.html#bus_stops).

---

## 🚉 3. Creating the Trip File (`.trip.xml`)

The `.trip.xml` file defines the trips (**train journeys**) in the network. Each trip includes:  
- 🚄 **The route that the train follows**.  
- ⏳ **Departure times**.  
- 🏁 **Stopping times at each station**.  

### 📜 **Manual Creation of Trips**  
1️⃣ Define **two different routes**:  
   - 🚉 **Route 1**: Covers all stations.  
   - 🚄 **Route 2**: Skips some intermediate stations.  
2️⃣ Create trips for each train with:  
   - **A 20-minute headway** between consecutive train departures.  
   - **A stopping time of 30 seconds** at each stop.  
   - **A total simulation duration of 8 hours**.  

📖 **More details**: [SUMO Trips Guide](https://sumo.dlr.de/docs/Simulation/Trips.html).

---

## 📍 4. Generating the Route File (`.rou.xml`)

The `.rou.xml` file specifies the **exact routes** taken by the trips. This file is generated by combining the `.net.xml` and `.trip.xml` files using **DUAROUTER**.

### ⚙️ **Using DUAROUTER**
DUAROUTER is a tool that converts trips into routes based on the network topology.

#### 🖥️ **Command to Generate the Route File**
```bash
duarouter -n network.net.xml -t trips.trip.xml -o routes.rou.xml
```
-n: Specifies the input network file.
-t: Specifies the input trip file.
-o: Specifies the output route file.
📖 More details: ([DUAROUTER Guide](https://sumo.dlr.de/docs/duarouter.html))

## ⚙️ 5. Creating the Configuration File (.cfg)
The .cfg file specifies the overall configuration for running the simulation. It includes references to all the required input files and simulation parameters.

📄 Basic Structure of the Configuration File
```sh
<configuration>
    <input>
        <net-file value="network.net.xml"/>
        <route-files value="routes.rou.xml"/>
        <additional-files value="stops.add.xml"/>
    </input>
    <time>
        <begin value="0"/>
        <end value="28800"/><!-- 8 hours = 28800 seconds -->
    </time>
    <output>
        <tripinfo-output value="tripinfo.xml"/>
    </output>
</configuration>
```
📖 **More details**: ([SUMO Configuration Guide](https://sumo.dlr.de/docs/sumo.html#configuration)).

## 🚀 6. Running the Simulation
To run the simulation, use the following command:
```sh
sumo-gui -c simulation.cfg
```
## 📊 7. Output Files
SUMO generates several output files based on the configuration. Common outputs include:

📄 tripinfo.xml: Contains detailed information about each trip, including departure time, arrival time, and travel duration.
📖 More details: ([SUMO Output Files](https://sumo.dlr.de/docs/Simulation/Output/index.html))

## ✍️ Author
👤 Pushya Shree Konasale Jayaramu
📧 pushyashree.kj.2000@gmail.com
🔗 https://www.linkedin.com/in/pushya-shree-konasale-jayaramu-6a61881a8/
