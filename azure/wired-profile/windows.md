# Windows

## Configurations Steps

To deploy a wired authentication profile, a custom XML is required. RADIUSaaS generates this XML for you

1. Download the XML from [here](../../portal/settings-trusted-roots/xml.md#wired).
2. Create a custom profile in Microsoft Intune for the LAN profile using the following settings (see [Use custom settings for Windows 10 devices in Intune](https://docs.microsoft.com/en-us/intune/custom-settings-windows-10)). In **Custom OMA-URI Settings**, select **Add**, and then enter the following values:
   * Name: _Modern Workplace-Windows 10 LAN Profile_
   * Description: Enter a description that gives an overview of the setting, and any other important details.
   * OMA-URI (case sensitive): Enter _./Device/Vendor/MSFT/WiredNetwork/LanXML_
   * Data type: select **String (XML file)**.
   * Custom XML: Upload the exported XML file.
