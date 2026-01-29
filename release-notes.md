# Wrong Way Controller Software Release Notes

## v5.3.17

### Added
- File-backed logging for detections – Detection events are now persisted to disk for improved data retention and analysis
- Added INFO level logging for wrong-way event lifecycle and dispatch decisions for better troubleshooting

### Changed
- Application Log flush interval reduced – Changed from 120 seconds to 30 seconds to significantly improve data persistence during unexpected power loss
- Timestamp storage optimization – Switched from formatted strings to integer microseconds for better memory efficiency
- Log retention management – Adjusted cutoff to maintain a maximum of 7 log files at once
- Enhanced online/offline detection – Improved state tracking by monitoring speed data timestamps in meshnet and radar modules

### Fixed
- Alert queueing behavior – Camera detections no longer queue while an alert is already active, preventing redundant alerts
- Flasher consistency – Flashers now activate reliably for wrong-way driver alerts
- FLIR camera exception handling – Added error handling for camera queries and lease monitoring to prevent application crashes
- Log rotation ordering bug – Fixed file sorting to properly use filename-based dates instead of filesystem timestamps
- Disk growth prevention – Removed forced file logging for fallback, restoration, and offline device data to reduce unnecessary disk writes

## v5.3.16


### Added
- Log persistence with disk-backed storage – Application logs are now persisted to disk for improved reliability and data retention
- Serial number display – Added serial number visibility on the status page for easier device identification

### Changed
- N/A

### Fixed
- Improved camera preview logic with better error handling

## v5.3.15

### Added
- Add a “Test Wrong Way Event” button to the Advanced page to simulate a wrong way event. This already existed within the controller’s API, but this adds an additional way to access it.

### Changed

### Fixed
- Improve sensor WebSocket reconnection after a disconnection.
- Improve calculated time between zones to ensure that a confirmation event is captured. Previously, this would only be noticed with slower moving objects with the sensor zones far apart from each other when using the sensor API version of software

## v5.3.14

### Added
- Hardware Input Control (Inputs 1–3): Each digital input can now be individually enabled or disabled.
  - Input 3 serves as a manual test trigger for the flashers so operators can verify system response without generating a real event.
  - A matching “Trigger Flashers” button is also available on the Advanced page for software‑initiated testing.
- Full System Factory Reset: Restore the entire system to default settings in a single action. You can choose whether to preserve existing network settings or reset them as well, reducing downtime in controlled recovery scenarios.
- Configuration Import & Export:
  - Export a configuration snapshot.
  - Import a prior configuration and automatically apply network changes when needed.
  - Clear feedback when network settings change and are applied.
- Unique Video Recording Filenames: All recordings now use guaranteed‑unique timestamps to prevent overwrites during rapid event bursts.

### Changed
- Smoother application of network changes with fewer unnecessary restarts.
- Removed some useless logs at the DEBUG level.
- Updated UI framework (patch-level) for minor visual and accessibility refinements.

### Fixed
- Removed the logging of reported events from a confirmation sensor that occurs outside of the appropriate timing window, as they were already being ignored for event triggers.
- Resolved rare cases where rapid event bursts could lead to duplicated or missing event imagery and, in some instances, filename collisions for recordings.
- Resolved rare case where manually setting the time or setting an NTP server would prevent the system from responding to additional web/API requests.

## v5.3.13

### Added
- Automatic NTP configuration for FLIR cameras – Cameras now automatically sync with local router time for consistent timestamping
- Zone 2 validation – Added validation to require `agreement_behavior` in Zone 2 configuration updates

### Changed
- WebSocket connection management – Refactored WebSocket handling for indefinite reconnection attempts and proper shutdown behavior
- WebSocket reconnection timing – Enhanced connection handling to be time-based with lengthened reconnection attempt intervals
- Collaborator network ID handling – Improved validation and consistency in zone settings

### Fixed
- WebSocket cleanup after network reconnections – Resolved continuous ping timeout warnings that occurred after network reconnections
- WebSocket closed state detection – Improved closed state checking for better reconnection handling
- Watchdog re-enabling – Fixed issues preventing watchdog from being re-enabled after stopping
- Time synchronization – Local time now syncs to router time after NTP settings update with appropriate delay

## v5.3.12

### Added
- N/A

### Changed
- N/A

### Fixed
- FDOT protocol event handling – Fixed race conditions causing errors in FDOT protocol event handling
- Video data handling – Refactored event dispatcher to ensure protocol data is generated successfully and prevent FDOT event data from being overwritten on confirmation events
