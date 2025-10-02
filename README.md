# Download Speed Monitor

A Python Terminal UI application that monitors download speed and connection statistics in real-time using Cloudflare's speed test endpoint.

![Download Speed Monitor](https://img.shields.io/badge/Python-3.10+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

## Features

- 🚀 **Automated Testing**: Runs speed tests every 5 minutes automatically
- 📊 **Real-time Plots**: Beautiful terminal-based graphs using plotext
- ⏱️ **Detailed Metrics**: Tracks multiple connection timing statistics
- 📈 **Historical Data**: Keeps up to 50 data points for trend analysis
- 💻 **Terminal UI**: Works entirely in your terminal, no browser needed

## Metrics Tracked

The application monitors the following statistics:

1. **Download Speed** (Mbps) - Actual data transfer rate
2. **Name Lookup Time** (ms) - DNS resolution time
3. **Connect Time** (ms) - TCP connection establishment
4. **Pre-transfer Time** (ms) - SSL/TLS handshake completion
5. **Start Transfer Time** (ms) - Time to first byte (TTFB)
6. **Total Time** (ms) - Complete download duration

## Installation

### Prerequisites

- Python 3.7 or higher
- pip (Python package manager)

### Install Dependencies

```bash
pip install plotext pycurl
```

**Note**: On some systems, you may need to install libcurl development files:

**Ubuntu/Debian:**
```bash
sudo apt-get install libcurl4-openssl-dev libssl-dev
pip install pycurl
```

**macOS:**
```bash
brew install curl
pip install pycurl
```

**Windows:**
```bash
pip install pycurl
```

## Usage

### Basic Usage

Run the monitor with default settings (10 MB test size, 5-minute intervals):

```bash
python speed_monitor.py
```

### Customization

Edit the `main()` function in `speed_monitor.py` to customize:

```python
monitor = SpeedMonitor(
    test_bytes=10_000_000,  # Test size in bytes (10 MB)
    interval=300,            # Seconds between tests (5 minutes)
    max_points=50            # Maximum data points to keep
)
```

**Examples:**

- **Faster testing** (every 1 minute with 5 MB):
  ```python
  monitor = SpeedMonitor(test_bytes=5_000_000, interval=60)
  ```

- **Larger test size** (50 MB every 10 minutes):
  ```python
  monitor = SpeedMonitor(test_bytes=50_000_000, interval=600)
  ```

- **Keep more history** (100 data points):
  ```python
  monitor = SpeedMonitor(max_points=100)
  ```

## Output

The application displays:

### 1. Real-time Plots
Two terminal-based graphs:
- **Download Speed Over Time**: Shows speed in Mbps
- **Connection Timing Statistics**: Shows all timing metrics in milliseconds

### 2. Statistics Dashboard
```
================================================================================
                       DOWNLOAD SPEED STATISTICS                              
================================================================================
Last Updated: 2025-10-02 14:30:45
Total Tests: 12

Current Speed: 85.42 Mbps
Average Speed: 82.15 Mbps
Maximum Speed: 91.23 Mbps
Minimum Speed: 75.80 Mbps

Current Total Time: 934.56 ms
Average Total Time: 971.23 ms
================================================================================
Next test in 300 seconds (5 minutes)
Press Ctrl+C to stop monitoring
================================================================================
```

## Stopping the Monitor

Press `Ctrl+C` to gracefully stop the monitoring process.

## How It Works

1. **Speed Test**: Downloads a specified number of bytes from Cloudflare's speed test endpoint
2. **Timing Collection**: Uses pycurl to capture detailed connection timing metrics
3. **Data Storage**: Maintains a rolling window of measurements in memory
4. **Visualization**: Updates terminal plots after each test
5. **Loop**: Waits for the specified interval before running the next test

## Use Cases

- **Network Monitoring**: Track internet connection stability over time
- **ISP Performance**: Monitor if your ISP delivers consistent speeds
- **Troubleshooting**: Identify patterns in connection issues
- **Remote Servers**: Monitor network performance of remote systems
- **DevOps**: Include in monitoring dashboards for server connectivity

## Technical Details

### Libraries Used

- **plotext**: Terminal-based plotting library for real-time graphs
- **pycurl**: Python interface to libcurl for precise timing measurements
- **threading**: Background monitoring without blocking the main thread

### API Endpoint

The application uses Cloudflare's speed test endpoint:
```
https://speed.cloudflare.com/__down?bytes=<number_of_bytes>
```

This endpoint returns random data of the specified size, perfect for bandwidth testing.

## Troubleshooting

### Issue: "ImportError: No module named 'pycurl'"
**Solution**: Install pycurl with system dependencies (see Installation section)

### Issue: Plots not displaying correctly
**Solution**: Ensure your terminal supports ANSI colors and has sufficient width (80+ columns recommended)

### Issue: Connection timeouts
**Solution**: Check your internet connection or increase the timeout value in the code:
```python
c.setopt(c.TIMEOUT, 120)  # Increase to 120 seconds
```

## License

MIT License - feel free to use and modify as needed.

## Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest new features
- Submit pull requests
- Improve documentation

## Acknowledgments

- Cloudflare for providing the speed test endpoint
- plotext library for terminal plotting capabilities
- pycurl for precise timing measurements

---

**Happy Monitoring! 📊**
