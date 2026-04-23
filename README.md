# Flow-Rule-Timeout-Manager
Implementing timeout-based flow rule management

🧠 Flow Timeout Manager in SDN (Ryu + Mininet)
📌 Overview

This project implements a Software Defined Networking (SDN) controller using the Ryu framework to manage OpenFlow rules with:

Idle Timeout
Hard Timeout

It demonstrates the complete lifecycle of flow rules:

Flow creation
Flow usage
Flow expiration
Flow removal
🎯 Objectives
Configure idle and hard timeouts
Automatically remove expired flow rules
Demonstrate flow lifecycle
Analyze network behavior
Perform regression testing
🛠️ Requirements
Software
Ubuntu Linux
Python 3
Mininet
Ryu Controller
Open vSwitch
Install (if needed)
sudo apt update
sudo apt install mininet python3-pip -y
pip3 install ryu
📁 Project Structure
sdn-flow-timeout-manager/
│
├── flow_timeout_manager.py
├── README.md
├── demo/
│   └── screenshots/
└── report/
⚙️ Setup (VERY IMPORTANT ORDER)

⚠️ Follow this order strictly or the controller will not work.

🟢 STEP 1: Activate Virtual Environment
source sdn-env/bin/activate

✔ Ensures Ryu runs correctly
✔ Avoids dependency issues

🟢 STEP 2: Clean Previous Mininet State
sudo mn -c

✔ Removes old switches, flows, and processes
✔ Prevents conflicts

🟢 STEP 3: Start Controller (Terminal 1)
ryu-manager flow_timeout_manager.py

✔ Keep this terminal running
✔ Do NOT close or interrupt

Expected output:

Switch connected → Table-miss installed
🟢 STEP 4: Start Mininet (Terminal 2)
sudo mn --topo single,2 \
--controller=remote,ip=127.0.0.1,port=6653 \
--switch ovsk,protocols=OpenFlow13

✔ Connects switch to Ryu controller

🟢 STEP 5: Monitor Flow Table (Terminal 3)
watch -n 1 "sudo ovs-ofctl -O OpenFlow13 dump-flows s1"

✔ Shows flow rules in real-time

🧪 Running the Demo
🔹 1. Generate Traffic

In Mininet:

h1 ping -c 3 h2

✔ Triggers flow creation

🔹 2. Observe Flow Creation

You will see:

Controller:
Flow ADDED
Flow installed
Flow table:
idle_timeout=5
hard_timeout=10
🔹 3. Demonstrate Idle Timeout
Run ping:
h1 ping h2
Stop it (Ctrl + C)
Wait ~5 seconds

✔ Flow disappears from table
✔ (Optional) Controller logs removal

🔹 4. Demonstrate Hard Timeout
Run continuous ping:
h1 ping h2
Do NOT stop
Wait ~10 seconds

✔ Flow removed even with traffic

🔄 Flow Lifecycle
1. Packet arrives → no flow
2. Switch sends PacketIn to controller
3. Controller installs flow
4. Switch forwards packets directly
5. Flow becomes idle or reaches timeout
6. Flow removed automatically
7. New packet → new flow created
📊 Behavior Analysis
Scenario	Behavior
First packet	Slow (controller involved)
Subsequent packets	Fast (flow installed)
After timeout	Slow again (flow recreated)
🔁 Regression Testing

Repeat:

h1 ping -c 3 h2
(wait 5–6 sec)
h1 ping -c 3 h2

✔ Each time:

Flow created
Flow removed
Same behavior observed


🧠 Key Concepts
Idle Timeout → removes unused flows
Hard Timeout → removes flows after fixed time
Controller → installs rules
Switch → executes rules

🏁 Conclusion

This project demonstrates:

✔ Dynamic flow rule installation
✔ Automatic timeout-based removal
✔ Efficient SDN behavior
✔ Complete flow lifecycle

👤 Author

Your Name
GURUBELLI YEKAMBAR ESHWAR RAO
