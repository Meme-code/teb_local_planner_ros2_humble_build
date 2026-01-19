# TEB Local Planner ROS2 Humble - ARM64 Deb Builder

GitHub Actions workflow to build `teb_local_planner` ROS2 package and release `.deb` packages for ARM64/aarch64.

## Usage

### Manual Build

1. Go to **Actions** tab
2. Select **Build and Release teb_local_planner**
3. Click **Run workflow**
4. Enter version (e.g., `1.0.0`)

### Tag-based Release

```bash
git tag v1.0.0
git push origin v1.0.0
```

This automatically triggers a build and creates a GitHub release.

## Installation

Download the `.deb` from Releases, then:

```bash
sudo dpkg -i ros-humble-teb-local-planner_*.deb
sudo apt-get install -f  # Install any missing dependencies
```

## What's Built

- `teb_local_planner` - Timed Elastic Band local planner
- `costmap_converter` - Required dependency
