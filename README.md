✅ **Conclusion: Findings and Forward Strategy**

This project served as an initial deep-dive into host-based telemetry using Splunk and Windows Security logs. The focus was on validating log ingestion, establishing behavioral baselines, and understanding how system processes and user activity are reflected in Security EventCodes — particularly 4624 (logon) and 4688 (process creation).

**🔍 What We Accomplished**
We completed three critical phases of host telemetry analysis:

**Section 2: Log Ingest Validation**
Confirmed that logs are successfully flowing into Splunk from the host system.

Verified that key EventCodes (4624, 4688, 4672, 4625) were present and parsed correctly.

Observed that over 65,000 log events were generated in a single day — despite minimal user activity — due to detailed audit policies and Splunk’s own internal operations.

**Section 3: Baselining Logon and Process Behavior**
Identified normal system users (Steve, SYSTEM, and the computer account).

Noted that most 4624 logons were system-driven and that user Steve logged in only once, yet multiple entries were generated due to background handling.

Cataloged 4688-based process creation patterns showing Splunk as the dominant generator (splunk-optimize.exe, splunkd.exe, etc.).

Classified process telemetry into Interactive, System, and Other categories, confirming that startup services and background processes made up the vast majority of execution activity.

**Section 4: Process Creation Frequency and Lineage**
Aggregated the most frequent process executions.

Mapped parent-child process relationships to verify trusted lineage (e.g., svchost.exe spawning expected services).

Searched for low-frequency, potentially anomalous processes — but none were found due to the host’s limited activity.

⚠️ **What We Discovered**
The project revealed a fundamental inefficiency in the current configuration:

Excessive log volume is being generated automatically, dominated by:

System services (e.g., svchost.exe)

Audit-intensive events (e.g., process starts/stops)

Splunk's own internal operations

Low signal-to-noise ratio for manual or analyst-driven activity

Difficulty separating background noise from meaningful behavior without significant filtering and interpretation

🛠️ **Why Configuration Changes Are Now Required**
The project proved that while verbose logging can be educational, this level of data volume is not sustainable for efficient, repeatable analysis. Therefore, in future projects, we will:

Streamline Windows audit policies to reduce verbosity (e.g., suppress event codes like 4689 for process termination).

Focus logging on high-value signals, including:

Interactive logons (Logon_Type=2 or 10)

Script execution (e.g., suspicious PowerShell, cmd.exe, or python.exe)

Scheduled task registration or registry changes

Reduce internal telemetry from Splunk where possible, for cleaner baselining.

🧭 **Final Value of This Project**

Although no abnormal behavior was found, the real value of this effort lies in what it taught:

Detection engineering begins not with finding threats, but with learning how to collect the right data.

By grappling with raw, noisy telemetry and establishing what “normal” looks like, we have set a solid foundation for:

Building high-fidelity detection logic

Designing cleaner triage playbooks

Optimizing future audit configurations to better balance signal clarity with data volume

Next steps will include documenting revised audit configurations and re-running streamlined versions of this analysis to compare efficiency, signal density, and investigative clarity.
