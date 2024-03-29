### Google Compute Engine (GCE)

GCE is for provisioning and managing virtual machines in GCP.
Virtual machines - virtual servers in GCP.
Features include:
  * load balancing and autoscaling of multiple VMs
  * attach storage to your VMs
  * manage network connectivity and configuration

#### VM Creation

- Choose zone and region
- Attach labels as needed
- Choose machine configuration:
  * Machine family:
    * General-purpose (E2 N2, N2D, N1) -best price-performance ratio
    * Compute optimized (C2) - compute intensive workloads
    * Memory-optimized (M2, M1) - ultra high memory workloads
  * Machine types:
    * e2-standard-2;
      - d2: machine type family
      - standard - type of workload
      - 2: Number of CPUs
  * Boot disk (OS)
    * Choose the image:
      * Public (Google-managed or open-source) images, custom images, snapshots, etc.
  * Firewall settings (HTTP/HTTPs traffic)

  #### Internal and External IP Addresses

  - External (public):
    - Internet addressable
    - Ephemeral by default; released when the VM instance is stopped.
    - Should be unique; no two resources with same public IP address allowed.
    - To make this constant, use a Static IP address:
      - VPC network > External IP Addresses > Reserve a static address
      - Once the statitc IP address is created, attach it to a VM instance.
      - Once the static IP address is assigned, the existing ephemeral external address of that VM instance is automatically removed.
      - Static IP addresses can be swithced from one VM instance to another.
      - Static IP addresses remains even when you stop the VM. You need to manually detach it.
      - <strong> You get billed for static IP address even when you're not using it </strong>

  - Internal (private):
    - Internal to a corporate network
    - Two resources can have the same IP address
  - All VM instancess are assigned at least one internal IP address

Ways to reduce the server setup
1) Startup script
- Bootstrapping:
  - install OS patches or software at the time of VM instance launch using a startup script.
2) Instance template
- Pre-define macine type, image, labels, startup script, etc.
- Used to create VM instances and managed instance groups
- <strong> CANNOT BE UPDATED </strong>
3) Custom image
- further reduce launch time by using an image with pre-installed OS patches and software
- Can be created from an instance, a persistent disk, a snapshot, another image, or a file in GCS.
- Can be shared across projects
- Old images should be marked as deprecated
- Hardening an image - customize images to your corporate security standards
- Instance A -> Image -> Instances
- You could also create an instance template from a custom image:
  - Instance A -> Image -> Instance template -> Instances

