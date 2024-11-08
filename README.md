# CrowdStrike Falcon Bosh Release

This project provides a Bosh release and Cloud Foundry tile for installing CrowdStrike Falcon as a [Bosh Add-on](https://bosh.io/docs/runtime-config/#addons) in VMWare Tanzu Application Service (TAS) Virtual Machines (VMs).

## Getting Started

> [!WARNING]
> Only deploying with VMWare Tanzu Operations Manager for VMWare Tanzu Application Service (TAS) Bosh-based installs is supported.

To install the CrowdStrike Sensor via VMware Tanzu Operations Manager, follow these steps:

1. **Download the Tile**: Obtain the `crowdstrike-falcon-cf-VERSION.pivotal` file (where `VERSION` is the desired tile installation version) from [Broadcom's Support portal](https://support.broadcom.com/group/ecx/productdownloads?subfamily=CrowdStrike%20Falcon%20for%20VMware%20Tanzu).
2. **Import the Tile**: Import the downloaded tile into VMware Tanzu Operations Manager.
3. **Add the Tile to the Dashboard**: Add the `CrowdStrike Falcon for VMWare Tanzu` tile to the Installation Dashboard.
4. **Configure the Tile**: Enter the OAuth Client ID and Secret.
5. **Apply Changes**: Apply the changes to deploy the sensor to all VMs in your VMWare TAS environment.

## Installation

For detailed instructions on installing the sensor using VMware Tanzu Operations Manager, refer to the [Installation Guide](docs/README.md).

## Questions?

Have questions? Please see the [Q&A section in Discussions](https://github.com/CrowdStrike/falcon-boshrelease/discussions/categories/q-a). Open a new [Q&A discussion](https://github.com/CrowdStrike/falcon-boshrelease/discussions/new?category=q-a) if your question(s) has not already been asked and answered.

## Troubleshooting

If you encounter any issues during the installation process, refer to the [troubleshooting section](docs/README.md#troubleshooting) for assistance.

## Contributing

We welcome contributions that improve the installation, uninstallation, and distribution processes of the Falcon Sensor on VMWare Tanzu with Bosh. Please ensure that your contributions align with our coding standards and pass all CI/CD checks.

## Support

The CrowdStrike Falcon Bosh Release is a community-driven, open source project designed to streamline the deployment and use of the CrowdStrike Falcon sensor on VMWare Tanzu Bosh systems. While not a formal CrowdStrike product, CrowdStrike Falcon Bosh Release is maintained by CrowdStrike and supported in partnership with the open source developer community.

For additional support, please refer to the [SUPPORT.md](SUPPORT.md) file.

## License

See [LICENSE](LICENSE)
