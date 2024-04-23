---
description: >-
  There are different method to transfer fsmo, i think using command is the best
  and easy method to transfer fsmo.
---

# Transfer FSMO roles

#### FSMO consist of 5 roles&#x20;

* Schema Master
* Domain Naming Master
* PDC
* RID pool manager
* Infrastructure Master

## Getting Super Powers

Open CMD as Administratror:

```
regsvr32 schmmgmt.dll
```

&#x20;Type **ntdsutil** and press Enter.\
&#x20;Type **roles** and press Enter.\
&#x20;Type **connections** and press Enter.\
&#x20;Type **connect to server DC01** and press Enter, where DC01 is the server computer name that will transfer the FSMO roles to.\
&#x20;Type **quit** and press Enter

Next, we will transfer FSMO roles one by one with the corresponding command, as the case may be. After each Enter appears a confirmation window. Just click **Yes** to continue.

For **Schema Master**, type **transfer schema master** and press Enter.\
&#x20;For **RID Master**, type **transfer rid master** and press Enter.\
&#x20;For **Domain Naming Master**, type **transfer naming master** and press Enter.\
&#x20;For **PDC Emulator**, type **transfer pdc** and press Enter.\
&#x20;For **Infrastructure Master**, type **transfer infrastructure master** and press Enter.







