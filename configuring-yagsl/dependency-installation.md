# Dependency Installation

## WPILib

{% embed url="https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/wpilib-setup.html" %}

## REV Hardware Client

{% embed url="https://docs.revrobotics.com/rev-hardware-client/gs/install" %}

## CTRE Tuner X

{% embed url="https://v6.docs.ctr-electronics.com/en/latest/docs/installation/installation-frc.html" %}

## Vendor URLs

Bellow is a list of URL's that are required to be installed for YAGSL to work properly, these are publicisized on the vendor websites and over YAGSL documentation. To install them follow this guide on WPILib docs.

{% embed url="https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#installing-libraries" %}

| Vendor         | URL                                                                                                                        | Offline Installation                                                                                       |
| -------------- | -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| CTRE Phoenix 6 | `unavailable`                                                                                                              | [Tuner X](https://v6.docs.ctr-electronics.com/en/latest/docs/installation/installation-frc.html)           |
| CTRE Phoenix 5 | `unavailable`                                                                                                              | [Tuner X](https://v6.docs.ctr-electronics.com/en/latest/docs/installation/installation-frc.html)           |
| REVLib         | [`https://software-metadata.revrobotics.com/REVLib-2024.json`](https://software-metadata.revrobotics.com/REVLib-2024.json) | [REV Hardware Client](https://github.com/REVrobotics/REV-Software-Binaries/releases/tag/rhc-1.6.2) Offline |
| Studica (NavX) | [`https://dev.studica.com/releases/2024/NavX.json`](https://dev.studica.com/releases/2024/NavX.json)                       | `unavailable`                                                                                              |
| ReduxLib       | `unavailable`                                                                                                              | `unavailable`                                                                                              |

All of these vendordeps must be installed for YAGSL to work due to the generic nature of YAGSL even if you do not use any of the products the vendor library supports.
