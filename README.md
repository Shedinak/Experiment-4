## Create-a-datacenter-with-one-host-and-run-one-cloudlet-on-it.


## Experiment 


# Create a Datacenter with One Host and Run One Cloudlet (CloudSim)

This repository demonstrates a **basic CloudSim simulation** where a single datacenter with one host is created, and one cloudlet is executed on it. The project is intended for **cloud computing laboratory experiments** and beginner-level understanding of CloudSim architecture.

---

## Aim

To create a simple cloud computing environment using CloudSim by configuring one datacenter with a single host and executing one cloudlet, and to observe the simulation results.



## Objectives

* Understand the working of the CloudSim toolkit
* Learn how to create a datacenter and host in CloudSim
* Execute a single cloudlet on a virtual machine
* Analyze cloudlet execution output

---

##  Software Requirements

* Operating System: Windows / Linux / macOS
* Java Development Kit (JDK): 8 or above
* IDE: Eclipse / IntelliJ IDEA / NetBeans
* CloudSim Toolkit: Version 3.x

---

##  Hardware Requirements

* Processor: Intel i3 or higher
* RAM: Minimum 4 GB
* Storage: At least 10 GB free disk space

---

## Project Structure

```
Create-a-datacenter-with-one-host-and-run-one-cloudlet-on-it/
│
├── src/
│   └── DatacenterSingleHostSingleCloudlet.java
├── lib/
│   └── cloudsim-3.x.jar
├── output/
│   └── simulation_output.png
├── README.md
```

---



##  Steps to Run the Project

1. Install Java JDK (8 or above).
2. Download and configure the CloudSim library.
3. Import the project into your IDE.
4. Add CloudSim JAR files to the project build path.
5. Compile and run the Java program.
6. Observe the cloudlet execution results in the console.

---

##  Simulation Workflow

1. Initialize CloudSim
2. Create a Datacenter with one Host
3. Create a Broker
4. Create one Virtual Machine (VM)
5. Create one Cloudlet
6. Bind the Cloudlet to the VM
7. Start Simulation
8. Display Results

---
## Program
import org.cloudbus.cloudsim.*;
import org.cloudbus.cloudsim.core.CloudSim;
import org.cloudbus.cloudsim.provisioners.*;

import java.util.*;

public class DifferentMipsExample {

    public static void main(String[] args) {

        try {
            // Initialize CloudSim
            CloudSim.init(1, Calendar.getInstance(), false);

            // Create Datacenter
            Datacenter datacenter = createDatacenter("Datacenter_0");

            // Create Broker
            DatacenterBroker broker = new DatacenterBroker("Broker");
            int brokerId = broker.getId();

            // Create VMs with DIFFERENT MIPS
            List<Vm> vmList = new ArrayList<>();

            Vm vm1 = new Vm(0, brokerId, 500, 1, 512, 1000, 10000,
                    "Xen", new CloudletSchedulerTimeShared());

            Vm vm2 = new Vm(1, brokerId, 2000, 1, 512, 1000, 10000,
                    "Xen", new CloudletSchedulerTimeShared());

            vmList.add(vm1);
            vmList.add(vm2);
            broker.submitVmList(vmList);

            // Create Cloudlets (same workload)
            List<Cloudlet> cloudletList = new ArrayList<>();

            UtilizationModel utilization = new UtilizationModelFull();

            Cloudlet cloudlet1 = new Cloudlet(0, 40000, 1,
                    300, 300, utilization, utilization, utilization);

            Cloudlet cloudlet2 = new Cloudlet(1, 40000, 1,
                    300, 300, utilization, utilization, utilization);

            cloudlet1.setUserId(brokerId);
            cloudlet2.setUserId(brokerId);

            cloudlet1.setVmId(0);
            cloudlet2.setVmId(1);

            cloudletList.add(cloudlet1);
            cloudletList.add(cloudlet2);

            broker.submitCloudletList(cloudletList);

            // Run Simulation
            CloudSim.startSimulation();

            List<Cloudlet> resultList = broker.getCloudletReceivedList();

            CloudSim.stopSimulation();

            printResults(resultList);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // ================= DATACENTER =================
    private static Datacenter createDatacenter(String name) throws Exception {

        List<Host> hostList = new ArrayList<>();

        // Create TWO hosts
        for (int i = 0; i < 2; i++) {

            List<Pe> peList = new ArrayList<>();
            peList.add(new Pe(0, new PeProvisionerSimple(3000)));

            Host host = new Host(
                    i,
                    new RamProvisionerSimple(4096),
                    new BwProvisionerSimple(10000),
                    1000000,
                    peList,
                    new VmSchedulerTimeShared(peList)
            );

            hostList.add(host);
        }

        DatacenterCharacteristics characteristics =
                new DatacenterCharacteristics(
                        "x86", "Linux", "Xen",
                        hostList, 10.0, 3.0,
                        0.05, 0.1, 0.1);

        return new Datacenter(
                name,
                characteristics,
                new VmAllocationPolicySimple(hostList),
                new LinkedList<>(),
                0);
    }

    // ================= PRINT RESULTS =================
    private static void printResults(List<Cloudlet> list) {

        System.out.println("\n========== OUTPUT ==========");
        System.out.println("Cloudlet | VM | Status | ExecTime | Start | Finish");

        for (Cloudlet cl : list) {
            System.out.printf("%5d\t%3d\t%s\t%.2f\t%.2f\t%.2f\n",
                    cl.getCloudletId(),
                    cl.getVmId(),
                    cl.getStatus() == Cloudlet.SUCCESS ? "SUCCESS" : "FAILED",
                    cl.getActualCPUTime(),
                    cl.getExecStartTime(),
                    cl.getFinishTime());
        }
    }
}










##  Output

The simulation displays:

* Cloudlet ID
* Execution status
* Datacenter ID
* VM ID
* Start time
* Finish time

 

##  Output Screenshot

Add the console output screenshot in the `output/` folder and name it as `simulation_output.png`.

---

##  Applications

* Cloud Computing Laboratory Experiments
* Learning CloudSim Basics
* Academic Demonstrations

---

##  Conclusion

This project successfully demonstrates the creation of a simple cloud environment using CloudSim, where a single cloudlet is executed on one host. It provides a strong foundation for understanding more complex cloud simulations.

---

 

---

##  License

This project is intended for educational and academic use only.

