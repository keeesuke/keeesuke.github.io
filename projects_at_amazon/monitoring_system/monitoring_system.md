## **Monitoring System Overview- Seiren**

Seiren is a visualization system developed in-house by Amazon Japan. This system enables operational users to monitor the real-time operational status, productivity data, and error logs of FC (Fulfillment Center) equipment, as well as historical data of these metrics. The system retrieves real-time data from the PLCs (Programmable Logic Controllers) that control warehouse conveyors and facilities. The PLCs store this data in their register memory, and Seiren extracts it for visualization.

When an abnormality or warning occurs in the warehouse equipment, Seiren immediately displays the location and details of the issue on-screen while notifying operational staff through an alarm sound and a warning light. Without this system, identifying the precise location and cause of a conveyor malfunction in a vast warehouse would be significantly delayed, potentially halting outbound processes and leading to “Breaks,” where on-time delivery to customers is jeopardized. As a result, Seiren is an essential system for minimizing equipment downtime and ensuring timely customer deliveries.

## **Project Role and End-to-End Execution**

The Seiren project was a comprehensive initiative that spanned system design, development, deployment, and ongoing maintenance. The project required a multidisciplinary approach, involving collaboration across various internal departments and external vendors to ensure its success. Key stakeholders included Finance and Procurement teams, which facilitated budget allocation, and on-site operational users, whose input shaped the refinement of the system's user interface to align with practical needs. As the project manager for Seiren, I was involved in the launch of six new warehouses and successfully implemented Seiren in all FCs.

During deployment, strategic planning was undertaken in coordination with the IT team to install monitoring screens in on-site control rooms, enhancing operational visibility and functionality. Additionally, software vendors were engaged for system development, while material handling vendors contributed by designing detailed PLC communication protocols. Effective management of schedules, procurement processes, and vendor coordination played a critical role in this project, ensuring that Seiren met its operational objectives and delivered value to the organization.

Amazon Japan launches multiple new FCs each year, and this monitoring system must be installed in all Amazon FCs. The preparation for the monitoring system begins approximately six months before the scheduled launch date, working backward from the launch date. The general deployment timeline is as follows:

- **Six months before launch**: Create vendor requirement specifications, design system details, and develop the user interface layout.

- **Four months before launch**: Conduct user design reviews and place orders with software vendors for development.

- **Three months before launch**: Set up servers, configure the environment, and establish the database.

- **Two months before launch**: Deploy the system to the servers and conduct offline testing.

- **One month before launch**: Perform on-site communication tests (testing 50–100 PLCs individually), finalize monitor placements in on-site control rooms, and coordinate cabling with the IT team.

- **Immediately before launch**: Conduct system specification briefings for users and perform final debugging.

- **Post-launch**: Address user requests for modifications, perform additional debugging, develop and integrate new features, and implement security updates.

## **System User Interface and Function**

Below is a sample image of Seiren’s main dashboard and real-time monitoring interface, illustrating its design and functionality (Noting that this is not a real image of the actual system, but is resemble to the real UI).

<img src="/assets/img/monitoring_system/UI_sample.png" width="800"/>

Key Features of Data Referenced in Seiren are:

- **Real-Time Monitoring**: Displays the operational status of facilities in real-time. Abnormalities are highlighted in red on a mapped control panel layout. Statuses include "Running," "Stopping," "Full," "Temporary Stop," and "Error."

- **Counting Function**: Aggregates data from sensors and BCRs (barcode readers) into tables that can be viewed and downloaded.

- **Error Logs**: Records and displays details of all abnormalities, including timestamp, control panel number, location, issue description, and recovery status.

- **KO Logs**: Logs details of all KO (Kick-Out, i.e. side out the problematic tray) occurrences, including timestamp, control panel number, location, and KO reason.

- **Signal Tower/Patlite Integration**: Alerts operational staff to abnormalities or warnings via buzzer sound and lamp signals in real-time, connected to PATLITE systems in maintenance offices.

## **System Architecture**

<img src="/assets/img/monitoring_system/System_architecture.png" width="800"/>

Seiren consists of a frontend server, a backend server, and a database, all hosted within the Amazon Network, as illustrated in the diagram above. Data such as equipment operational status by area, shipment counts, and error details are collected from individual PLCs within the FC and stored in the database on the Amazon Network. The stored data is categorized by type and displayed via a web interface. This website is accessible from PCs connected to the Amazon Network, including those within the warehouse.

## **Reference**
- UI sample image source: <https://www.southernconveyor.com/services/human-machine-interfaces/>
