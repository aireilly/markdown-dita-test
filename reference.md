# About root device hints {.reference}

The `rootDeviceHints` parameter enables the installer to provision the {op-system-first} image to a particular device. The installer examines the devices in the order it discovers them, and compares the discovered values with the hint values. The installer uses the first discovered device that matches the hint value. The configuration can combine multiple hints, but a device must match all hints for the installer to select it.

**Table 1. Subfields**

|Subfield|Description|
|------|------|
|`deviceName`|A string containing a Linux device name such as `/dev/vda` or `/dev/disk/by-path/`. It is recommended to use the `/dev/disk/by-path/<device_path>` link to the storage location. The hint must match the actual value exactly.|
|`hctl`|A string containing a SCSI bus address like `0:0:0:0`. The hint must match the actual value exactly.|
|`model`|A string containing a vendor-specific device identifier. The hint can be a substring of the actual value.|
|`vendor`|A string containing the name of the vendor or manufacturer of the device. The hint can be a sub-string of the actual value.|
|`serialNumber`|A string containing the device serial number. The hint must match the actual value exactly.|
|`minSizeGigabytes`|An integer representing the minimum size of the device in gigabytes.|
|`wwn`|A string containing the unique storage identifier. The hint must match the actual value exactly.
If you use the `udevadm` command to retrieve the `wwn` value, and the command outputs a value for `ID_WWN_WITH_EXTENSION`, then you must use this value to specify the `wwn` subfield.|
|`rotational`|A boolean indicating whether the device should be a rotating disk (true) or not (false).|

Example usage:

```yaml
- name: master-0
  role: master
  rootDeviceHints:
    deviceName: "/dev/sda"
```
