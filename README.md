# Mystery CTF Write-Up

## Challenge Scenario

In this CTF, your task is to follow a series of clues and use various tools to extract a hidden flag. This write-up explains each step in detail to help you navigate through the challenge and successfully capture the flag.

---

### Step 1: QR Code Scan

1. **Scan the QR Code** provided at the beginning of the challenge.
2. The QR code leads to a Pastebin link containing a **locked Paste**.

### Step 2: Accessing the Pastebin File

1. After trying several guesses, you find that the **password** is `"securinets"` (based on the CTF theme).
2. Unlocking the Pastebin file reveals the following information:
   - A ![image](https://github.com/user-attachments/assets/dd217a04-ccf7-4d0d-a92c-896a6cdc4745)
   - An **link to a Google Drive file**: [Drive Link](https://drive.google.com/file/d/1MP6h-GpRFRje12IYaK4Mum6clyUC31mn/view?usp=drivesdk)
   - Ann **email address**: `kharrato@gmail.com`

### Step 3: Download the Capture File

1. Download the capture file from the Google Drive link.
2. **Open the capture file in Wireshark** for network traffic analysis.

### Step 4: Analyzing Network Traffic in Wireshark

1. In Wireshark, to analyze the captured TCP communication:
   - Go to **`Analyze > Follow > TCP Stream`**.
2. You will discover the **server IP address: `102.207.250.8`** and a hint about SSH access.
3. The analysis reveals the **password: `"securinets"`**.

### Step 5: Scanning the Server with Nmap

1. Use the `nmap` tool to scan the server and discover open ports:

   ```bash
   nmap -v -sT 102.207.250.8
   ```

2. The scan results indicate that **port 22 (SSH)** is open, allowing SSH access to the server.

### Step 6: Logging into the Server via SSH

1. With the username `"omar"` (found in Wireshark), initiate an SSH connection:

   ```bash
   ssh omar@102.207.250.8
   ```

2. Enter the **password: `"securinets"`** to gain access to the server.

### Step 7: Exploring the Server

1. Once logged in, use the `ls -l` command to list files in the directory:

   ```bash
   ls -l
   ```

2. You find an interesting file: `image.jpg`.

### Step 8: Analyzing `image.jpg` for Steganography

1. To examine the file for hidden information, run the following tools in sequence:

   - **strings**: Searches for readable text within the file.
   - **exiftool**: Checks for metadata that may contain hidden messages.
   - **xxd**: Converts the file to a hex dump to search for embedded data.

2. **Steghide**: Use `steghide` to extract hidden data within `image.jpg`:

   ```bash
   steghide extract -sf image.jpg
   ```

3. When prompted for a **passphrase**, leave it **empty** and press Enter. This reveals the hidden flag.

### Conclusion

After extracting data from `image.jpg`, you retrieve the **flag** and complete the CTF challenge!

