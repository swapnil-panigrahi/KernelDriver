# Kernel-Mode Memory Manipulation Driver

This is a Windows kernel-mode driver designed to facilitate reading from and writing to the memory of a target process. The driver provides basic functionality to attach to a process, read memory from it, and write memory to it.

## Features

- Attach to a process by its process ID.
- Read memory from the attached process.
- Write memory to the attached process.

## Requirements

- Windows Driver Kit (WDK)
- Visual Studio with the WDK integration

## Getting Started

### Building the Driver

1. **Clone the repository**:
    ```sh
    git clone https://github.com/yourusername/kernel-memory-driver.git
    cd kernel-memory-driver
    ```

2. **Open the project in Visual Studio**:
   - Ensure you have the WDK installed and integrated with Visual Studio.
   - Open the solution file (`.sln`) in Visual Studio.

3. **Build the project**:
   - Select the appropriate configuration (`Debug` or `Release`).
   - Build the project to generate the driver binary (`.sys` file).

### Installing the Driver

1. **Copy the driver to your target system**.

2. **Install the driver**:
    - Use an administrator command prompt to install the driver:
    ```sh
    sc create KDriver type= kernel binPath= "C:\path\to\your\driver.sys"
    ```

3. **Start the driver**:
    ```sh
    sc start KDriver
    ```

### Uninstalling the Driver

1. **Stop the driver**:
    ```sh
    sc stop KDriver
    ```

2. **Delete the driver**:
    ```sh
    sc delete KDriver
    ```

## Usage

### IOCTL Codes

The driver supports the following IOCTL codes for interaction:

- **Attach to Process**:
  - IOCTL Code: `CTL_CODE(FILE_DEVICE_UNKNOWN, 0x696, METHOD_BUFFERED, FILE_SPECIAL_ACCESS)`
- **Read Memory**:
  - IOCTL Code: `CTL_CODE(FILE_DEVICE_UNKNOWN, 0x697, METHOD_BUFFERED, FILE_SPECIAL_ACCESS)`
- **Write Memory**:
  - IOCTL Code: `CTL_CODE(FILE_DEVICE_UNKNOWN, 0x698, METHOD_BUFFERED, FILE_SPECIAL_ACCESS)`

### Example Requests

#### Structure for Requests
```cpp
struct Request {
    HANDLE process_id;   // Target process ID
    PVOID target;        // Target address in the process memory
    PVOID buffer;        // Buffer for reading/writing data
    SIZE_T size;         // Size of the buffer
    SIZE_T return_size;  // Size of data actually read/written
};
```
### Attach to a Process

To attach to a process, send an IOCTL request with the `process_id` filled in the `Request` structure.

### Read Memory

To read memory from the attached process, fill the `target`, `buffer`, and `size` fields in the `Request` structure and send an IOCTL request.

### Write Memory

To write memory to the attached process, fill the `target`, `buffer`, and `size` fields in the `Request` structure and send an IOCTL request.
