### Optimizing Cost and Performance

#### Sustanined use discounts
- Automatic
- Increases with usage
- billed per month
- Applicable for instances created by GKE and GCE only
- Does not apply to certain machine types such as E2 and A2.

#### Committed use discounts
- For predictable resource needs
- 1 year or 3 years
- Up to 70% discount
- Applicable for instances created by GKE and GCE only

Steps: GCE > Virtual machines > Committed use discounts
* Region, commit type, duration, GPUs, memory, SSDs, etc.

#### [Pre-emptible VMs](https://cloud.google.com/compute/docs/instances/preemptible)
- Google definition: Preemptible VM instances are available at much lower price—a 60-91% discount—compared to the price of standard VMs. However, Compute Engine might stop (preempt) these instances if it needs to reclaim the compute capacity for allocation to other VMs. Preemptible instances use excess Compute Engine capacity, so their availability varies with usage.<br>
If your apps are fault-tolerant and can withstand possible instance preemptions, then preemptible instances can reduce your Compute Engine costs significantly. For example, batch processing jobs can run on preemptible instances. If some of those instances stop during processing, the job slows but does not completely stop. Preemptible instances complete your batch processing tasks without placing additional workload on your existing instances and without requiring you to pay full price for additional normal instances.

- short-lived (24 hours), cheaper compute instances.
- Can be stopped by GCP any time within 24 hours.
- Instances get 30 second warning before termination
- Use if:
  * your applications are fault tolerant (so you can stop it any time)
  * cost-sensitive
  * workload is not immediate (e.g., non-immediate batch processing)
- Limitations:
  * not always available
  * No SLAs and cannot be converted to regular VMs
  * No automatic restart after you stop them
  * Free tier credits not applicable.

#### [Spot VMs](https://cloud.google.com/compute/docs/instances/spot)
- Pre-emptible VMs without a maximum runtime unless specified.
- Not always available
- 60 - 91% discount
- May be reclaimed by Google at any time with a 30-second notice
- Free credits don't apply.

#### Billing
- Billed by second (after a minimum of 1 minute)
- Not billed for a stopped compute instance, but billed for <strong>storage</strong> attached to it.
- You can create a budget:
  * Billing > Budgets > Alerts > Create budget
- Billing export
  * Can send billing data to Big Query.


#### Live migration and availability policy
Live migration
* Your running instance is migrated to another in the same zone while the host system needs to be updated
* Does not change any attributes of the VM
* Supported for instances with local SSDs
* Not supported for GPUs and pre-emptible instances

To configure migration, set availability policy:
* On host maintenance:
  * `migrate` (Default) - migrate VM instance to other hardward
  * `terminate` - stop the VM instance during host maintenance
* Automatic restart` - restart VM instances if they are terminated due to non-user initiated erasons (maintenance, hardware failure, etc.)

#### Custom Machine Types
* Customize vCPUs, memory, and GPUs.
* Only for E2, N2, or N1 machine types
* Billed per vCPUs, memory provisiond to each instance.
* Steps: Create an instance > Machine configuration > Machine type > custom > Set cores and memory

#### GPUs
* Add a GPU to CM for graphics-intensive workloads
* Higher performace, higher cost
* Must use images with GPU libraries installed
* Limitations:
  - Not supported on all machine types
  - On host maintenance is only `terminate` only.
* Recommended availability policy is restart.

#### VMs
* associated with a project
* certain machine types are available in certain regions only
* You need to stop a VM instance before trying to change its machine type (CPUs and memory)
* Instances are zonal
* Images are global - can be shared across projects, multiple zones and regions.
* Instance templates are global
* Automatic basic monitoring is autmoatically enabled.

* [Sole tenant nodes](https://cloud.google.com/compute/docs/nodes/sole-tenant-nodes)
- if you want only your corporate's instances to be running in the same host, you setu p a sole tenante node.
- Steps:
  * Create a node group
  * Set node template properties
    * CReate node template if none exists
    * choose node type
    * add affinity label to ensure that instnaces run on the required node groups.
  * Configure autoscaling and maintenance
  * Once the node template is created, create a VM instance > sole tenancy > node affinity labels > Now the VM will be created in that specific node.

  * [VM manager](https://cloud.google.com/compute/docs/vm-manager)
    - manage OS patch management, OS inventory management, and configuration for a fleet of VMs
    - The following services are available as part of the VM Manager suite:
      * Patch: use this service to apply on-demand and scheduled patches. You can also use Patch for patch compliance reporting in your environment.
      * OS inventory management: use this service to collect and review operating system information.
      * OS policies: use this service to install, remove, and auto-update software packages.
  * To not expose a VM to internet, do not assign an external IP address to it.
