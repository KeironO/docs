# Synology NAS Maintenance Documentation

## Storage Health Maintenance Procedures

This documentation covers essential maintenance procedures for ensuring data integrity and drive health on Synology NAS systems.

## S.M.A.R.T. Test Configuration

### Purpose
S.M.A.R.T. (Self-Monitoring, Analysis, and Reporting Technology) tests evaluate the physical health of hard drives to detect potential failures before they occur.

### Scheduling Daily S.M.A.R.T. Tests
1. Log in to your Synology DSM (DiskStation Manager) via web browser
2. Open the "Storage Manager" application
3. Click on the "HDD/SSD" tab
4. Select the "S.M.A.R.T. Test" option from the left menu
5. Click the "Schedule" button
6. Click "Create" to set up a new test schedule
7. Configure your daily test:
   - Select "Daily" for the frequency
   - Choose your preferred time (ideally during low-usage periods)
   - Select which drives to test
   - Choose the test type:
     - Quick Test (~10 minutes): Suitable for daily checks
     - Extended Test (several hours): More thorough but time-consuming
8. Click "OK" to save your schedule

### Best Practices
- Schedule quick tests daily and extended tests monthly
- Set tests to run during periods of low NAS usage
- Review S.M.A.R.T. test results regularly in the Storage Manager interface
- Investigate any warnings or errors immediately

## Data Scrubbing Configuration

### Purpose
Data scrubbing is a maintenance process that scans storage volumes to detect and repair data inconsistencies, preventing silent data corruption and "bit rot".

### Benefits
- Identifies and repairs corrupted data blocks
- Verifies data integrity through checksum validation
- Prevents gradual data degradation
- Maintains reliability of RAID/SHR volumes over time

### Setting Up Data Scrubbing Schedule
1. Log in to your Synology DSM (DiskStation Manager) via web browser
2. Open the "Storage Manager" application
3. Click on the "Storage Pool" tab
4. Select the storage pool you want to configure
5. Click on the "Configure" button (or "Action" > "Configure")
6. Select "Data Scrubbing" from the menu
7. Check the box for "Enable data scrubbing schedule"
8. Set your preferred schedule:
   - Choose the frequency (monthly recommended for home use)
   - Set the start date
   - Set the time during expected low system usage
9. Click "OK" or "Apply" to save your settings

### Execution Time Expectations
For a typical 2-drive setup with 4TB drives per drive (8TB total storage):
- First scrubbing operation: 4-8 hours
- Subsequent operations: 3-6 hours (may vary based on data volume)
- Factors affecting duration:
  - Drive speed and connection type
  - System load during scrubbing
  - Volume of data stored
  - RAID configuration
  - Drive health and age

### Monitoring Scrubbing Progress
1. Go to Storage Manager > Storage Pool
2. Check the status indicator next to your storage pool
3. Click on the storage pool to view detailed progress
4. Review completion reports for any detected and repaired errors

### Best Practices
- Schedule scrubbing monthly for home use, weekly for critical data
- Run scrubbing operations during periods of low NAS usage
- Monitor the first scrubbing operation to establish a baseline duration
- Review reports after completion to identify potential issues
- Perform scrubbing more frequently on older storage systems

## Maintenance Schedule Summary

| Maintenance Task | Recommended Frequency | Duration | Best Time to Run |
|-----------------|----------------------|----------|-----------------|
| S.M.A.R.T. Quick Test | Daily | ~10 minutes | Late night |
| S.M.A.R.T. Extended Test | Monthly | 1-3 hours per drive | Weekend, low usage |
| Data Scrubbing | Monthly | 4-8 hours | Weekend, low usage |

## Troubleshooting

### Failed S.M.A.R.T. Tests
1. Check the error details in Storage Manager
2. Verify drive connections
3. Run a manual test to confirm results
4. Consider backing up data and replacing the drive if failures persist

### Issues During Data Scrubbing
1. If scrubbing detects errors but repairs them, monitor for recurring problems
2. If scrubbing fails to complete, check system logs for detailed error messages
3. Consider running a different maintenance task (e.g., S.M.A.R.T. extended test) to cross-verify drive health

### Preventing Data Loss
1. Maintain regular backups separate from your NAS
2. Configure email notifications for storage events
3. Replace drives approaching their end-of-life or showing repeated errors
4. Monitor system logs periodically for warning signs

## Additional Resources
- Synology Knowledge Base: https://kb.synology.com
- Synology Support: https://www.synology.com/support