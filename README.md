# Using Scanless for Anonymous Port Scanning

## Lab Objective
Learn how to use Scanless to scan a target through online port scanners, so the target sees the scanner’s IP address instead of yours.

## Lab Purpose
Scanless is a Python script that sends port scan requests through third-party online scanners. The target sees the scanner service’s IP, not yours. Each scanner checks a different set of ports, so running several gives a fuller picture. Note that this is IP masking, not full anonymity: the scanner service still sees your IP and your request.

## Lab Tool
- Kali Linux

## Lab Topology
You can use Kali Linux in a VM for this lab. It needs internet access.

---

## Lab Walkthrough

### Task 1: Install Scanless

Scanless doesn’t need root, so work as your normal user. First update your package list and install pipx, which installs Python tools in isolated environments (current Kali blocks plain pip install with an “externally-managed-environment” error):

```bash
sudo apt update
sudo apt install pipx -y
pipx ensurepath
```

Close the terminal and open a new one so the PATH change takes effect. Then install Scanless:

```bash
pipx install scanless
```

Confirm it installed:

```bash
scanless -h
```

If you get “command not found”, close and reopen the terminal, or run:

```bash
source ~/.bashrc
```

### Task 2: Explore the Tool

View the help screen:

```bash
scanless -h
```

List the available scanners and their websites:

```bash
scanless -l
```

Write down the scanner names. The list can vary by version, and some sites may be offline.

### Task 3: Scan a Target with Individual Scanners

The format is:

```bash
scanless -t [target hostname or IP] -s [scanner name]
```

Only scan systems you own or have written permission from the owner to test. You can use scanme.nmap.org, which the Nmap project provides for scan practice.

Run a scan with each of these scanners:

```bash
scanless -t scanme.nmap.org -s hackertarget
scanless -t scanme.nmap.org -s spiderip
scanless -t scanme.nmap.org -s t1shopper
scanless -t scanme.nmap.org -s ipfingerprints
```

Each scanner prioritises different ports, so the results will differ. The ipfingerprints scanner also estimates the target’s operating system and gives confidence percentages.

If a scanner returns an error or times out, note it and move on. These are third-party sites and may be down.

And you can see from the image above that some scanners were unavailable or returned unusable output because they depend on third-party websites that change. That’s a real limitation of the tool

### Task 4: Scan with All Scanners

To run every available scanner against the target:

```bash
scanless -t scanme.nmap.org -a
```

This takes longer, and some scanners may fail.

Scroll down, you’ll see what’s on the image below

### Task 5: Compare with Nmap

Run a direct scan and compare it with your Scanless results:

```bash
nmap scanme.nmap.org
```

Note that Nmap shows your IP to the target, while Scanless shows the scanner’s IP. Check which open ports Nmap found that some of the online scanners missed.

---

## Conclusion

This lab explores how Scanless uses third-party online scanners to perform port scanning, how different scanners can produce different results, and how their findings compare with a direct Nmap scan.
