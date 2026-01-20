# ROS2 Package Builder - Multi-Architecture Deb Releases

GitHub Actions workflow to build ROS2 packages and release `.deb` packages for multiple architectures (AMD64 and ARM64 with CPU-specific optimizations).

## Supported Packages

Packages are defined in `.github/packages.yml`:

| Package | Description |
|---------|-------------|
| `teb` | TEB Local Planner - Timed Elastic Band approach for local trajectory planning |
| `mppi` | Nav2 MPPI Controller - Model Predictive Path Integral controller |
| `costmap-converter` | Costmap Converter - Converts costmap_2d into geometric primitives |

## ARM64 CPU Targets

Each package can be built for multiple ARM64 CPU targets with optimized compiler flags:

| Target | CPU Flags | Deb Suffix |
|--------|-----------|------------|
| `generic` | `-march=armv8-a -O3` | (none) |
| `cortex-a53` | `-mcpu=cortex-a53 -O3` | `-a53` |
| `cortex-a72` | `-mcpu=cortex-a72 -O3` | `-a72` |
| `cortex-a76` | `-mcpu=cortex-a76 -O3` | `-a76` |

## Usage

### Manual Build

1. Go to **Actions** tab
2. Select **Build and Release ROS2 Packages**
3. Click **Run workflow**
4. Select package (e.g., `teb`)
5. Enter version (e.g., `1.0.0`)

### Output Files

Building `teb` version `1.0.0` produces:

```
ros-humble-teb-local-planner_1.0.0_amd64.deb       # x86_64
ros-humble-teb-local-planner_1.0.0_arm64.deb       # Generic ARMv8-A
ros-humble-teb-local-planner-a53_1.0.0_arm64.deb   # Cortex-A53 optimized
ros-humble-teb-local-planner-a72_1.0.0_arm64.deb   # Cortex-A72 optimized
```

## Installation

Download the appropriate `.deb` from Releases:

```bash
# For x86_64
sudo dpkg -i ros-humble-teb-local-planner_*_amd64.deb

# For ARM64 (choose based on your CPU)
sudo dpkg -i ros-humble-teb-local-planner_*_arm64.deb      # Generic
sudo dpkg -i ros-humble-teb-local-planner-a53_*_arm64.deb  # Raspberry Pi 3
sudo dpkg -i ros-humble-teb-local-planner-a72_*_arm64.deb  # Raspberry Pi 4/5, Jetson

# Install any missing dependencies
sudo apt-get install -f
```

## Adding New Packages

Edit `.github/packages.yml`:

```yaml
packages:
  my-package:
    deb_name: my-ros-package
    description: |
      My custom ROS2 package
    repositories:
      - url: https://github.com/user/repo.git
        branch: main
    install_dirs:
      - my_package
    dependencies:
      - ros-humble-ros-base
    arm64_targets:
      - generic
      - cortex-a72
```

Then add the package name to the workflow's `options` list in `.github/workflows/build-ros-package.yml`.

## Adding New ARM64 Targets

Edit `.github/packages.yml`:

```yaml
arm64_targets:
  cortex-a55:
    cpu_flags: "-mcpu=cortex-a55 -O3"
    suffix: "-a55"
```

Then add it to the package's `arm64_targets` list.
