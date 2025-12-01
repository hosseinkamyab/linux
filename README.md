# Installing Linux on ASUS N552VX: A Personal Journey

I recently attempted to install Linux on my ASUS N552VX laptop. The process was a bit of a rollercoaster, with different distributions and boot issues, but eventually, I managed to get Debian running smoothly. Here’s a step-by-step overview of what happened:

## 1. Initial Attempt – Debian
- I first tried installing **Debian** on my laptop.
- After installation, Debian would only run in **safe mode**, and normal boot was not working.

## 2. Switching to Ubuntu
- Hoping for an easier experience, I decided to try **Ubuntu**.
- Unfortunately, I **couldn’t even start the Ubuntu installation**, which was frustrating.

## 3. Returning to Debian
- I returned to **Debian** after the Ubuntu attempt failed.
- I modified the **GRUB settings** for normal boot to match the safe mode configuration.  
  To make it work properly, I added the option `dis_ucode_ldr` to the GRUB configuration, which is the same setting originally used in recovery mode.  
  More details can be found [here](/fixing_normal_boot.md).


- After this adjustment, Debian finally **ran normally** on my ASUS N552VX.

## 4. Outcome
- Debian is now fully functional on my laptop.
- The process did not result in the loss of any important data.


