# Meraki RadSec authentication fails after PureCA migration

#### Symptoms

* Meraki access points using RadSec suddenly fail to authenticate clients
* Nothing was changed in the RADIUSaaS configuration

#### Cause

Meraki is moving RadSec device certificates from the Keeper Organization CA to its new [PureCA service](https://documentation.meraki.com/Wireless/Design_and_Configure/Architecture_and_Best_Practices/Cisco_Keeper_to_PureCA_Migration_Strategy). Access points with a PureCA-issued certificate are rejected until the PureCA chain is trusted in RADIUSaaS.

Meraki lists MR 33.1.2 as the minimum firmware for PureCA, but we have also seen access points on earlier firmware presenting PureCA certificates.

#### Solution

Add the PureCA chain to your RADIUSaaS Trusted Certificates as described in [RadSec PureCA certificates](https://docs.radiusaas.com/configuration/access-point-setup/radsec-available/meraki#radsec-pureca-certificates). Keep your Keeper CA trusted as well. Both can be trusted in parallel.

If authentication still fails afterwards, check your configuration against the [Meraki RadSec setup guide](https://docs.radiusaas.com/configuration/access-point-setup/radsec-available/meraki) or contact our support. For questions about certificates or firmware on your Meraki devices, contact Meraki support.
