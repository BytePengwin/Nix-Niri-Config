My NixOS ConfigurationThis repository contains my personal NixOS configuration, managed with Nix Flakes.Directory Structureflake.nix: The entry point for the entire configuration. It defines the inputs (Nixpkgs, Home-Manager, etc.) and outputs (the final system configuration).configuration.nix: The main system-wide configuration. This is where you configure hardware, networking, system services like GDM, and users.home.nix: The user-specific configuration, managed by Home-Manager. This handles your personal packages, shell aliases, and dotfiles.config/: A directory to store the actual configuration files (dotfiles) for applications like Niri and Waybar.How to InstallThis assumes you have already booted into a NixOS installer and have an internet connection.Partition and Format Drives: Use gparted, cfdisk, or another tool to partition your disks. Format them with appropriate filesystems (e.g., ext4 for root, FAT32 for EFI).Mount Filesystems: Mount your root partition to /mnt and your EFI partition to /mnt/boot.# Example for a simple setup:
mount /dev/disk/by-label/nixos /mnt
mkdir -p /mnt/boot
mount /dev/disk/by-label/boot /mnt/boot
Generate Base Config (to get hardware-configuration.nix):nixos-generate-config --root /mnt
We only want the hardware-configuration.nix file from this.Clone This Repository: Replace the generated configuration with this repository.# Remove the generated config files (but keep the hardware one!)
rm /mnt/etc/nixos/configuration.nix

# Clone your repository
git clone https://your-git-repo-url/nixos-config.git /mnt/etc/nixos
Important: After cloning, you might need to copy the hardware-configuration.nix file back if it was deleted.Edit Files: Before installing, you must review and edit the files, especially configuration.nix and home.nix, to set your correct username, hostname, and timezone. Also, copy the contents of /mnt/etc/nixos/hardware-configuration.nix generated in step 3 into the configuration.nix file where indicated.Install NixOS: Run the installation using the flake. The #nixos at the end refers to the output name defined in flake.nix.nixos-install --flake /mnt/etc/nixos#nixos
Reboot and enjoy your new system!How to UpdateAfter booting into your new system, you can update it by pulling changes from your git repository.Navigate to your config directory:cd /etc/nixos
Pull the latest changes:git pull
Apply the new configuration:sudo nixos-rebuild switch --flake .#nixos
This command will build the new configuration and make it the current boot default.
