---
description: 'error: you need to load the kernel first'
---

# bzImage has invalid signature

Even with a successful chain **shim -> grub** displaying GRUB menu you can see the `invalid signature` error when you are trying to load a custom kernel.

![](<../.gitbook/assets/2021-09-21 11-44-06.JPG>)

### The reason

Since the most recent GRUB2 update (2.02+dfsg1 in Ubuntu), GRUB2 does not load unsigned kernels anymore
