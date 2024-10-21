# scanstreets
Scan for open cameras to locate wanted individuals.
This is a basic script approach for scanning for open cameras. 
!!! PLEASE NOTE: THIS PROJECT IS IN PROGRESS AND NOT FOR USE AGAINST THE GENERAL PUBLIC !!!
Accessing any cameras without permission is illegal and can violate privacy laws. It's important to ensure that you have the necessary permissions to access or scan these devices.

Here’s a simple script that checks for open camera streams on common ports. This script will scan a specific IP range for devices that might be serving video streams, focusing on well-known camera ports.

 ### Python Script to Scan for Open Cameras
import requests
import socket
from requests.exceptions import RequestException

# Common ports used by IP cameras
camera_ports = [80, 8080, 554, 443]

def is_camera(url):
    """Check if a URL returns a camera stream."""
    try:
        response = requests.get(url, timeout=2)
        if response.status_code == 200:
            content_type = response.headers.get('Content-Type', '')
            return 'video' in content_type or 'image' in content_type
    except RequestException:
        return False
    return False

def scan_ip_range(start_ip, end_ip):
    """Scan a range of IP addresses for open cameras."""
    open_cameras = []

    # Convert IP addresses to numerical format
    start = int(socket.inet_aton(start_ip).hex, 16)
    end = int(socket.inet_aton(end_ip).hex, 16)

    for ip_int in range(start, end + 1):
        ip = socket.inet_ntoa(int.to_bytes(ip_int, 4, 'big'))
        for port in camera_ports:
            url = f'http://{ip}:{port}'
            if is_camera(url):
                print(f"Open camera found: {url}")
                open_cameras.append(url)

    return open_cameras

if __name__ == "__main__":
    # Define the IP range to scan
    start_ip = '192.168.1.1'  # Change to your starting IP
    end_ip = '192.168.1.255'   # Change to your ending IP

    print("Scanning for open cameras...")
    found_cameras = scan_ip_range(start_ip, end_ip)

    print("\nScan complete.")
    print("Found cameras:")
    for camera in found_cameras:
        print(camera)
```

### Instructions

1. **Install Required Libraries**:
   Make sure you have the `requests` library installed:
   ```bash
   pip install requests
   ```

2. **Adjust IP Range**:
   Modify the `start_ip` and `end_ip` variables to specify the range you want to scan.

3. **Run the Script**:
   Execute the script:
   ```bash
   python scanstreets.py
   ```

### Important Notes

- **Ethics and Legality**: Always ensure you have permission to scan networks or devices. Scanning without permission is illegal in many jurisdictions.
- **Use Responsibly**: Scanning large ranges or unauthorized networks can be considered a malicious activity. You can limit scans to your own network or networks where you have explicit permission.
- **Camera Protocols**: This script primarily checks for HTTP streams; many cameras may use different protocols. You might need to adapt the script for various camera types or access methods.
