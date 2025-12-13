# 1.0.0 (2025-12-13)


### Bug Fixes

* corrects the formatting of the LICENSE file to include a new line at the end ([27e1388](https://github.com/Diaszano/actions/commit/27e13889eaadbced9dfdadc55abc090a7dee432e))
* corrects the Gitleaks file extension from .yml to .yaml in workflows ([a09984a](https://github.com/Diaszano/actions/commit/a09984aeb60a7077f3220c8cddc8bbb9aa3ec670))
* remove the commit verification dependency for the pre-commit task in the workflow ([85d5be8](https://github.com/Diaszano/actions/commit/85d5be8525f11ea15e5b8311a10cdf92de9e421f))
* remove token secrets from pre-commit job in check workflow ([f08ebbe](https://github.com/Diaszano/actions/commit/f08ebbed0136cc12a0a29a60b03bb151618c9eaa))
* remove unnecessary secrets inheritance from commit-check workflow ([0ac6d3d](https://github.com/Diaszano/actions/commit/0ac6d3ddfd3d74b6bac61bb7e1cc54e18dab4a29))
* update workflow path to use the correct commit-check YAML file ([89e283b](https://github.com/Diaszano/actions/commit/89e283b898b7621cf83ff42acf42552ddc6a2ed2))


### Features

* adds a pre-commit workflow for executing hooks ([78f4565](https://github.com/Diaszano/actions/commit/78f45654dd59f3f46ae1860f4c930dbe196cd795))
* adds a reusable workflow for validating commit messages and naming branches ([240bac6](https://github.com/Diaszano/actions/commit/240bac6fb02f4da3dac6ee175013f1aaeb348a55))
* adds a workflow file for verifying commits in pull requests ([98eb08d](https://github.com/Diaszano/actions/commit/98eb08d18ff435a208e136925713d59b46351174))
* adds configuration for automatic release with semantic-release ([fc9f5ad](https://github.com/Diaszano/actions/commit/fc9f5ad8a4bc2dccfd5edb927c067375c79eb104))
* adds configuration for PR comments in the commit verification job ([28c1c17](https://github.com/Diaszano/actions/commit/28c1c1716d9ed3ba8f0ba4c18e6e35b9a4a2be1e))
* adds Gitleaks workflow for secret verification in pull requests ([0b76bc7](https://github.com/Diaszano/actions/commit/0b76bc7c17cfd00c4d4b4ec9756a42cd28ba527f))
* adds permissions for verification and release workflows ([21ed372](https://github.com/Diaszano/actions/commit/21ed3729e5bbb3de06beae19a6c86119dd01fdac))
* adds workflow for release version. ([35aab5a](https://github.com/Diaszano/actions/commit/35aab5a09434b01203ec444593e591f82c1c1000))
* adds workflow for validating commits and branches ([7c0f253](https://github.com/Diaszano/actions/commit/7c0f2539637d625287345fbff9b835a5da27ca8b))
