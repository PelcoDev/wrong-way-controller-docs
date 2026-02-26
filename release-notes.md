# Wrong Way Controller Software Release Notes

## v5.3.19 - 2/26/2026

### Added {#added-v5319}

- Time jump detection: The system now detects and gracefully handles wall-clock time jumps (e.g. NTP corrections, DST changes) during monitoring, preventing stale state.
- Startup version logging: The application version and release date are now logged at startup, making it easier to confirm what is deployed from log files.
- Input gating for thermal camera: Thermal camera usage is now causes the corresponding input to be gated.
- APN configuration logging: Cellular connection checks now log APN configuration details to aid in network diagnostics.

### Changed {#changed-v5319}

- Monotonic timers throughout: Timer implementations across all subsystems have been migrated to monotonic time sources, improving reliability in environments where the system clock may change.
- RTSP stream handling improvements: FFmpeg restart logic has been hardened and stale stream detection added, reducing camera downtime after stream failures.
- Thread safety improvements: Input state is now protected by a lock in ADC, mesh network, and detection processing paths, reducing the risk of race conditions.
- Video processing refactor: FPS handling and remuxing logic have been encapsulated and streamlined for maintainability.
- Protocol handler timeouts: Timeout values for SafePath and FDOT protocol handlers have been increased to better accommodate slower or high-latency connections.
- FLIR camera NTP configuration: NTP config updates now attempt a GET before POST, to prevent unnecessary updates.

### Fixed {#fixed-v5319}

- Radar offline status check: The radar error string logic now correctly accounts for the offline radar status, preventing missed or misleading error reports.

## v5.3.18 - 2/17/2026

### Added {#added-v5318}

- Added date range selection capability for downloading detection data, allowing users to export specific time periods
- Detection log cleanup mechanism to automatically remove files prior to baseline retention dates
- Implemented periodic 5-minute revalidation for runtime camera setting changes, allowing settings to be updated without requiring a system restart

### Changed {#changed-v5318}

- Improved detection event processing to maintain receive-order consistency with external detection queue
- Enhanced time handling in detection log reading and download functions for more accurate data retrieval
- Renamed "log" references to "application logs" throughout the interface for better clarity and consistency

>#### FDOT Protocol Enhancements

- Improved event handling with adaptive frame spacing and GOP metadata detection for more reliable image capture
- Updated image processing to dynamically adjust buffer duration based on camera capabilities

### Fixed {#fixed-v5318}

>#### FDOT Protocol

- Timestamp handling in FDOT update events (renamed `alertTimestamp` to `updateTimestamp` to match the API spec)
- Cleanup now occurs for excess processed images when multiple alert zone events occur before a confirmation zone event.

## v5.3.17 - 1/29/2026

### Added {#added-v5317}

- File-backed logging for detections – Detection events are now persisted to disk for improved data retention and analysis
- Added INFO level logging for wrong-way event lifecycle and dispatch decisions for better troubleshooting

### Changed {#changed-v5317}

- Application Log flush interval reduced – Changed from 120 seconds to 30 seconds to significantly improve data persistence during unexpected power loss
- Timestamp storage optimization – Switched from formatted strings to integer microseconds for better memory efficiency
- Log retention management – Adjusted cutoff to maintain a maximum of 7 log files at once
- Enhanced online/offline detection – Improved state tracking by monitoring speed data timestamps in meshnet and radar modules

### Fixed {#fixed-v5317}

- Alert queueing behavior – Camera detections no longer queue while an alert is already active, preventing redundant alerts
- Flasher consistency – Flashers now activate reliably for wrong-way driver alerts
- FLIR camera exception handling – Added error handling for camera queries and lease monitoring to prevent application crashes
- Log rotation ordering bug – Fixed file sorting to properly use filename-based dates instead of filesystem timestamps
- Disk growth prevention – Removed forced file logging for fallback, restoration, and offline device data to reduce unnecessary disk writes

## v5.3.16 - 12/10/2025

### Added {#added-v5316}

- Log persistence with disk-backed storage – Application logs are now persisted to disk for improved reliability and data retention
- Serial number display – Added serial number visibility on the status page for easier device identification

### Changed {#changed-v5316}

- N/A

### Fixed {#fixed-v5316}

- Improved camera preview logic with better error handling

## v5.3.15 - 10/22/2025

### Added {#added-v5315}

- Add a “Test Wrong Way Event” button to the Advanced page to simulate a wrong way event. This already existed within the controller’s API, but this adds an additional way to access it.

### Changed {#changed-v5315}

### Fixed {#fixed-v5315}

- Improve sensor WebSocket reconnection after a disconnection.
- Improve calculated time between zones to ensure that a confirmation event is captured. Previously, this would only be noticed with slower moving objects with the sensor zones far apart from each other when using the sensor API version of software

## v5.3.14 - 10/9/2025

### Added {#added-v5314}

- Hardware Input Control (Inputs 1–3): Each digital input can now be individually enabled or disabled.
  - Input 3 serves as a manual test trigger for the flashers so operators can verify system response without generating a real event.
  - A matching “Trigger Flashers” button is also available on the Advanced page for software‑initiated testing.
- Full System Factory Reset: Restore the entire system to default settings in a single action. You can choose whether to preserve existing network settings or reset them as well, reducing downtime in controlled recovery scenarios.
- Configuration Import & Export:
  - Export a configuration snapshot.
  - Import a prior configuration and automatically apply network changes when needed.
  - Clear feedback when network settings change and are applied.
- Unique Video Recording Filenames: All recordings now use guaranteed‑unique timestamps to prevent overwrites during rapid event bursts.

### Changed {#changed-v5314}

- Smoother application of network changes with fewer unnecessary restarts.
- Removed some useless logs at the DEBUG level.
- Updated UI framework (patch-level) for minor visual and accessibility refinements.

### Fixed {#fixed-v5314}

- Removed the logging of reported events from a confirmation sensor that occurs outside of the appropriate timing window, as they were already being ignored for event triggers.
- Resolved rare cases where rapid event bursts could lead to duplicated or missing event imagery and, in some instances, filename collisions for recordings.
- Resolved rare case where manually setting the time or setting an NTP server would prevent the system from responding to additional web/API requests.

## v5.3.13 - 9/23/2025

### Added {#added-v5313}

- Automatic NTP configuration for FLIR cameras – Cameras now automatically sync with local router time for consistent timestamping
- Zone 2 validation – Added validation to require `agreement_behavior` in Zone 2 configuration updates

### Changed {#changed-v5313}

- WebSocket connection management – Refactored WebSocket handling for indefinite reconnection attempts and proper shutdown behavior
- WebSocket reconnection timing – Enhanced connection handling to be time-based with lengthened reconnection attempt intervals
- Collaborator network ID handling – Improved validation and consistency in zone settings

### Fixed {#fixed-v5313}

- WebSocket cleanup after network reconnections – Resolved continuous ping timeout warnings that occurred after network reconnections
- WebSocket closed state detection – Improved closed state checking for better reconnection handling
- Watchdog re-enabling – Fixed issues preventing watchdog from being re-enabled after stopping
- Time synchronization – Local time now syncs to router time after NTP settings update with appropriate delay

## v5.3.12 - 9/4/2025

### Added {#added-v5312}

- N/A

### Changed {#changed-v5312}

- N/A

### Fixed {#fixed-v5312}

- FDOT protocol event handling – Fixed race conditions causing errors in FDOT protocol event handling
- Video data handling – Refactored event dispatcher to ensure protocol data is generated successfully and prevent FDOT event data from being overwritten on confirmation events
