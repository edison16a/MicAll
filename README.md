# MicAll
Here’s the updated script with detailed comments and a GitHub-style description for documentation.

### Script with Comments

```python
import os
import subprocess

# Path to TCC Plus executable
TCCPLUS_PATH = os.path.expanduser("~/Downloads/tccplus")

# Directory containing macOS applications
APPLICATIONS_DIR = "/Applications"

def get_bundle_identifier(app_path):
    """
    Get the bundle identifier of a macOS application.
    
    Args:
        app_path (str): Full path to the .app file.

    Returns:
        str: The bundle identifier of the app, or None if it cannot be retrieved.
    """
    try:
        output = subprocess.check_output(
            ["osascript", "-e", f'id of app "{app_path}"'],
            stderr=subprocess.DEVNULL
        )
        return output.decode("utf-8").strip()
    except subprocess.CalledProcessError:
        return None

def grant_permissions(bundle_id):
    """
    Grant microphone and camera permissions to an app using TCC Plus.

    Args:
        bundle_id (str): The bundle identifier of the app.
    """
    if not bundle_id:
        return
    try:
        # Grant microphone access
        subprocess.run([TCCPLUS_PATH, "add", "Microphone", bundle_id], check=True)
        print(f"Microphone access granted for: {bundle_id}")
        # Grant camera access
        subprocess.run([TCCPLUS_PATH, "add", "Camera", bundle_id], check=True)
        print(f"Camera access granted for: {bundle_id}")
    except subprocess.CalledProcessError as e:
        print(f"Failed to grant permissions for: {bundle_id}\nError: {e}")

def main():
    """
    Main function to iterate through all apps in the Applications directory 
    and grant microphone and camera permissions using TCC Plus.
    """
    # Ensure TCC Plus is available
    if not os.path.exists(TCCPLUS_PATH):
        print(f"TCC Plus not found at {TCCPLUS_PATH}. Please download and set it up first.")
        return

    # Iterate through applications in the Applications directory
    for item in os.listdir(APPLICATIONS_DIR):
        app_path = os.path.join(APPLICATIONS_DIR, item)
        if app_path.endswith(".app"):
            bundle_id = get_bundle_identifier(app_path)
            if bundle_id:
                print(f"Processing {item} with bundle ID: {bundle_id}")
                grant_permissions(bundle_id)
            else:
                print(f"Could not retrieve bundle ID for: {item}")

if __name__ == "__main__":
    main()
```

---

### GitHub Description

# **Grant macOS Microphone and Camera Permissions Script**

This Python script automates the process of granting **microphone** and **camera** permissions to all applications in your `/Applications` directory on macOS using [TCC Plus](https://github.com/jslegendre/tccplus). It retrieves each application's bundle identifier and applies the required permissions seamlessly.

---

## **Features**
- Scans the `/Applications` directory for installed `.app` files.
- Retrieves bundle identifiers using AppleScript.
- Grants both **Microphone** and **Camera** permissions using `tccplus`.
- Logs the status of each operation for easy tracking.

---

## **Requirements**
1. **macOS** with `/Applications` directory populated.
2. **Python 3.8+** installed.
3. **TCC Plus**:  
   Download from [TCC Plus GitHub](https://github.com/jslegendre/tccplus/releases/tag/1.0).  
   Save it to your `~/Downloads/` directory and make it executable:
   ```bash
   cd ~/Downloads
   chmod +x tccplus
   ```

---

## **Installation**
1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/mac-permissions-grant.git
   cd mac-permissions-grant
   ```
2. Ensure TCC Plus is downloaded and executable.

---

## **Usage**
1. Run the script:
   ```bash
   python3 grant_permissions.py
   ```
2. The script will:
   - Scan `/Applications` for `.app` files.
   - Retrieve their bundle identifiers.
   - Grant microphone and camera permissions for each app.

---

## **Example**
For an app like Discord:
- **Bundle Identifier**: `com.hnc.Discord`
- Permissions Added:
  - Microphone: `./tccplus add Microphone com.hnc.Discord`
  - Camera: `./tccplus add Camera com.hnc.Discord`

The script automates this process for all `.app` files in `/Applications`.

---

## **Disclaimer**
Granting permissions to all apps may expose sensitive data. Use cautiously and review apps before running the script.

---

Let me know if you need help setting up or customizing this further! 😊
