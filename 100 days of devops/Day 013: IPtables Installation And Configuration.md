# Task

We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 8082 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:





1. Install iptables and all its dependencies on each app host.



2. Block incoming port 8082 on all apps for everyone except for LBR host.



3. Make sure the rules remain, even after system reboot.
-----

# Securing App Servers with iptables

This document outlines the process for installing and configuring `iptables` on the Nautilus application servers (`stapp01`, `stapp02`, `stapp03`) to enhance security by restricting access to a specific port.

-----

## 🎯 Objective

The goal is to secure the application servers by installing a firewall (`iptables`) and creating rules to block all incoming traffic on port `8082`, except for traffic originating from the Load Balancer (LBR) host.

### Requirements

1.  Install `iptables` and its dependencies on all three app servers.
2.  Block incoming connections on port `8082` for all sources **except** the LBR host (`172.16.238.14`).
3.  Ensure the firewall rules are persistent and survive a system reboot.

-----

## 🛠️ Solution Steps

The following steps must be performed on **each** of the three app servers (`stapp01`, `stapp02`, `stapp03`).

### 1\. Install and Start iptables

First, install the `iptables-services` package, which allows `iptables` to be managed as a system service.

```bash
# Install the package
sudo yum install iptables-services -y

# Start the iptables service
sudo systemctl start iptables

# Enable the service to start on boot
sudo systemctl enable iptables
```

### 2\. Configure Firewall Rules

The order of these rules is critical. We must first `ACCEPT` traffic from the LBR and then `DROP` all other traffic for the target port.

```bash
# Rule 1: Allow traffic from the LBR host to port 8082.
# The -I INPUT 1 flag inserts this rule at the top of the chain.
sudo iptables -I INPUT 1 -p tcp -s 172.16.238.14 --dport 8082 -j ACCEPT

# Rule 2: Drop all other traffic destined for port 8082.
# The -A INPUT flag appends this rule to the end of the chain.
sudo iptables -A INPUT -p tcp --dport 8082 -j DROP
```

### 3\. Save the Rules

To make the rules persistent across reboots, save the current configuration.

```bash
# Save the current iptables rules
sudo service iptables save
```

This command writes the rules to `/etc/sysconfig/iptables`.

-----

## ✅ Verification

To confirm that your rules are correctly configured, you can list them with line numbers.

```bash
sudo iptables -L INPUT -v -n --line-numbers
```

The output should show the `ACCEPT` rule for the LBR host (`172.16.238.14`) appearing before any general `DROP` or `REJECT` rules for port `8082`.
