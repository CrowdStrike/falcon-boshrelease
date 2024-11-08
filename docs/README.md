# CrowdStrike Falcon Bosh Release

The CrowdStrike Falcon Bosh Release leverages [Cloud Foundry Bosh](https://bosh.io/docs/) as an [Add-on](https://bosh.io/docs/runtime-config/#addons) to facilitate the installation of the Falcon Sensor on each Virtual Machine (VM) in the Tanzu cluster through the Falcon APIs. By default, the installer deployed via Bosh will handle the installation, registration of the sensor, and initiation of the service. Because the CrowdStrike Falcon Bosh Release functions as a [Bosh Add-on](https://bosh.io/docs/runtime-config/#addons), it does not require a stemcell to be used or configured either by Bosh or in the VMWare Tanzu Operations Manager Stemcell Library because a [Bosh Add-on](https://bosh.io/docs/runtime-config/#addons) runs as a co-located job in each of the already deployed VMs. In other words, no CrowdStrike VM is created that runs in its own Stemcell; instead, CrowdStrike is installed via your deployed applications Stemcells via an install script.

## Requirements

To successfully deploy the CrowdStrike Falcon Bosh Release, ensure the following prerequisites are met:

- **Access to Broadcom Portal**: You must have access to download necessary files from the [Broadcom Support portal](https://support.broadcom.com/group/ecx/productdownloads?subfamily=CrowdStrike%20Falcon%20for%20VMware%20Tanzu.
- **VMWare Tanzu Operations Manager**: Ensure you have VMWare Tanzu Operations Manager installed.
- **VMWare Tanzu Application Service Version**: The minimum supported version of VMWare Tanzu Application Service (TAS) is 2.11.
- **Supported Stemcells**: Currently, all Ubuntu and Windows Stemcells are supported.
- **Proxy Configuration**: If your environment uses a proxy, it must be configured to allow the VMs to communicate with CrowdStrike's cloud. For detailed information, refer to the [Falcon Sensor for Linux System Requirements](https://falcon.crowdstrike.com/documentation/page/edd7717e/falcon-sensor-for-linux-system-requirements#l0523dcd) and/or the [Falcon Sensor for Windows Deployment](https://falcon.crowdstrike.com/documentation/page/ecc97e75/falcon-sensor-for-windows-deployment#e43dade4).

## Current Limitations

> [!IMPORTANT]
> Since the Bosh job operates as an Addon rather than a deployment, uninstalling the sensor, including the `.deb` package, when the tile is uninstalled, is currently unsupported.

## Falcon API Permissions

API clients are granted specific API scopes that allow access to certain CrowdStrike APIs and define the actions that an API client can perform. Ensure the following API scopes are enabled for proper functionality:

> [!IMPORTANT]
> - **Sensor Download** [read]: This scope is essential for downloading the sensor.
> - **Sensor Update Policies** [read] (optional): This scope is required if you plan to configure the `Sensor Update Policy` via the VMWare Ops Manager UI.
> - **Installation Tokens** [read] (optional): This scope is required if you plan on using installation tokens in your environment and want the installer to automatically attempt to discover them instead of manually adding it in the Bosh tile.

## Installation

Follow these steps to install CrowdStrike Falcon in your VMWare Tanzu Application Service (TAS) environment:

1. **Download the Tile**: Obtain the CrowdStrike Falcon tile from [Broadcom's support portal](https://support.broadcom.com).

2. **Import the Tile**: Import the downloaded tile into VMWare Operations Manager.

    ![VMWare Operations Manager Dashboard](images/import.png)

3. **Add the Tile to the Dashboard**: Navigate to the VMWare Operations Manager dashboard and click the `+` button to add the `CrowdStrike Falcon for VMWare Tanzu` tile.

    ![VMWare Operations Manager Dashboard](images/add.png)

4. **Configure the Tile**: Click on the `CrowdStrike Falcon for VMWare Tanzu` product tile to begin configuring the deployment of the Bosh Addon.

    ![CrowdStrike Falcon for VMWare Tanzu](images/tile.png)

5. **API Configuration**: Enter the required OAuth Client ID and Secret key pair generated from the CrowdStrike Falcon console. By default, the Falcon cloud region associated with your OAuth Client ID and Secret will be auto-discovered. If necessary, you can manually configure the Falcon Cloud region. Click `Save` when done.

    ![CrowdStrike Falcon for VMWare Tanzu API configuration](images/api.png)

6. **Optional Sensor Configuration**: If additional sensor configuration is needed, you can set the following optional settings:
   - **Falcon Customer ID (CID)**: Enter your Falcon Customer ID.
   - **Sensor Proxy Settings**: Enable or disable the sensor proxy as required.
   - **Proxy Host Configuration**: Specify the proxy host for the sensor to communicate with the Falcon cloud.
   - **Proxy Port Configuration**: Specify the proxy port for the sensor to communicate with the Falcon cloud.
   - **Provisioning Token**: Enter the provisioning token for the sensor.
   - **Provisioning Wait Time**: Enter the provisioning wait time for the sensor. (Windows only)
   - **Tags**: Enter the provisioning token for the sensor. Any sensor tags
   - **Sensor Update Policy**: The name of the sensor update policy for the installer to get the sensor version to install specified by the sensor update policy.

    ![CrowdStrike Falcon for VMWare Tanzu Sensor configuration](images/sensor.png)

   Once any of the optional sensor configurations have been set, click `Save`.

7. **Review Pending Changes**: In the VMWare Operations Manager Dashboard, click the `Review Pending Changes` button. This will display a list of all the changes that are queued for deployment. Ensure that the checkbox next to the `CrowdStrike Falcon for VMWare Tanzu` tile is selected. This step is crucial to confirm that the deployment process will include the Falcon sensor on each of the VMs.

8. **Apply Changes**: After ensuring that the , click the `Apply Changes` button in the VMWare Operations Manager. This action will start the deployment process of the CrowdStrike Falcon Bosh Addon to your VMs. Monitor the progress to ensure that the deployment completes successfully.

## Troubleshooting

If you encounter any issues during the deployment process, the following logs will be generated on the VMs:

- **`pre-start.stdout.log`**: This log file captures the standard output of the pre-start script.
- **`pre-start.stderr.log`**: This log file captures the standard error output of the pre-start script.
- **`falcon-installer.log`**: This log file contains detailed information about the Falcon sensor installation process. It includes messages about the progress of the installation, any errors encountered, and other relevant details.

These logs can be found under `/var/vcap/sys/log/falcon-linux-sensor/` for Linux VMs and `C:\var\vcap\sys\log\falcon-windows-sensor` for Windows VMs.

In addition to the VM logs, please also provide the following Tanzu Operations Manager information:

- The `crowdstrike-falcon-cf` product metadata at https://tanzuops.manager/debug/product_metadata replacing `tanzuops.manager` with the address of your Tanzu Operations Manager.
- The `crowdstrike-falcon-cf` runtime configuration at https://tanzuops.manager/debug/files replacing `tanzuops.manager` with the address of your Tanzu Operations Manager.

Review these logs for failures as to why the installation and deployment failed. When contacting CrowdStrike support or creating a GitHub, these logs should be provided.

> [!IMPORTANT]
> When providing logs, please make sure to sanitize the output and do not provide any credentials.

## Frequently Asked Questions (FAQs)

### When the CrowdStrike Tanzu tile is uninstalled, will the CrowdStrike sensor also uninstall?

Currently uninstalling the sensor, including the `.deb` package, when the tile is uninstalled, is currently unsupported.

### Does CrowdStrike use Stemcells for deployment and is the Stemcell version configurable via the Stemcell Library?

At this point in time for the 1.x versions, Stemcells are not used at all for deploying the CrowdStrike sensor to your running VMs. This means setting any Stemcell version in the Stemcell Library does nothing since we do not use Stemcells for deployment. Currently, the CrowdStrike Bosh Tile only is a Bosh Addon which runs as a job co-located on each of the existing VMs in your Tanzu TAS deployment. This means that you can leave the CrowdStrike Falcon listing in the Stemcell library without configuring a Stemcell version.

### How do I upgrade from version 1.1.0 to a later version?

Currently, upgrading from version 1.1.0 requires a reinstall of the entire tile. When the tile is reinstalled, the sensor will not be uninstalled.
