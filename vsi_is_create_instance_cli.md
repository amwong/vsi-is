
---

copyright:
  years: 2018
lastupdated: "2018-08-15"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}

# Creating virtual server instances (CLI)
{: #creating-virtual-servers-cli}

You can create instances using the command line interface (CLI).
{:shortdesc}

## Before you begin
1. Ensure you have downloaded, installed, and initialized the following CLI plug-ins:
    * {{site.data.keyword.cloud_notm}} CLI
    * {{site.data.keyword.cloud_notm}} Regional API CLI

   For more information, see [CLI access](/docs/infrastructure/vpc/how-to-verify-access.html).
2. Ensure you have already created a virtual private cloud.

## Gathering information to create an instance using the CLI
Ready to create an instance? Before you can run the `ibmcloud is instances` command, you'll need to know the details about the instance, such as what profile or image you want to use.

Let's gather that information first:

|    Instance details   |  Listing options                |  Copy your values                             |
| --------------------- | --------------------------------|---------------------------------------------- |
| Image                 | `ibmcloud is images`            |                                               |
| Profile               | `ibmcloud is instance-profiles` |                                               |
| Key                   | `ibmcloud is keys`              |                                               |
| Subnet                | `ibmcloud is subnets`           |                                               |
| VPC                   | `ibmcloud is vpcs`              |                                               |
| Zone                  | `ibmcloud is zones`             |                                               |   
{: caption="Table 1. Required instance details" caption-side="top"}   

Use the following commands to determine the required information for creating a new instance.

1. List the regions associated with your account.
   ```
   $ ibmcloud is regions
   ```
   {:codeblock}

   For this example, you'd see a response similar to the following output:
   ```
   Name       Endpoint               Status   
   us-south   /v1/regions/us-south   available
   ```
   {:screen}

2. List the zones associated with the desired region.
   ```
   $ ibmcloud is zones us-south
   ```
   {:codeblock}

   For this example, you'd see a response similar to the following output:
   ```
   Name         Region     Status   
   us-south-1   us-south   available
   ```
   {:screen}

3. List the virtual private clouds that are associated with your account.
   ```
   $ ibmcloud is vpcs
   ```
   {:codeblock}

   For this example, you'd see a response similar to the following output:
   ```
   ID                                     Name                                  Default   Created       Status      Tags   
   xxx1xx23-4xx5-6789-12x3-456xx7xx123x   my-vpc                                yes       1 month ago   available   -   
   xxxx1234-5678-9x12-x34x-567x8912x3xx   my-other-vpc                          no        4 days ago    available   -   
   ```
   {:screen}

   If you do not have one available, you can create a VPC using the `ibmcloud is vpc-create` command. For more information about creating a VPC, see [VPC Beta CLI reference](/docs/infrastructure/vpc/cli-network-reference.html).

4. List the subnets that are associated with the virtual private cloud.
   ```
   $ ibmcloud is subnets
   ```
   {:codeblock}

   For this example, you'd see a response similar to the following output:
   ```
   ID                                     Name                                     IPv*   Subnet CIDR         Addresses   Gen   ACL   Gateway   Created       Status      VPC                        Zone         Resource Group   Tags   
   1234x12x-345x-1x23-45x6-x7x891011x1x   my-subnet                                ipv4   172.16.1.0/24       0/0         -     -     -         1 week ago    available   my-vpc(xxx1xx23-.)         us-south-1   -                -   
   12xx345x-6789-1xxx-x2x3-x4x56xx78x9x   my-subnet-2                              ipv4   172.20.28.0/24      0/0         -     -     -         1 day ago     available   my-vpc(xxx1xx23-.)         us-south-1   -                -   
   1x2x3xx4-xx56-7891-234x-xx5678x9x123   my-other-subnet                          ipv4   192.168.88.0/24     0/0         -     -     -         1 day ago     available   my-other-vpc(xxxx1234-.)   us-south-1   -                -   
   ```
   {:screen}

   If you do not have one available, you can create a subnet using the `ibmcloud is subnet-create` command. For more information about creating a subnet, see [VPC Beta CLI reference](/docs/infrastructure/vpc/cli-network-reference.html).

5. List the available profiles for creating your instance.
   ```
   $ ibmcloud is instance-profiles
   ```
   {:codeblock}

   For this example, you'd see a response similar to the following output:
   ```
   Name            CPU Arch   CPU Cores   CPU Frequency   Memory   GPU Model   GPU Cores   GPU Count   CPU Memory   Max Volumes   Max IOPS   Max Interfaces   Max Bandwidth   
   B_2X4       amd64      2           2000            4                    0           0           0            25            0          0                0   
   B_4X8       amd64      4           2000            8                    0           0           0            100           0          0                0   
   M_16X128    amd64      16          2000            128                  0           0           0            25            0          0                0   
   B_48X192    amd64      48          2000            192                  0           0           0            100           0          0                0   
   B_8X32      amd64      8           2000            32                   0           0           0            100           0          0                0   
   C_16X16     amd64      16          2000            16                   0           0           0            25            0          0                0   
   M_4X32      amd64      4           2000            32                   0           0           0            100           0          0                0     
   ```
   {:screen}

6. List the available images for creating your instance.
   ```
   $ ibmcloud is images   
   ```
   {:codeblock}

   For this example, you'd see a response similar to the following output:
   ```
   ID                                     Name                 OS                                                       Arch    Created       Status   Visibility   Tags   
   cc8debe0-1b30-6e37-2e13-744bfb2a0c11   centos-7.x-amd64     CentOS (7.x - Minimal Install)                           amd64   9 hours ago   READY    public       -   
   7eb4e35b-4257-56f8-d7da-326d85452591   ubuntu-16.04-amd64   Ubuntu Linux (16.04 LTS Xenial Xerus Minimal Install)    amd64   9 hours ago   READY    public       -   
   ```
   {:screen}

7. List the available SSH keys that you can associate with your instance.
   ```
   $ ibmcloud is keys
   ```
   {:codeblock}

   For this example, you'd see a response similar to the following output:
   ```
   ID                                     Name           Type   Length   FingerPrint          Created        Resource Group   Tags
   1234xxxx-x12x-xxxx-34xx-xx1234xxxxxx   my-key         RSA    2048     PHcP/zyw/PNGIe/u..   5 days ago     -                -   
   12xx3456-x78x-9123-4x56-78xx9xxx1x2x   my-other-key   RSA    2048     +rvkRMBhdFmz1dlT..   2 days ago     -                -    
   ```
   {:screen}

   If you do not have one available, you can create an SSH key using the `ibmcloud is key-create` command. For more information about managing SSH keys, see [Managing SSH keys](vsi_is_ssh_keys.html).

## Creating an instance using the CLI
After you know these values, use them to run the `instance-create` command. In addition to the information that you gathered, you must specify a name for the instance, and a port speed in MB per second. The following example shows the command in action (using generic x and 123 values for example purposes only).  

1. Create a new instance.
   ```
   $ ibmcloud is instance-create \
       my-instance \
       xxx1xx23-4xx5-6789-12x3-456xx7xx123x \
       us-south-1 \
       b-2x4 \
       1234x12x-345x-1x23-45x6-x7x891011x1x \
       100 \
       --image 1xx2x34x-5678-12x3-x4xx-567x81234567 \
       --keys 1234xxxx-x12x-xxxx-34xx-xx1234xxxxxx
   ```
   {:codeblock}
   
   where:
   - `INSTANCE_NAME` is my-instance
   - `VPC_ID` is _VPC_ID_
   - `ZONE_NAME` is  us-south-1
   - `PROFILE_ID` is b-2x4
   - `SUBNET_ID` is _SUBNET_ID_
   - `PORT_SPEED` is 100
   - `IMAGE_ID` is _IMAGE_ID_
   - `KEY_IDS` is _KEY_ID1, KEY_ID2, ..._

   For this example, you'd see the following responses. **Note:** The following response will vary depending on what optional values you use.
   ```
   ID                2x12xxx5-xx11-1234-x4x5-1xxx12345678   
   Name              my-instance   
   Profile           B_2X4   
   Gen                  
   CPU Arch          amd64   
   CPU Cores         2   
   CPU Frequency     2000   
   Memory            4   
   Primary Intf      great-scott-stride-lilac-captivate-filtrate(xx12x345-5bxx-1x23-458x-1x2xxx345x6x)   
   Primary Address   172.16.1.4   
   Image             ubuntu-16.04-amd64(7eb4e35b-4257-56f8-d7da-326d85452591)   
   Profile           -   
   Status            pending   
   Created           now   
   VPC               my-vpc(xxx1xx23-4xx5-6789-12x3-456xx7xx123x)   
   Zone              us-south-1   
   Resource Group    -   
   Tags              -   
   ```
   {:screen}

   The status displays pending until the instance is created.
   {: tip}

2. When the status changes to *running*, verify that you can see your new instance.

   ```
   $ ibmcloud is instance 2x12xxx5-xx11-1234-x4x5-1xxx12345678
   ```
   {:codeblock}

   For this example, you'd see the following responses.
   ```
   ID                2x12xxx5-xx11-1234-x4x5-1xxx12345678   
   Name              my-instance   
   Profile           B_2X4   
   Gen                  
   CPU Arch          amd64   
   CPU Cores         2   
   CPU Frequency     2000   
   Memory            4   
   Primary Intf      great-scott-stride-lilac-captivate-filtrate(xx12x345-5bxx-1x23-458x-1x2xxx345x6x)   
   Primary Address   172.16.1.4  
   Image             ubuntu-16.04-amd64(7eb4e35b-4257-56f8-d7da-326d85452591)   
   Profile           -   
   Status            running   
   Created           5 minutes ago   
   VPC               my-vpc(xxx1xx23-4xx5-6789-12x3-456xx7xx123x)   
   Zone              us-south-1   
   Resource Group    -   
   Tags              -
   ```
   {:screen}

3. View the network interfaces that were created for your new instance.
   ```
   $ ibmcloud is instance-network-interfaces 2x12xxx5-xx11-1234-x4x5-1xxx12345678
   ```
   {:codeblock}

   For this example, you'd see the following responses.
   ```
   ID                                     Name                                            Type       Subnet CIDR                    Primary Address   Speed   SecAddr   SecGrps   FloatIPs   Created          Status   Resource Group   
   xx12x345-6xxx-7x89-123x-4x5xxx678x9x   great-scott-stride-lilac-captivate-filtrate              my-subnet(1234x12x-.)          172.16.1.4        100     -         -         {...}      10 seconds ago   -   
   ```
   {:screen}

4. Request a floating IP address to associate to your instance. 

   ```
   $ ibmcloud is floating-ip-reserve \
       my-floatingip \
       --nic xx12x345-6xxx-7x89-123x-4x5xxx678x9x
   ```
   {:codeblock}

   For this example, you'd see the following responses.
   ```
     Address          xxx.xxx.xxx.xxx   
     Name             my-floatingip   
     Target           great-scott-stride-lilac-captivate-filtrate(xx12x345-.)   
     Target Type      intf   
     Target IP        172.16.1.4   
     Created          1 second ago   
     Status           available   
     Zone             us-south-1   
     Resource Group   -   
     Tags             -  
    ```
    {:screen}

    Remember the floating IP address for later.  

Need more help? You can always run `ibmcloud is help` when needed.
{: tip}

Do you prefer to create an instance using the {{site.data.keyword.cloud_notm}} console? For more information, see [Creating an instance](vsi_is_create_instance.html).
{: tip}

## What happens next
A series of emails are sent to your administrator: acknowledgment of the virtual server instance order, order approval and processing, and a message stating the instance is created.

After the server is created, you can [connect to your instance](vsi_is_connecting_linux_gc.html).
